# Intent based zero touch service chaining layer for SDN


- - -


![Fig.2.jpg](/img/paperreview/250711/Fig.2.jpg)


- - -


| **Author** | B. Martini, M. Gharbaoui, P. Castoldi |
| --- | --- |
| **Institute** | sciencedirect |
| **Cite** | Martini, Barbara, Molka Gharbaoui, and Piero Castoldi. "Intent-based zero-touch service chaining layer for software-defined edge cloud networks." Computer Networks 212 (2022): 109034. |


- - -


==Keyword : Intent-based Networking(IBN), Service chaining, Edge networking, SDN, NFV==


- - -



## [ Abstract ]

1. Edge 컴퓨팅 환경에서 분산된 가상화 기능의 복잡성을 해결하기 위해 IBN이 부상
2. 본 논문은 애플리케이션 intent를 기반으로 서비스 체인 경로를 자동 프로비저닝하고 동적으로 적응시키는 zero-touch service chaining layer를 제안
    
    → 분산된 Edge Cloud 네트워크에서 동적 서비스 체인 경로를 프로그래밍 가능한 방식으로 제공하는 Intent 기반 Zero-Touch 서비스 체인 계층(Intent-based zero-touch service chaining layer)을 소개
    
3. 실험을 통해 제안된 layer는 네트워크 자원 사용을 최적화하고 자동 경로 재조정을 통해 서비스 연속성을 보장함을 보여줌

## [  Intent layer이 배치될 환경 ]

![Fig.1.jpg](/img/paperreview/250711/Fig.1.jpg)

1. Intent layer는 Service Orchestrator (application layer)와 SDN Controller 사이에 위치하는 미들웨어로서 작동

1. 수행 역할
    - 인텐트 번역 및 이행 (Fullfilment & Verification)
        - 애플리케이션의 고수준 인텐트 (서비스 체인 요구사항 등)를 받아 네트워크에서 실행 가능한 저수준의 구성 작업으로 번역하고, 현재 네트워크 상태에서 해당 intent의 이행이 가능한지 검증
        - 가능하다면 SDN Controller와 상호작용하여 네트워크 경로를 설정하고 VNF 체인 구성
    - 인텐트 보증 및 유효성 검사 (Assurance & Validation)
        - 설정된 서비스 체인이 intent에 명시된 요구사항 (ex : 특정 대역폭 보장)을 지속적으로 만족하는지 네트워크 상태를 모니터링하여 검증
        - 네트워크 상태 변화 (ex : 스위치 과부하)로 인해 요구사항이 충족되지 못할 경우, intent 계층이 자율적으로 (zero-touch) 감지하고 서비스 체인 경로를 재설정하는 등의 복구/최적화 작업을 트리거
    
    > 애플리케이션의 추상적인 요구사항이 서비스 오케스트레이터와 SDN 컨트롤러를 통해 실제 네트워크 구성으로 구현되는 기본적인 흐름을 보여주며, 논문에서 제안하는 인텐트 계층은 이 과정에서 고수준의 사용자/애플리케이션 인텐트를 네트워크 구성으로 번역하고, 그 성능을 지속적으로 보증하는 핵심적인 역할을 수행
    > 
    
    > 특히, 이 논문은 이러한 인텐트 기반 접근 방식을 동적인 에지 클라우드 환경에서의 서비스 체인링에 적용하여, 복잡한 저수준 구성을 자동화하고 네트워크 변화에 자율적으로 대처하는 방법을 제시
    > 

## [ Core Methodology and Architecture ]

![Fig.2.jpg](/img/paperreview/250711/Fig.2.jpg)

1. 목적
    - 이 Intent layer는 SDN 기반 edge cloud network에서 애플리케이션이 추상적인 목표 (intent)를 표현하면, 이를 바탕으로 적응형 서비스 데이터 경로를 프로비저닝하고 관리하여 애플리케이션이 구체적인 기술 지침에 신경쓰지 않아도 되도록 도움
        
        → 이는 네트워크 관리의 복잡성을 줄이고 자동화를 가능하게 함
        

