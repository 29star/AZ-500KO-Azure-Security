﻿---
lab:
    title: '랩 4 - VNet 만들기'
    module: '모듈 2 - 플랫폼 보호 구현'
---

# 모듈 2: 랩 4 - VNet 만들기


**시나리오**

이 모듈에서는 Azure 가상 네트워크의 정의와 구성 방식을 알아봅니다. 그리고 템플릿, PowerShell, CLI 또는 Azure Portal을 사용하여 가상 네트워크를 만들고 구성하는 방법을 살펴봅니다. 또한 공용 IP, 개인 IP, 정적 IP, 동적 IP 지정 방식 간의 차이점과 시스템 경로, 라우팅 테이블, 라우팅 알고리즘의 사용 방식도 알아봅니다. 이 랩에 포함된 단원은 다음과 같습니다.

- 가상 네트워크 소개
- Azure 가상 네트워크 만들기
- IP 주소 지정 검토


## 연습 1: Azure Portal을 사용하여 가상 네트워크 만들기


**시나리오**

가상 네트워크는 Azure의 개인 네트워크용 기본 구성 요소입니다. VM(가상 머신) 등의 Azure 리소스는 가상 네트워크를 통해 상호 간에/인터넷과 안전하게 통신할 수 있습니다. 이 랩에서는 Azure Portal을 사용하여 가상 네트워크를 만드는 방법을 배웁니다. 그런 다음 가상 네트워크에 VM 두 개를 배포하고, 두 VM 간의 안전한 통신을 진행한 다음, 인터넷에서 VM에 연결합니다.


### 태스크 1: 가상 네트워크 만들기

1.  화면 왼쪽 위에서 **리소스 만들기** > **네트워킹** > **가상 네트워크**를 선택합니다.

1.  **가상 네트워크 만들기**에서 다음 정보를 입력하거나 선택합니다.

    | 설정 | 값 |
    | ------- | ------ |
    | 이름 | *myVirtualNetwork*를 입력합니다. |
    | 주소 공간 | *10.1.0.0/16*을 입력합니다. |
    | 구독 | 사용자의 구독을 선택합니다.|
    | 리소스 그룹 | **새로 만들기**를 선택하고 *myResourceGroup*을 입력한 다음 **확인**을 선택합니다. |
    | 위치 | **미국 동부**를 입력합니다.|
    | 서브넷 - 이름 | *myVirtualSubnet*을 입력합니다. |
    | 서브넷 - 주소 범위 | *10.1.0.0/24*을 입력합니다. |

1.  나머지 항목은 기본값을 그대로 유지하고 **만들기**를 선택합니다.

### 태스크 2: 가상 머신 만들기


가상 네트워크에서 VM 두 개를 만듭니다.


1.  화면 왼쪽 위에서 **리소스 만들기** > **컴퓨팅** > **Windows Server 2019 Datacenter**를 선택합니다.

1.  **가상 머신 만들기 - 기본 사항**에서 다음 정보를 입력하거나 선택합니다.

    | 설정 | 값 |
    | ------- | ------ |
    | **프로젝트 세부 정보** | |
    | 구독 | 사용자의 구독을 선택합니다. |
    | 리소스 그룹 | 이전 섹션에서 만든 **MyResourceGroup**을 선택합니다. |
    | **인스턴스 세부 정보** |  |
    | 가상 머신 이름 | *myVm1*을 입력합니다. |
    | 지역 | **미국 동부**를 입력합니다. |
    | 가용성 옵션 | 기본 **인프라 중복성 없음**을 그대로 둡니다. |
    | 이미지 | 기본 **Windows Server 2019 Datacenter**를 그대로 둡니다. |
    | 크기 | 기본 **표준 DS1 v2**를 그대로 둡니다. |
    | **관리자 계정** |  |
    | 사용자 이름 | 선택한 사용자 이름을 입력합니다. |
    | 암호 | Pa55w.rd1234 |
    | 암호 확인 | 암호를 다시 입력합니다. |
    | **인바운드 포트 규칙** |  |
    | 공용 인바운드 포트 | 기본 **없음**을 그대로 둡니다. |
    | **비용 절약** |  |
    | 이미 Windows 라이선스가 있으신가요? | 기본 **없음**을 그대로 둡니다. |

1.  **다음: 디스크**를 선택합니다.

1.  **가상 머신 만들기 - 디스크**에서 기본값을 그대로 두고 **다음 네트워킹**을 선택합니다.

1.  **가상 머신 만들기 - 네트워킹**에서 다음 정보를 선택합니다.

    | 설정 | 값 |
    | ------- | ------ |
    | 가상 네트워크 | 기본 **myVirtualNetwork**를 그대로 둡니다. |
    | 서브넷 | 기본 **myVirtualSubnet (10.1.0.0/24)**을 그대로 둡니다. |
    | 공용 IP | 기본 **(new) myVm-ip**를 그대로 둡니다. |
    | 공용 인바운드 포트 | **선택한 포트 허용**을 선택합니다. |
    | 인바운드 포트 선택 | **HTTP** 및 **RDP**를 선택합니다.

1.  **다음: 관리**를 선택합니다.

1.  **가상 머신 만들기 - 관리**에서, **진단 스토리지 계정**에 대해 **새로 만들기**를 선택합니다.

