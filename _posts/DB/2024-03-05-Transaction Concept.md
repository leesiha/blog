---
title: "2-1. Transaction Concept"
date: 2024-03-05 14:36:00 +0900
# layout: splash
toc: true
---

# Transaction Concept

Transaction이란 무엇인가?

Transaction이란 데이터베이스에서 데이터를 처리하는 **(논리적인) 작업 단위**를 말합니다. 이는 데이터베이스의 일관성을 유지하면서 데이터를 안전하게 처리하기 위한 중요한 개념입니다.

```sql
start transaction;  	// start a new transaction
select @orderNumber := max(orderNumber) from orders;	//get largest order number
set @orderNumber = @orderNumber  + 1;  // set new order number

// insert a new order for customer 145      
insert into orders(orderNumber, orderDate, requiredDate, shippedDate, status, customerNumber)      
values(@orderNumber, now(), date_add(now(), INTERVAL 5 DAY), date_add(now(), INTERVAL 2 DAY), 'In Process', 145);       

// insert 2 order line items      
insert into orderdetails(orderNumber, productCode, quantityOrdered, priceEach, orderLineNumber)      
values(@orderNumber,'S18_1749', 30, '136', 1);      
insert into orderdetails(orderNumber, productCode, quantityOrdered, priceEach, orderLineNumber)      
values(@orderNumber,'S18_2248', 50, '55.09', 2); 

commit;  // commit changes  
  
// get the new inserted order      
select * from orders a inner join orderdetails b on a.ordernumber = b.ordernumber      
where a.ordernumber = @ordernumber;
```

# Transaction Management

## Dealing with Issues

There are two main issues to address when working with **transactions**:

