---
title: Magnetic Disks
date: 2024-05-20 13:09
category: DB
tags: [Storage Devices]
summary: Magnetic Disks are storage devices that use magnetic storage to store and retrieve digital information. In this article, we explore the key characteristics, structure, and reliability of hard disk drives (HDDs).
toc: true
toc_sticky: true
---

# What are Magnetic Disks?

하드 디스크 드라이브(HDD)는 데이터 저장을 위해 회전하는 자기 디스크와 읽기/쓰기 헤드를 사용하는 장치입니다.

![Magnetic Disks]({{"assets/images/storage_devices/2.png" | relative_url}}){: .align-center width="50%"}

위 이미지에서는 Magnetic Disk 내부를 추상적으로 보여주고 있습니다. 여러개의 디스크 platter가 그림과 같이 구성되어 있고, 일정한 속도로 회전합니다. read/write head가 디스크의 특정 위치(원하는 track 및 sector)에 자기적으로 데이터를 읽거나 쓸 수 있습니다.
{: .notice}

자기 디스크, 일반적으로 하드 디스크 드라이브(HDD)라고 불리는 저장 장치는, 디지털 정보를 저장하고 읽기 위해 자기 저장을 사용하는 장치입니다. HDD는 하나 이상의 빠르게 회전하는 플래터(플래터)로 구성되어 있으며, 플래터는 자기 물질로 코팅되어 있습니다.

**Key Characteristics and Structure**

- **Multiple Disk Platters:** 일반적으로 HDD는 하나의 spindle에 여러 개의 platter(1에서 5개)를 가지고 있습니다.
- **Read/Write Heads:** 각 platter는 하나 또는 두 개의 읽기/쓰기 head를 가지고 있으며, 이 head는 데이터를 읽고 씁니다.
- **Tracks and Cylinders:**
  - 각 platter의 표면은 동심원 형태의 track으로 나뉩니다.
  - Track은 cylinder로 그룹화됩니다. (여러 개의 platter를 가진 하드 디스크에서 동일한 track번호를 가진 track들이 모여 하나의 cylinder를 구성한다는 뜻입니다.)
- **Sector:**
  - 각 트랙은 sector라는 더 작은 단위로 나뉩니다.
  - sector는 일반적으로 512바이트로, 읽기 또는 쓰기 가능한 데이터의 가장 작은 단위입니다.
  - 일반적으로 track당 sector 수는 내부 track에서 500에서 1000, 외부 track에서는 1000에서 2000입니다.
{: .notice--info}

**Reading/Writing Data**

- **Positioning:** 데이터를 읽거나 쓰기 위해 disk arm이 Read/Write Head를 올바른 트랙 위로 이동시킵니다.
- **Rotation:** platter가 계속 회전하며, sector가 Read/Write Head 아래를 지날 때 데이터를 읽거나 씁니다.
- **Magnetic Encoding:** Read/Write Head는 platter에 자기적으로 인코딩된 데이터를 읽거나 씁니다.
{: .notice--info}

**Reliability**

- **Head-Crashes**: 초기 세대의 HDD는 헤드 크래시(Read/Write Head가 디스크 표면을 손상시키는 현상)에 취약했습니다.
- **Improved Reliability:** 현대의 HDD는 이러한 failures에 덜 취약하지만, 개별 sector는 여전히 손상될 수 있습니다.
{: .notice--info}

# Disk Controller

디스크 컨트롤러는 컴퓨터 시스템과 disk drive 하드웨어 사이의 인터페이스 역할을 합니다.

![Disk Controller]({{"assets/images/storage_devices/3.png" | relative_url}}){: .align-center width="50%"}

**기능**

- **명령 처리:** 컴퓨터로부터 데이터를 읽거나 쓰기 위한 고수준 명령을 수락합니다.
- **움직임 제어:** disk arm을 올바른 track으로 이동시키고 실제로 데이터를 읽거나 쓰는 작업을 수행합니다.
- **Multiple Disk Management:** 여러 디스크를 컴퓨터 시스템에 연결합니다.
{: .notice--info}

**구성 요소**

- **Microprocessor:** 디스크의 제어 논리를 처리합니다.
- **Buffer Memory:** 데이터 전송 중에 데이터를 임시로 저장합니다.
- **Cache:** 접근 시간을 단축시키기 위해 캐시를 포함할 수 있습니다.
{: .notice--info}

**데이터 무결성**

- **Checksums:** 각 섹터에 체크섬을 추가하여 데이터 무결성을 확인합니다. 데이터가 손상되면 저장된 체크섬이 다시 계산된 체크섬과 일치하지 않습니다.
- **Successful Writing:** 데이터를 쓴 후 sector를 다시 읽어 성공적인 쓰기를 보장합니다.
- **Bad Sector Remapping:** 불량 sector를 재할당하여 데이터 무결성을 보장합니다.
{: .notice--info}