1. 구조
    - Intent Layer는 애플리케이션 계층 (ex : Service Orchestrators)과 SDN을 통해 네트워크를 제어하는 네트워크 계층 사이에 위치, 애플리케이션은 원하는 결과와 운영 목표를 Intent layer에 전달

1. 구성 요소
    - Application Layer : 이 layer는 Service Orchestrator와 같이 네트워크 인프라를 활용하여 애플리케이션의 목표를 달성하려는 상위 entity를 나타냄, 이들은 기술적인 세부 사항 대신 원하는 결과나 비지니스 목표를 높은 수준의 intent로 표현
    - Intent Layer : 이 논문에서 제안하는 핵심 레이어로, Application Layer의 intent를 받아 Network Layer에서 구현 가능한 설정으로 변환하고, 인텐트가 네트워크 상에서 지속적으로 유지되도록 보장
        - Intent Manager
            
            → Translation : Application Layer로부터 intent를 수신하고 파싱하여 네트워크 설정에 필요한 운영 단계로 변환
            
            → Verification : 현재 네트워크 상태를 기반으로 intent 실행 가능 여부를 확인, intent가 유효하면 Configuration and Deployment Engine을 트리거
            
        - Adaptation Module
            
            → Validation : Statistics Collector로부터 주기적으로 수집된 네트워크 상태 데이터를 모니터링하고, 설정된 intent의 성능이 지정된 요구사항에서 벗어나는지 확인
            
            → intent 유효성 검사에 실패하면, Intent Manager에게 재최적화 또는 교정 조치를 트리거하여 우너하는 성능을 회복하도록 함
            
        - Registry
            
            → 사용 가능한 가상 기능 (Virtual Function, VNF) 인스턴스 및 설정된 데이터 전달 경로에 대한 서비스 및 운영 데이터를 저장하는 데이터베이스
            
        - Statistic Collector
            
            → Network Layer의 SDN Controller와 상호작용하여 스위치의 트래픽 부하와 같은 네트워크 통계를 수집
            
            → 이 데이터는 Adaptation Module에서 사용됨
            
        - Configuration and Deployment Engine
            
            → Intent Manager 또는 Adaptation Module의 트리거에 따라 서비스 체인 경로를 따라 종단 간 서비스 데이터 경로를 프로비저닝하는 작업을 조정
            
            → SDN Controller와 상호작용하여 필요한 Flow Rules를 네트워크 스위치에 설치
            
    - Network Layer : SDN Controller와 물리적 또는 가상 스위치 (A, B, C, D, E) 및 가상 네트워크 기능 (VNF A, VNF B, VNF X, VNF Y)으로 구성된 기본 네트워크 인프라를 나타냄
        - SDN Controller
            
            → 네트워크 스위치를 중앙에서 관리하고 제어하며, Configuration and Deployment Engine으로부터 받은 지시에 따라 스위치에 플로우 규칙 설정
            
        - Switches (A-E)
            
            → OpenFlow가 활성화된 스위치로. SDN Controller의 지시에 따라 데이터를 전달
            
            → Data Plane을 형성 (파란색 선)
            
        - VNFs (A, B, X, Y)
            
            → 에지 컴퓨팅 노드에 배포된 가상 네트워크 기능으로, 서비스 체인의 구성 요소로 사용됨
            