1. **Hardware/Software/Transaction Failures**: These can occur due to various reasons such as power outages, system crashes, or errors within the transaction itself. Robust error handling and recovery procedures need to be in place to ensure the integrity of the database.
    - So, a Transaction should have the ACID properties.
    Transaction이 지켜야 하는 4가지 조건
        
        ### ACID properties
        
        트랜잭션은 데이터베이스에서 일부 데이터를 읽고(**read**) 쓰는(**write**) 일련의 과정으로 이루어지며, 이러한 작업들은 원자적으로 실행되어야 합니다. 즉, 모든 작업이 성공적으로 완료되거나 실패하는 것을 보장하여 데이터베이스를 일관된 상태로 유지합니다.
        
        ### Atomicity
        
        - **원자성은 트랜잭션 내의 모든 작업이 하나의 원자처럼 동작해야 함을 의미합니다.** 즉, 트랜잭션 내의 모든 연산이 성공적으로 수행되어야 트랜잭션이 성공적으로 완료되며, 어떤 연산이라도 실패하면 트랜잭션 전체가 롤백되어 이전 상태로 복구됩니다.
        - 예를 들어, 여러 개의 쿼리가 하나의 트랜잭션으로 묶여 있을 때, 하나의 쿼리가 실패하면 다른 쿼리들도 취소되어 일관된 상태를 유지합니다.
        
        ### Consistency
        
        - **일관성은 트랜잭션이 실행되기 전과 후에 데이터베이스의 일관성이 유지되어야 함을 의미합니다.** 트랜잭션은 데이터베이스를 일관된 상태로 변환해야 하며, 정의된 규칙과 제약 조건을 준수해야 합니다.
        - 예를 들어, 특정 제약 조건이나 비즈니스 규칙이 있다면, 트랜잭션이 이를 준수하고 일관된 상태로 데이터베이스를 변환해야 합니다.
        
        트랜잭션은 일련의 데이터베이스 작업을 논리적 단위로 묶어 원자성, 일관성, 고립성, 지속성(ACID)을 보장합니다. DBMS는 트랜잭션을 관리하여 데이터베이스의 일관성을 유지합니다.
        
        ### Isolation
        
        독립성(Isolation)은 **다른 트랜잭션의 연산에 영향을 받지 않고 독립적으로 실행되어야 한다**는 원칙을 의미합니다. 이는 **하나의 트랜잭션이 다른 트랜잭션이 동시에 수행되더라도 서로 간섭하지 않고 독립적으로 실행되어야 함**을 의미합니다. 즉, 동시에 실행되는 여러 트랜잭션 간에 데이터베이스의 일관성을 유지하면서 데이터 충돌을 방지하는 것을 보장합니다.
        
        - **고립성은 여러 트랜잭션이 동시에 실행될 때, 각 트랜잭션이 다른 트랜잭션에게 영향을 미치지 않도록 하는 정도를 나타냅니다.** 다시 말해, 하나의 트랜잭션이 실행 중일 때 그 결과를 다른 트랜잭션이 볼 수 없어야 합니다.
        - 예를 들어, 트랜잭션 A가 데이터를 변경 중일 때, 이 변경 내용이 트랜잭션 B에게 보이지 않고, B의 변경 내용도 A에게 보이지 않아야 합니다.
        
        ### Durability
        
        지속성(Durability)은 **트랜잭션이 성공적으로 완료된 후에는 그 결과가 영구적으로 데이터베이스에 반영되어야 한다**는 원칙을 의미합니다. 이는 **트랜잭션 완료 후에 시스템 장애가 발생하더라도 그 결과는 손실되지 않고 유지**되어야 함을 의미합니다. 즉, 트랜잭션의 변화가 영구적인 것을 보장합니다.
        
        - **지속성은 트랜잭션이 성공적으로 완료되면 해당 변경 내용이 영구적으로 저장되어야 함을 의미합니다.** 즉, 시스템이 다운되거나 장애가 발생하더라도 트랜잭션의 결과가 보존되어야 합니다.
        - 예를 들어, 트랜잭션이 커밋되면 데이터베이스 시스템은 해당 변경을 디스크에 기록하여 지속성을 보장합니다. 이후에 시스템 장애가 발생하더라도 이러한 로그를 사용하여 데이터베이스를 복구할 수 있습니다.
        
        ## Transaction State
        
        ![Untitled](/assets/images/2-1_Transaction_Concept/Untitled.png)
        
        ### Active
        
        "Active"는 트랜잭션의 상태 중 하나로, 트랜잭션이 현재 실행 중이며 아직 완료되지 않았음을 나타냅니다. 이 상태에서 트랜잭션은 계속해서 데이터베이스에 대한 연산을 수행할 수 있습니다.
        
        ### Partially committed
        
        "Partially committed"는 트랜잭션의 상태 중 하나로, **트랜잭션의 모든 연산이 수행되고 커밋 명령이 나오기 직전의 상태**를 말합니다. 이 상태에서는 아직 최종 커밋이 이루어지지 않았으므로, 에러가 발생하면 롤백이 가능합니다.
        
        ### Committed
        
        "Committed"는 트랜잭션의 상태 중 하나로, 트랜잭션의 모든 연산이 성공적으로 완료되고 데이터베이스에 영구적으로 반영된 상태를 말합니다. 이 상태가 되면 트랜잭션은 완료된 것으로 간주되며, 이후에는 롤백(취소)이 불가능합니다.
        
        ### Aborted
        
        "Aborted"는 트랜잭션의 상태로, 트랜잭션이 실패하거나 롤백(취소)된 경우에 발생하는 상태를 말합니다. 이 상태에서는 트랜잭션이 수행되던 중에 발생한 모든 변경이 취소되고, 데이터베이스는 트랜잭션 시작 전 상태로 복구됩니다.
        
2. **Concurrent Execution of Multiple Transactions**: When multiple transactions are executed concurrently, conflicts can occur. Techniques like locking, timestamping, or using a concurrency control protocol can help manage these conflicts and maintain consistency in the database.
    - Then, how should we run concurrently in the system? 
    (나중에 배우는 내용. 아래의 내용들은 내가 추측한 해결방법)
        1. **Locking**: 데이터에 대한 동시 액세스를 제한하여 충돌을 방지합니다.
        2. **Timestamping**: 각 트랜잭션에 시간스탬프를 부여하여 충돌을 방지합니다.
        3. **Concurrency Control Protocol**: 이 프로토콜을 사용하면 여러 트랜잭션을 동시에 안전하게 실행할 수 있습니다.
        4. **Data Partitioning**: 데이터를 여러 파티션으로 분할하여, 동시에 실행되는 트랜잭션들이 서로 다른 데이터 파티션에 액세스하도록 합니다.