# 디스크 인터페이스 표준

디스크 드라이브와 컴퓨터 시스템을 연결하기 위해 여러 가지 표준 인터페이스 방식이 사용되며, 각 표준은 서로 다른 기능과 속도를 제공합니다:

- **SCSI (Small Computer System Interconnect)**
- **ATA (Advanced Technology Attachment), IDE로도 알려져 있음**
- **SATA (Serial ATA), SATA I, SATA II, SATA III**
- **SAS (Serial Attached SCSI)**
- **Fiber Channel**
- **IEEE 1394 (FireWire, 주로 Apple에서 사용)**

각 표준에는 여러 변형이 있으며, 서로 다른 속도와 기능을 제공합니다(각 방식은 전송속도 면에서 차이가 있습니다).

# Disk Connection

직접 연결 방식은 DAS, 네트워크 연결 방식은 SAN과 NAS로 구분할 수 있습니다.
- **DAS (Direct Attached Storage):** 디스크가 컴퓨터 시스템에 직접 연결됩니다.
- **SAN (Storage Area Network):** 고속 네트워크를 통해 다수의 디스크가 여러 서버에 연결됩니다.
  - 디스크는 RAID를 사용하여 논리적으로 조직됩니다.
  - SCSI, SAS, 파이버 채널 인터페이스를 사용합니다.
- **NAS (Network Attached Storage):** 네트워크 파일 시스템 프로토콜을 사용하여 네트워크 클라이언트에 데이터를 제공하는 파일 시스템 인터페이스를 제공합니다.
{: .notice}

## SAN
고속 네트워크를 통해 servers와 storage devices를 연결하는 특수한 네트워크입니다. SAN의 주요 목적은 컴퓨터 시스템과 저장 장치 간에 데이터를 전송하는 것입니다. (disk arrays, tape libraries 및 optical jukeboxes 같은 다양한 저장 요소를 포함할 수 있습니다)

**Key Features**

1. **High-Speed Network:** SAN은 고속 네트워크를 사용하여 서버와 저장 장치 간의 데이터 전송을 빠르게 합니다.
2. **Remote Storage Connection:** SAN은 원격 저장 장치를 server에 연결하는 아키텍처를 제공하여, 저장 장치가 로컬에 연결된 것처럼(로컬 서버가 확장된 것처럼) 보이게 합니다.
3. **Shared Disks:** SAN을 통해 multiple computing servers가 Disk를 공유할 수 있습니다.
{: .notice}

**주요 기능**

- **"Any-to-Any" Connection:** SAN은 라우터, 게이트웨이, 허브 및 스위치와 같은 상호 연결 요소를 사용하여 네트워크 전체에서 모든-to-모든 연결을 허용합니다.
- **Block-Level Operations:** SAN은 "file" abstraction을 제공하지 않고 block-level operation만 제공합니다.
- **비용 및 복잡성 감소:** 2000년대 후반에 SAN의 비용과 복잡성이 감소하여 더 널리 채택되었습니다.
{: .notice}

**SAN과 관련된 기술**

- **RAID (Redundant Array of Independent Disks):** 데이터를 여러 디스크에 분산 저장하여 신뢰성과 성능을 높입니다.
- **SCSI (Small Computer System Interface):** 서버와 저장 장치 간의 통신을 위한 표준 인터페이스입니다.
- **Fiber Channel:** 고속 데이터 전송을 위해 SAN에서 널리 사용되는 기술입니다.
{: .notice}

## NAS
네트워크에 연결된 파일 수준의 데이터 저장 장치로, 이기종 네트워크 클라이언트 간에 데이터를 제공하는 시스템입니다. 

**Key Features**

1. **파일 수준 저장:** NAS는 파일 시스템 인터페이스를 통해 데이터를 제공합니다.
2. **네트워크 연결:** NAS는 네트워크를 통해 여러 클라이언트가 데이터를 접근하고 공유할 수 있게 합니다.
{: .notice}

**주요 기능**

- **네트워크를 통한 데이터 공유:** NAS는 네트워크를 통해 다양한 클라이언트가 데이터를 접근할 수 있도록 하여 파일 공유를 쉽게 합니다.
- **파일 기반 프로토콜 사용:** NAS는 NFS, SMB/CIFS, AFP, AFS 등의 파일 기반 프로토콜을 사용하여 데이터를 전송합니다.
{: .notice}

**파일 기반 프로토콜**

- **NFS (Network File System):** UNIX 시스템에서 많이 사용됨.
- **SMB/CIFS (Server Message Block/Common Internet File System):** MS Windows에서 많이 사용됨.
- **AFP (Apple Filing Protocol):** Apple Macintosh 컴퓨터에서 사용됨.
- **AFS (Andrew File System):** Carnegie Mellon University에서 개발된 파일 시스템.
{: .notice}

