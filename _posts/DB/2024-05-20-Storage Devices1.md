---
title: Physical Storage Media
date: 2024-05-20 13:09
category: DB
tags: [Storage Devices]
summary: Physical Storage Media Classification and Storage Hierarchy
toc: true
toc_sticky: true
---

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

### Primary Storage
- 레지스터 (Registers)
- 캐시 메모리 (Cache Memory)
- 주 메모리 (Main Memory)

Primary Storage(기본 저장 장치)는 CPU가 직접 접근하여 데이터를 처리하는 저장 장치로, 매우 빠른 속도를 가지고 있습니다. 그러나 용량이 작고, 휘발성 메모리인 경우 전원이 꺼지면 데이터가 사라지는 특징이 있습니다.

### Secondary Storage
- 플래시 메모리 (Flash Memory: SSD)
- 자기 디스크 (Magnetic Disks: HDD)

Secondary Storage(보조 저장 장치)는 대용량 데이터를 영구적으로 저장하기 위한 장치입니다. 전원이 꺼져도 데이터가 유지되며, Primary Storage보다 속도는 느리지만 용량이 크고 비용이 저렴합니다.

### Tertiary Storage
- 광학 디스크 (Optical Disks)
- 자기 테이프 (Magnetic Tape)

Tertiary Storage(삼차 저장 장치)는 주로 백업 및 장기 보관용으로 사용됩니다. 데이터 접근 속도는 가장 느리지만, 대용량 데이터를 저렴한 비용으로 저장할 수 있습니다.