---

2주차

# Serializability

스케줄은 동시적으로 수행되는 **다수 트랜잭션에 속하는 연산이, 수행된 시간적 순서**를 의미한다. 동시적으로 수행된 트랜잭션의 모든 연산이 스케줄에 나와 있어야 하고, 스케줄에 있는 특정 트랜잭션에 속하는 연산 순서는 또한 해당 트랜잭션 내의 연산 순서와 동일하여야 한다.

Serial schedule(직렬 스케줄)은 데이터베이스 시스템에서 여러 트랜잭션이 동시에 실행되지 않고, 순차적으로 실행되어 **데이터 일관성을 보장하는 트랜잭션 실행 순서**를 의미합니다.

Serializability이란 무엇인가?

Serializability는 데이터베이스에서 여러 트랜잭션이 동시에 실행되더라도, **마치 순차적으로 실행된 것처럼** 결과가 나오는 것을 보장하는 특성을 말합니다. 
이는 데이터베이스의 일관성을 유지하는데 중요한 역할을 합니다.

---

There are three cases in which **non-serializable execution** causes problems, concurrency anomalies

1. Dirty read는 트랜잭션에서 아직 **커밋되지 않은 수정 중인 데이터**를 다른 트랜잭션에서 **읽는 것**을 말합니다. 이로 인해 데이터의 불일치 문제가 발생할 수 있습니다. 
2. Lost Update는 두 개 이상의 트랜잭션이 **동일한 데이터를 동시에 업데이트**하려고 할 때 발생하는 문제를 의미합니다. 이로 인해 한 트랜잭션에서 수행한 업데이트가 다른 트랜잭션에 의해 덮어쓰여지거나 손실될 수 있습니다.
    
    이러한 문제를 해결하기 위해 데이터베이스 관리 시스템은 일반적으로 잠금이나 타임스탬프와 같은 동시성 제어 메커니즘을 사용합니다.
    
3. Unrepeatable read는 트랜잭션 내에서 동일한 데이터를 **읽을 때,** **중간에 다른 트랜잭션에 의해 해당 데이터가 수정**되어 내용이 변경되는 현상을 나타냅니다. 이로 인해 한 트랜잭션이 동일한 쿼리를 두 번 수행했을 때 결과가 일관성 없이 다르게 나타날 수 있습니다.

---

The **two widely-accepted criteria for serializability** in database systems are 
**conflict serializability** and **view serializability**.

concurrent하게 돌아가는 transaction에서 consistency를 만족하기 위해선 == **Serializable 하기 위해선**

 둘 중 하나를 만족하면 됩니다.

1. **Conflict Serializability**: This checks whether the conflict order (read, write, update) among transactions is preserved in the schedule. If the schedule is conflict equivalent to any serial schedule, it is conflict serializable.
    
    **Conflict Serializability**는 데이터베이스에서 여러 트랜잭션이 동시에 실행되는 경우 발생하는 충돌을 분석하고 제어하기 위한 개념입니다. 이는 트랜잭션 간의 상호작용을 분석하여 직렬 가능한(schedule) 순서를 찾는 것을 의미합니다.
    
    트랜잭션 간에는 읽기-쓰기, 쓰기-읽기, **쓰기-쓰기**와 같은 다양한 종류의 충돌이 발생할 수 있습니다. Conflict Serializability는 이러한 충돌을 일정한 순서로 배치하여 데이터 일관성을 유지하면서 동시성을 향상시키기 위한 목적으로 사용됩니다.
    
    **스케줄**에서 충돌 **연산**이 아닌 두 연속 **연산**은 그 순서를 교환하더라도 상관없다. → 충돌 직렬가능 스케줄이다.
    