## NAS vs. DAS vs. SAN

| 항목                        | DAS (직접 연결 스토리지)                | SAN (스토리지 영역 네트워크)                | NAS (네트워크 연결 스토리지)           |
|-----------------------------|-----------------------------------------|-------------------------------------------|-----------------------------------------|
| **연결 방식**               | 단일 서버에 직접 연결                   | 고속 네트워크를 통해 여러 서버와 스토리지 장치를 연결 | 네트워크에 연결되어 여러 클라이언트에서 접근 가능 |
| **Data Access Level**        | Block-level                              | Block-level                                  | File-level                                |
| **Scalability**                  | Limited, 서버에 직접 드라이브를 추가해야 함 | Highly scalable, 네트워크에 장치 추가 가능      | Moderately scalable, 네트워크에 NAS 장치 추가 가능 |
| **Performance**                    | 단일 서버 접근 시 높은 성능              | 매우 높은 성능, 기업 환경에 적합              | 좋은 성능, 파일 공유 및 백업에 적합        |
| **비용**                    | 초기 비용 낮음, 확장성 제한적            | 비용 높음, 대기업에 적합                     | 보통의 비용, 중소기업에 적합              |
| **Use Case**               | 단일 서버의 로컬 스토리지                | 대규모 기업 스토리지, 데이터 센터             | 파일 공유, 백업, 중소기업의 미디어 저장   |
| **프로토콜**                | SCSI, SAS, SATA                        | Fibre Channel, iSCSI, FCoE                | NFS, SMB/CIFS, AFP, FTP                  |
| **관리 복잡성**             | 단순, 서버에서 관리                     | 복잡, 전용 관리 및 구성 필요                 | 보통의 관리 복잡성, 내장 인터페이스로 관리 용이 |
| **Data Sharing**             | 연결된 서버에 제한됨                    | 여러 서버가 공유 스토리지 접근 가능            | 다양한 네트워크 클라이언트에서 쉽게 공유 가능 |
| **Backup and Recovery**            | 일반적으로 로컬 백업만 가능              | 고급 백업 및 복구 옵션, 높은 가용성       | 좋은 백업 옵션, 스냅샷 기능          |
| **중복성 및 장애 조치**      | 제한적, 서버 기능에 의존                 | 높은 중복성 및 장애 조치 기능                 | 보통, 일부 NAS 장치는 중복성 및 장애 조치 제공 |

**추가 설명**

![Storage Devices]({{"assets/images/storage_devices/4.png" | relative_url}}){: .align-center width="50%"}

- **NAS**: 네트워크를 통해 여러 클라이언트와 서버가 파일 수준에서 데이터를 공유할 수 있도록 하는 저장 장치입니다. NAS는 파일 기반 프로토콜을 사용하여 데이터를 네트워크 패킷으로 전송합니다. 
- **DAS**: 단일 서버에 직접 연결된 저장 장치로, 다른 네트워크 장치와 공유되지 않습니다.
- **SAN**: 고속 네트워크를 통해 여러 서버가 블록 수준에서 데이터를 공유하고 관리할 수 있도록 하는 시스템입니다. SAN은 블록 기반 프로토콜을 사용하여 데이터를 전송합니다. 
{: .notice}


**SAN-NAS 하이브리드**
- NAS와 SAN의 장점을 모두 제공하여, 파일 수준 프로토콜(NAS)과 블록 수준 프로토콜(SAN)을 동시에 지원하는 시스템입니다
{: .notice}

# 디스크 성능 평가

Performance Measures of Disks는 다음과 같은 세 가지 측정 항목으로 평가됩니다.

**1. 접근 시간 (Access Time)**<br>
    읽기/쓰기 요청이 발행된 시점 ~ 데이터 전송이 시작될 때까지 걸리는 시간.
- **탐색 시간(Seek Time) + 회전 지연 시간(Rotational Latency)**
  - **탐색 시간 (Seek Time):** 읽기/쓰기 헤드가 올바른 트랙으로 이동하는 데 걸리는 시간.
    - 일반적인 디스크에서는 4~10밀리초.
  - **회전 지연 시간 (Rotational Latency):** 디스크의 섹터가 헤드 아래로 올 때까지의 회전 시간.
    - 일반적인 디스크에서는 4~11밀리초 (5400~15000 RPM).
- **총 접근 시간:** 일반적으로 15~20밀리초.
{: .notice}

**2. 데이터 전송 속도 (Data-Transfer Rate)**<br>
    디스크에서 데이터를 읽거나 쓸 때의 속도.
