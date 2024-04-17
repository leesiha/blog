---
title: Orthodox Canonical Form
date: 2024-04-17 13:23
category: C++
author: LSH
tags: [constructor, destructor, OCCF]
summary: OCCF는 올바른 클래스 디자인을 위한 지침입니다. 사용자가 직접 정의한 데이터 타입이 기본 객체 사용 환경에서 올바르게 작동하도록 보장하기 위해, 구현해야 하는 4개의 함수를 소개하고 있습니다.
toc: true
---

## What is the Orthodox Canonical Class Form?
OCCF는 <span style="background-color:#fff5b1"> 올바른 클래스 디자인을 위한 지침 </span> 입니다. <br>
사용자가 직접 정의한 데이터 타입이 기본 객체 사용 환경에서 올바르게 작동하도록 보장하기 위해, 구현해야 하는 4개의 함수를 소개하고 있습니다. <br>

**OCCF를 준수하여 클래스를 작성하면**
- 코드 내에서 객체가 어떻게 활용되는지 깊이있게 이해할 수 있습니다.
- 고급 C++ idioms 를 이해하기 위한 중요한 기초가 됩니다.

> OCCF를 잘 이해하면, 복잡한 객체 지향 애플리케이션에서도 잘 동작하는 C++ 클래스를 작성하는 데 큰 도움이 됩니다.

### The Four Required Functions
*OCCF specifies 4 required functions that should be implemented for complex, user-defined data types.*
1. 기본 생성자 (default constructor): 객체 생성 시 초기화
2. 복사 생성자 (copy constructor): 기존 객체로부터 새로운 객체 생성
3. 복사 할당 연산자 (copy assignment operator): 객체 할당 시 값 복사
4. 소멸자 (destructor): 객체 소멸 시 리소스 해제

### Example
Foo 라는 user-defined type을 가정해보겠습니다.
```c++
// Foo.hpp
class Foo {
public:
    // Default constructor
    Foo();

    // Copy constructor
    Foo(const Foo& other);

    // Copy assignment operator
    Foo& operator=(const Foo& other);

    // Destructor
    ~Foo();

private:
    // Foo가 관리하는 포인터
    int* pointer;
};
```
#### Default Constructor

기본 생성자는 객체를 생성할 때 호출되는 생성자입니다. 기본 생성자는 객체의 모든 멤버 변수를 초기화해야 합니다.
```c++
// Foo.cpp
#include "Foo.hpp"

Foo::Foo() {
    pointer = new int(0);
}
```
- 인수 없이 호출되는 생성자입니다.
- `new int(0)`을 통해 `pointer`를 초기화하고 있습니다.

#### Copy Constructor

복사 생성자는 기존 객체에 대한 복사본을 생성하는 생성자입니다. 객체를 복사할 때 호출되며, (보통) 객체의 모든 멤버 변수를 복사합니다.
```c++
// Foo.cpp
#include "Foo.hpp"

Foo::Foo(const Foo& other) {
    pointer = new int(*other.pointer);
}
```
- `const Foo& other`를 통해 다른 객체를 참조하고 있습니다.
- `new int(*other.pointer)`를 통해 `pointer`를 초기화하고 있습니다.

#### Copy Assignment Operator

복사 할당 연산자는 객체에 다른 객체를 할당할 때 호출되는 연산자입니다. 객체의 값을 복사하는 것이 목적입니다.
```c++
// Foo.cpp
#include "Foo.hpp"

Foo& Foo::operator=(const Foo& other) {
    if (this != &other) {
        delete pointer;
        pointer = new int(*other.pointer);
    }
    return *this;
}
```
- `const Foo& other`를 통해 다른 객체를 참조하고 있습니다.
- `if (this != &other)`를 통해 자기 자신에 대한 할당인지 확인하고 있습니다.
- `delete pointer`를 통해 자신이 가리키는 메모리를 해제하고 있습니다.
- `new int(*other.pointer)`를 통해 `pointer`를 초기화하고 있습니다.

#### Destructor

소멸자는 객체가 소멸될 때 호출되는 함수입니다. 객체가 소멸될 때 필요한 리소스를 해제하는 것이 목적입니다.
```c++
// Foo.cpp
#include "Foo.hpp"

Foo::~Foo() {
    delete pointer;
}
```
- `delete pointer`를 통해 `pointer`가 가리키는 메모리를 해제하고 있습니다.