1. 작동 방식
    - Application Layer는 Intent Layer (Intent Manager)에 높은 수준의 인텐트 전달
    - Intent Manager는 인텐트를 번역하고 현재 네트워크 상태를 Registry 및 Statistics Collector로 받은 정보를 기반으로 실행 가능성 확인
    - Configuration and Deployment Engine은 Intent Manager의 지시에 따라 SDN Controller와 통신하여 필요한 서비스 체인 경로를 Network Layer에 설정
    - Statistic Collector는 네트워크 상태를 지속적으로 모니터링하고 데이터 수집
    - Adaptation Module은 수집된 통계를 분석하여 설정된 인텐트가 잘 유지되고 있는지 검증
    - 인텐트가 요구 사항에서 벗어나면 Adaptation Module은 Intent Manager에게 알리고, Intent Manager는 Configuration and Deployment Engine을 통해 Network Layer에 교정 조치 (ex : 경로 재설정)를 적용하여 인텐트를 다시 충족시킴
        - 이 과정은 지속적인 루프 (Monitoringk, Validation, Adaptation)를 통해 자동적으로 이루어짐 = Zero-touch
    

## [ Intent 계층 운영 단계 ]

1. Awareness (네트워크 인식)
    - Statistics Collector가 SDN Controller의 NBI (Northbound Interface)를 통해 OpenFlow 통계 데이터를 수집하고 처리하여 현재 네트워크 상태를 파악
    - 논문에서는 스위치 부하를 검증 메커니즘의 핵심 지표로 사용
    - 특정 시간 t_i와 t_{i-1} 사이에 스위치 s의 포트 l에서 수신/전송된 바이트 카운터 (Rx_Bytes, Tx_Bytes)를 사용하여 링크 부하를 계산하고 (수식 1, 2), 이를 합산하여 스위치 부하를 산출 (수식 3)
    
    ![link_I.png](/img/paperreview/250711/link_I.png)
    
    - 이 부하 정보는 Grafana 같은 도구를 통해 시각화될 수 있으며, 스위치 부하의 동적 변화 및 정체 여부를 모니터링하는데 활용

![Fig.3.jpg](/img/paperreview/250711/Fig.3.jpg)

1. Fulfilment & Verification (Intent 이행 및 검증)
    - Intent Manager가 Intent Template를 번역하여 필요한 파라미터를 추출하고 (Translation)
    - 현재 네트워크 상태를 기반으로 Intent 배포의 실현 가능성 (Verification)을 검증
    - 특히, 소스 및 목적지 스위치와 서비스 체인을 구성하는 VNF들이 연결된 스위치들의 현재 부하를 확인하여 특정 임계값 이하인지 검사
    - 모든 스위치가 요구 사항을 충족하면 Intent가 수락되고, Configuration and Deployment Engine을 통해 네트워크에 플로우 규칙이 설정

1. Assurance & Validation (Intent 보장 및 유효성 검증)
    - Validity Verification : Adaptation Module이 수집된 네트워크 통계를 주기적으로 검사하여 구축된 Intent의 상태가 사용자 요구 사항과 일치하는지 지속적으로 확인
    - 동적인 네트워크 상태 변화 (ex : 스위치 부하 증가)로 인해 Intent의 성능이 저하될 경우, Adaptation Module은 정체된 스위치 목록을 Intent Manager에게 통보하여 복구 작업을 트리거
    - Intent Manager는 해당 Intent의 실행 가능성을 다시 확인한 후, Configuration and Deployment Engine에게 정체된 스위치를 통과하는 데이터 경로를 다른 비정체 스위치로 리디렉션하도록 지시
        - 이 과정에서 기존 플로우 규칙이 삭제되고 새로운 경로에 규칙 재설정
            
            ![Fig.5.jpg](/img/paperreview/250711/Fig.5.jpg)
            

## [ Intent  Northbound Interface ]

1. Application Layer와 Intent Layer는 RESTful Interface를 통해 상호작용
2. 논문에서 정의된 두 가지 유형의 Intent
    - Simple Intent
        - 소스 및 목적지 엔드포인트만 지정하여 특정 Throughput을 가진 경로 설정
    - Composite Intent
        - 엔드포인트 외에도 통과해야 할 가상 기능 (VNF)의 수와 유형을 지정
        - Intent는 XML 기반의 Template 형식으로 표현되며, Source IP, Destination IPk, Chain (Length, Node_ID, Order), Throughput과 같은 파라미터 포함
        
        ![Listing.1.jpg](/img/paperreview/250711/Listing.1.jpg)
        