- **외곽 트랙이 더 높은 전송 속도를 가지며, 내부 트랙은 더 낮은 전송 속도를 가짐** (외곽 트랙이 더 긴 거리를 이동하기 때문).
    - 일반적으로 디스크의 바깥쪽에 있는 트랙은 디스크가 회전할 때 더 많은 데이터를 읽고 쓸 수 있어서 전송 속도가 빠르고, <br>안쪽에 있는 트랙은 데이터를 읽고 쓸 수 있는 양이 적어서 전송 속도가 느립니다.
- **속도 예시:**
  - SATA: 150 MB/sec
  - SATA II (3Gb/s): 300 MB/sec
  - SATA III (6Gb/s): 600 MB/sec
  - Ultra 320 SCSI: 320 MB/sec
  - SAS: 3~6 Gb/sec
  - Fiber Channel (FC 2Gb 또는 FC 4Gb): 256~512 MB/sec
{: .notice}

**3. 평균 무고장 시간 (MTTF, Mean Time to Failure)**<br>
    디스크가 고장 없이 계속 동작할 것으로 예상되는 평균 시간.
- **새 디스크의 고장 확률**은 낮으며, 이론적인 MTTF는 500,000~1,200,000시간 (57~136년).
  - 예시: MTTF가 1,200,000시간인 새 디스크 1000개 중 하나는 평균적으로 1200시간 (50일)마다 고장 발생.
- 디스크가 오래될수록 MTTF는 감소.
- 일반 수명은 보통 3~5년 정도
{: .notice}

# 디스크 성능 향상

자기 디스크는 다른 저장 매체에 비해 접근 속도가 느린 단점이 있습니다. 이를 극복하기 위해 여러 가지 기법을 지원하고 있습니다. 디스크 성능을 향상시키는 주요 기법들을 다음과 같이 정리할 수 있습니다.

**1. 블록 단위 데이터 접근**
- **블록 (Block):** 디스크의 데이터를 읽거나 쓰는 최소 단위.
    - 블록은 단일 track의 연속적인 sector로 구성된 데이터 단위입니다.
- **특징:** 
  - 데이터는 디스크와 메인 메모리 사이에서 블록 단위로 전송됩니다.
  - 작은 블록: 더 많은 전송이 필요하지만, 각 전송의 데이터 양이 적어 데이터 접근이 빠릅니다.
  - 큰 블록: 부분적으로 채워진 블록으로 인해 공간 낭비가 증가할 수 있지만, 한 번에 많은 데이터를 전송할 수 있습니다.
  - 일반적인 블록 크기: 4~16KB
{: .notice}

**2. Disk-Arm-Scheduling Algorithms**<br>
    disk arm의 이동을 최소화하기 위해 트랙에 대한 접근 요청을 정렬하는 알고리즘입니다. 이 알고리즘은 disk arm이 가장 가까운 요청을 먼저 처리하도록 하여 효율성을 높입니다.
  - **엘리베이터 알고리즘:** disk arm이 엘리베이터처럼 위아래로 움직이며, 가장 가까운 요청을 먼저 처리합니다. 이 알고리즘은 disk arm 이동을 최적화하여 접근 시간을 줄입니다.
{: .notice}

**3. 데이터 집약화(clustering)**<br>
    데이터 접근 시간을 최적화하기 위해 관련 데이터를 물리적으로 근접한 위치에 저장하는 방법입니다. 논리적으로 가까운 데이터를 같은 실린더나 인접 실린더에 저장하여 접근 시간을 줄입니다.
- **특징:**
  - 같은 실린더나 인접 실린더에 관련 정보를 저장합니다.
  - 시간이 지나면서 파일이 조각화될 수 있으며, 이로 인해 disk arm 이동이 증가할 수 있습니다.
  - **디스크 조각 모음 유틸리티**를 사용하여 파일 시스템을 정리하고 최적화할 수 있습니다.
{: .notice}

**4. 비휘발성 쓰기 버퍼 (Nonvolatile Write Buffers)**<br>
    비활성 RAM에 쓰기 연산을 우선 하게 하여 쓰기 연산에 따른 기다림을 없애는 방식입니다.
- **특징:**
  - 데이터베이스 작업이 디스크에 데이터를 안전하게 저장하기 전까지 계속 진행될 수 있습니다.
  - disk arm 이동을 최소화하기 위해 쓰기 작업을 재정렬할 수 있습니다.
{: .notice}

**5. 로그 디스크 (Log Disk)**<br>
    블록 업데이트의 순차 로그를 쓰는 전용 디스크입니다. 순차적인 데이터 쓰기만 수행하므로 탐색 시간이 필요 없으며, 데이터 접근 성능이 향상됩니다.
- **특징:**
  - 로그 디스크에 쓰기는 탐색 시간이 필요 없어 매우 빠릅니다.
  - 특별한 하드웨어(NV-RAM)가 필요하지 않습니다.
  - **저널링 파일 시스템**: 로그 디스크를 지원하며, 성능 향상을 위해 디스크 쓰기를 재정렬합니다.
{: .notice}