---
title: Storage Devices(1)
date: 2024-05-20 13:09
category: DB
tags: [Physical Storage Media, Magnetic Disks]
summary: In this section, we will look at computer storage devices. Because database systems store and manage data, knowledge of data storage devices is necessary to understand database systems. We will discuss the characteristics of each storage device, the characteristics of hard disks, the most common medium for storing data, RAID, and the file and record structures and buffers in computer systems. 
toc: true
toc_sticky: true
---

# Physical Storage Media

## Classification of Physical Storage Media

물리적 저장 매체의 분류는 1. 데이터 접근 속도 2. 데이터 저장 비용 3. 신뢰성 등으로 구분이 가능하며, 특히 전원 공급이 중단되었을 때 데이터 손실 여부에 따라 **휘발성/비휘발성** 저장 장치로 구분할 수 있습니다.

## Storage Hierarchy

저장 계층 구조는 컴퓨터 시스템에서 데이터 저장 장치들을 계층적으로 분류한 것입니다. 이 계층 구조는 각 저장 장치의 성능, 비용, 용량 등을 고려하여 다음과 같이 구분됩니다:

1. **Register**: CPU 내부에 위치하며, 가장 빠른 속도로 데이터에 접근할 수 있습니다.
![Storage Hierarchy]({{"assets/images/storage_devices/1.png" | relative_url}}){: .align-right width="10%"}
2. **Cache Memory**: CPU와 메인 메모리 사이에 위치하며, CPU가 자주 사용하는 데이터를 저장합니다.
3. **Main Memory**(RAM): 프로그램이 실행되는 동안 데이터를 저장하는 메모리로, 데이터에 직접 접근할 수 있습니다.
4. **Flash Memory**(SSD): 비휘발성 메모리로, 전원이 꺼져도 데이터가 보존됩니다. 빠른 속도와 내구성을 제공하지만, HDD보다는 비용이 높습니다.
5. **Magnetic Disk**(HDD): 비휘발성 저장 장치로, 가장 일반적인 데이터 저장 매체입니다. 용량이 크고 비용이 저렴하지만, 속도는 SSD보다 느립니다.
6. **Optical Disk**: CD, DVD 등의 광학 디스크로, 대용량 데이터 저장이 가능하지만 접근 속도가 느립니다.
7. **Magnetic Tape**: 데이터를 순차적으로 읽고 쓰는 테이프입니다. 주로 백업 용도로 사용됩니다. 대용량 데이터를 저렴하게 저장할 수 있지만, 데이터 접근 속도가 매우 느립니다.
{: .notice}

이들은 크게 Primary, Secondary, Tertiary Storage로 분류할 수 있습니다. 

**Primary Storage (기본 저장 장치)**
- 레지스터 (Registers)
- 캐시 메모리 (Cache Memory)
- 주 메모리 (Main Memory)

Primary Storage는 CPU가 직접 접근하여 데이터를 처리하는 저장 장치로, 매우 빠른 속도를 가지고 있습니다. 그러나 용량이 작고, 휘발성 메모리인 경우 전원이 꺼지면 데이터가 사라지는 특징이 있습니다.

**Secondary Storage (보조 저장 장치)**
- 플래시 메모리 (Flash Memory: SSD)
- 자기 디스크 (Magnetic Disks: HDD)

Secondary Storage는 대용량 데이터를 영구적으로 저장하기 위한 장치입니다. 전원이 꺼져도 데이터가 유지되며, Primary Storage보다 속도는 느리지만 용량이 크고 비용이 저렴합니다.

**Tertiary Storage (삼차 저장 장치)**
- 광학 디스크 (Optical Disks)
- 자기 테이프 (Magnetic Tape)

Tertiary Storage는 주로 백업 및 장기 보관용으로 사용됩니다. 데이터 접근 속도는 가장 느리지만, 대용량 데이터를 저렴한 비용으로 저장할 수 있습니다.

# Magnetic Disks
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

## Disk Controller

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

## 디스크 인터페이스 표준

디스크 드라이브와 컴퓨터 시스템을 연결하기 위해 여러 가지 표준 인터페이스 방식이 사용되며, 각 표준은 서로 다른 기능과 속도를 제공합니다:

- **SCSI (Small Computer System Interconnect)**
- **ATA (Advanced Technology Attachment), IDE로도 알려져 있음**
- **SATA (Serial ATA), SATA I, SATA II, SATA III**
- **SAS (Serial Attached SCSI)**
- **Fiber Channel**
- **IEEE 1394 (FireWire, 주로 Apple에서 사용)**

각 표준에는 여러 변형이 있으며, 서로 다른 속도와 기능을 제공합니다(각 방식은 전송속도 면에서 차이가 있습니다).

## Disk Connection

직접 연결 방식은 DAS, 네트워크 연결 방식은 SAN과 NAS로 구분할 수 있습니다.
- **DAS (Direct Attached Storage):** 디스크가 컴퓨터 시스템에 직접 연결됩니다.
- **SAN (Storage Area Network):** 고속 네트워크를 통해 다수의 디스크가 여러 서버에 연결됩니다.
  - 디스크는 RAID를 사용하여 논리적으로 조직됩니다.
  - SCSI, SAS, 파이버 채널 인터페이스를 사용합니다.
- **NAS (Network Attached Storage):** 네트워크 파일 시스템 프로토콜을 사용하여 네트워크 클라이언트에 데이터를 제공하는 파일 시스템 인터페이스를 제공합니다.
{: .notice}

### SAN
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

### NAS
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

### NAS vs. DAS vs. SAN

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