2. **View Serializability**: This is a more general form and it checks whether the final state of a schedule is the same as if the transactions were executed serially in some order. It allows more schedules than conflict serializability, thus providing more concurrency.
    
    View Serializability는 Conflict Serializability와는 다르게 트랜잭션 간의 충돌뿐만 아니라 트랜잭션이 볼 수 있는 데이터의 일관성까지 고려하는 개념입니다.
    
    **View equivalence**는 두 개의 트랜잭션 스케줄이 **서로 다른 뷰에서 동일한 결과**를 가지는 경우를 나타냅니다. 트랜잭션의 실행 순서가 아닌, 결과의 관점에서 스케줄을 평가하는 중요한 개념입니다.
    
    뷰 직렬가능 스케줄 정의
    스케줄 S에 속한 각 트랜잭션의 읽기 연산에 대하여 S' 스케줄에서 동일한 값을 읽고,
    스케줄 S의 마지막 쓰기 연산이 S' 스케줄의 마지막 쓰기 연산과 동일하면,
    스케줄 S'는 뷰 직렬가능 스케줄이라고 한다.
    

![Schedule 8은 V.S하지만 C.S하지 않습니다](/assets/images/2-1_Transaction_Concept/Untitled1.png)

Schedule 8은 V.S하지만 C.S하지 않습니다

![Untitled](/assets/images/2-1_Transaction_Concept/Untitled2.png)

![**Conflict Serializability하지 않고 View Serializability하지 않아도 consistency한 경우도 있습니다.**](/assets/images/2-1_Transaction_Concept/Untitled3.png)

**Conflict Serializability하지 않고 View Serializability하지 않아도 consistency한 경우도 있습니다.**

# How to Test Serializability

1. **non-confilct**한지 확인
    1. 실행 순서를 바꿔도 **View Serializability한지 확인하는 방식**

말고도 test방법이 있을까?

## Testing for Conflict Serializability

![그래프를 그려보았을 때 사이클이 존재한다면 Not conflict serializable](/assets/images/2-1_Transaction_Concept/Untitled4.png)

그래프를 그려보았을 때 사이클이 존재한다면 Not conflict serializable

![사이클이 존재하지 않으므로 conflict serializable](/assets/images/2-1_Transaction_Concept/Untitled5.png)

사이클이 존재하지 않으므로 conflict serializable

## Test for View Serializability

View Serializability 시험은 NP-complete 영역 문제이기에, 다항식 시간(polynomial time) 알고리즘을 구하는 것은 매우 어렵다.

그러나 실질적인 View Serializability 시험은 충돌 직렬가능 스케줄에서 읽지 않고 쓰기(blind write) 연산 존재 여부를 활용한다.

# Recoverability

Transaction State 가 commit의 영역까지 왔을 때 고려하는 사항. 이전 단계까지는 Serializability를 고려한다.

서로 관련이 있는 transaction들(자식 transaction…?)은 부모에 abort가 생기면 연달아서 abort를 발생시키고 rollback해야 한다. → 연쇄철회

연쇄 철회를 최소화하기 위해선?

## Cascadeless Schedules (연속적인 철회가 필요 없는 스케줄)

관련을 안 만들면 된다

트랜젝션 하나가 끝나면 커밋 완료하고, 다음 트랜젝션을 진행한다.
→ commit하는 시점을 앞으로 당긴다 
→ 제한적인(strict) Schedules : 쓰기 연산을 한 트랜잭션이 (완료 또는 철회로) 종료될 때까지 해당 데이터를 다른 트랜잭션이 읽거나 쓸 수 없다. (w-w, w-r 제한걸기)

![Untitled](/assets/images/2-1_Transaction_Concept/Untitled6.png)

RC : recoverable
ACA : avoids cascading aborts. **w-r만 따짐**
ST : strict. **w-w, w-r 둘 다 따짐**
SR : conflict serializable

3주차

# 배운 내용 정리

---

A database must provide a mechanism that will ensure that all possible schedules are

- Either conflict or view serializable
- Recoverable and preferably cascadeless

> 그러면 차라리 순차적으로 transaction 처리하면 안되나요?
serial schedules 하면 안되나요?
> 
- NO!!⚠️ **Concurrency**가 너무 떨어집니다!

> 그러면 concurrent schedules로 해야겠네요..
근데 그러면 schedules are conflict/view serializable, and are recoverable and cascadeless 해야하는데, 방법이 있나요?
> 
- 현재까지 개발된 동시성 제어 규약은 
록킹(locking), 
타임스탬프 순서화(timestamp ordering), 
다중버전(multiversion), 
낙관적 규약(optimistic protocol 또는 validation protocol) 등이 있습니다.
그중 가장 많이 사용되는 것이 **lock based protocol**입니다.