1.  **스토리지 계정 만들기**에서 다음 정보를 입력하거나 선택합니다.

    | 설정 | 값 |
    | ------- | ------ |
    | 이름 | *myvmstorageaccount*를 입력합니다. 이 이름이 사용 중이라면 고유한 이름을 작성하세요.|
    | 계정 종류 | 기본 **스토리지(범용 v1)**를 그대로 둡니다. |
    | 성능 | **기본**을 그대로 둡니다. |
    | 복제 | 기본 **로컬 중복 스토리지(LRS)**를 그대로 둡니다. |

1.  **확인**을 선택합니다.

1.  **검토 + 만들기**를 선택합니다. **검토 + 만들기** 페이지가 표시되며, 구성 유효성 검사가 진행됩니다.

1.  **유효성 검사 통과** 메시지가 표시되면 **만들기**를 선택합니다.

### 태스크 3:  두 번째 VM 만들기

1.  위의 1~9단계를 완료합니다.

    **참고**: 2단계에서는 **가상 머신 이름**으로 *myVm2*를 입력합니다.  7단계에서는 **진단 스토리지 계정**으로 **myvmstorageaccount**를 선택해야 합니다.


1.  **검토 + 만들기**를 선택합니다. **검토 + 만들기 페이지**로 이동하고 Azure에서 구성의 유효성을 검사합니다.

1.  **유효성 검사 통과** 메시지가 표시되면 **만들기**를 선택합니다.

### 태스크 4:  인터넷에서 VM에 연결


*myVm1*을 만든 후 인터넷에 연결합니다.


1.  포털의 검색 표시줄에서 *myVm1*을 입력합니다.

1.  **연결** 단추를 선택합니다.


    **연결** 단추를 선택하면 **가상 머신에 연결**이 열립니다.

1.  **RDP 파일 다운로드**를 선택합니다. 원격 데스크톱 프로토콜(*.rdp*) 파일이 작성되어 컴퓨터에 다운로드됩니다.

1.  다운로드된 *rdp* 파일을 엽니다.

    1. 메시지가 표시되면 **연결**을 선택합니다.

    1. VM을 만들 때 지정한 사용자 이름과 암호를 입력합니다.

    **참고**: VM을 만들 때 입력한 자격 증명을 지정하려면 **다른 옵션 선택** > **다른 계정 사용**을 선택해야 할 수 있습니다.


1.  **확인**을 선택합니다.

1.  로그인 프로세스 중에 인증서 경고가 표시될 수 있습니다. 인증서 경고가 표시되면 **예** 또는 **계속**을 선택합니다.

1.  VM 데스크톱이 나타나면 최소화하여 로컬 데스크톱으로 돌아옵니다.

### 태스크 5: VM 간 통신

1.  *myVm1*의 원격 데스크톱에서 PowerShell을 엽니다.

1.  `ping myVm2`를 입력합니다.

    다음과 같은 메시지가 나타납니다.

    ```powershell
    Pinging myVm2.0v0zze1s0uiedpvtxz5z0r0cxg.bx.internal.clouda
    Request timed out.
    Request timed out.
    Request timed out.
    Request timed out.

    Ping statistics for 10.1.0.5:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
    ```

    `ping`은 ICMP(Internet Control Message Protocol)를 사용하므로 실패합니다. 기본적으로 ICMP는 Windows 방화벽을 통해 허용되지 않습니다.

1.  이후 단계에서 *myVm2*가 *myVm1*에 ping을 실행할 수 있도록 하려면 다음 명령을 입력합니다.

    ```powershell
    New-NetFirewallRule -DisplayName "Allow ICMPv4-In" -Protocol ICMPv4
    ```

    이 명령을 실행하면 Windows 방화벽에서 ICMP가 인바운드로 허용됩니다.

1.  *myVm1*에 대한 원격 데스크톱 연결을 닫습니다.

1.  **인터넷에서 VM에 연결** 태스크의 단계를 다시 완료하되, 이번에는 *myVm2*에 연결합니다.

1.  명령 프롬프트에서 `ping myvm1`을 입력합니다.

    그러면 다음과 같은 메시지가 반환됩니다.

    ```powershell
    Pinging myVm1.0v0zze1s0uiedpvtxz5z0r0cxg.bx.internal.cloudapp.net [10.1.0.4] with 32 bytes of data:
    Reply from 10.1.0.4: bytes=32 time=1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128

    Ping statistics for 10.1.0.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 1ms, Average = 0ms
    ```

    3단계에서 *myVm1* VM의 Windows 방화벽을 통해 ICMP를 허용했으므로 *myVm1*에서 회신이 수신됩니다.

1.  *myVm2*에 대한 원격 데스크톱 연결을 닫습니다.


| 경고: 계속하기 전에 이 랩에서 사용한 모든 리소스를 제거해야 합니다.  **Azure Portal**에서 리소스를 제거하려면 **리소스 그룹**을 클릭합니다.  랩에서 만든 리소스 그룹을 모두 선택합니다.  리소스 그룹 블레이드에서 **리소스 그룹 삭제**를 클릭하고 리소스 그룹 이름을 입력한 다음 **삭제**를 클릭합니다.  추가로 만든 리소스 그룹이 있으면 이 프로세스를 반복합니다. **리소스 그룹을 삭제하지 않으면 다른 랩에서 문제가 발생할 수 있습니다.** |
| --- |

**결과**: 이 랩이 완료되었습니다.