## [ 성능 평가 ]

1. Mininet 에뮬레이션 환경 (Abilene 토폴로지, Floodlight Controller)에서 제안된 프레임워크의 성능 평가
    - Intent Enforcement 성능
        - 클라우드 플랫폼 연결 스위치 수 변화에 따른 플로우 설정 시간은 상대적으로 일정
            
            ![Fig.6.jpg](/img/paperreview/250711/Fig.6.jpg)
        
        - 클라우드 플랫폼 연결 스위치 수가 증가할수록 플로우 엔트리 수는 감소
            
            ![Fig.7.jpg](/img/paperreview/250711/Fig.7.jpg)
        
        - RTT는 클라우드 플랫폼 연결 스위치 수가 증가할수록 약간 감소
            
            → 이는 서비스가 사용자에게 더 가까워짐을 의미하지만, VNF 복제를 통해 적은 수의 엣지 클라우드만으로도 합리적인 RTT를 얻을 수 있음을 시사하며 배포 비용과 QoE 간의 절충점을 보여줌
            
            ![Fig.8.jpg](/img/paperreview/250711/Fig.8.jpg)
        
        - 서비스 체인 길이 증가는 플로우 설정 시간, 플로우 엔트리 수, RTT를 증가
        
        ![Fig.11.jpg](/img/paperreview/250711/Fig.11.jpg)
        
        ![Fig.9.jpg](/img/paperreview/250711/Fig.9.jpg)
        
        ![Fig.10.jpg](/img/paperreview/250711/Fig.10.jpg)
        

- Intent Layer 검증 및 유효성 검증 성능
    - 검증 및 유효성 검증 메커니즘을 도입했을 때, 높은 부하에서도 차단 확률(Blocking Probability, BP)이 감소하고 안정적으로 유지됨
        
        → 이는 정체된 스위치에서 흐름을 리디렉션하여 사용 가능한 스위치 전반에 부하를 더 잘 분산하기 때문
        
    - 검증 및 유효성 검증을 적용했을 때, 스위치 간의 평균 부하 분포가 훨씬 균등해짐
    - 네트워크 부하가 증가함에 따라 리디렉션 횟수는 선형적으로 증가했지만, 검증 기능이 활성화된 경우(요청 수락 감소) 리디렉션 횟수가 감소
    - 리디렉션 시간은 네트워크 부하와 무관하게 거의 일정
    - 유효성 검증을 적용했을 때, 특히 검증이 비활성화된 경우(더 많은 리디렉션 발생) Intent Layer와 SDN Controller 간의 메시지 교환 수가 증가
        
![Fig.12.jpg](/img/paperreview/250711/Fig.12.png)
        
![Fig.13_16.png](/img/paperreview/250711/Fig.1316.png)
    

## [ Conclusion ]

1. 본 논문은 SDN 기반 엣지 클라우드 네트워크를 위한 Intent 기반 서비스 체인 계층을 제안
2. 이 계층은 Template 기반 Intent를 통해 사용자가 높은 수준에서 서비스 목표를 표현할 수 있도록 하여 복잡한 네트워크 구성을 추상화
3. 또한, 네트워크 상태를 지속적으로 모니터링하고 스위치 정체와 같은 성능 저하 발생 시 서비스 체인 경로를 자동으로 조정하는 Zero-Touch 보장 기능을 제공
4.  에뮬레이션 실험을 통해 제안된 방식의 타당성과 효과성(낮은 차단 확률, 균등한 부하 분산 등)을 입증
5. 향후 연구로는 보다 정교한 보장 파라미터 도입, 정형화된 Intent 언어 개발, 다양한 네트워크 토폴로지 및 트래픽 패턴 검토 등이 필요