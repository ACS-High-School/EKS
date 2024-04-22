# EKS
- AWS EKS에 Argocd를 통한 web, was 배포
- hpa를 통한 고가용성과 효율적인 자원 관리
- ALB를 활용한 ingress 배포
- Readiness probe를 활용한 지속적인 배포

<br>

## Architecture
<img width="1351" alt="image" src="https://github.com/ACS-High-School/EKS/assets/94664341/c74cdc8e-afeb-428b-a589-7ae4d9a9a98b">


<br>

## Technologies

- [EKS](https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/kubernetes-versions.html) 1.29.0
- [Application Load Balancer](https://docs.aws.amazon.com/ko_kr/elasticloadbalancing/latest/application/introduction.html)
- [Amazon EBS CSI DRIVER](https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/ebs-csi.html)

## Trouble Shooting 
### 웹 서비스 배포 과정에서의 5XX 에러
- 문제 원인
  - Replica 설정과 HPA 설정의 충돌 발생
  - RollingUpdate 배포에 대한 추가 설정 필요

- 해결 방안
  - ReadinessGate 설정으로 ALB 에서 Pod 의 상태에 따른 트래픽 조정
  - Readiness / Liveness Probe 를 통한 클러스터에서의 Pod 상태 확인
  - PreStop 설정을 통한 Draining 상태에 대한 Pod 접근 제어

### Jenkins를 이용한 ECR 빌드 시 오류
- 문제 원인
  - Jenkins secret file에 업로드한 파일의 권한 문제

- 해결 방안
  - sudo cp를 통해 빌드 시 필요한 파일 경로에 복사
  - chmod 명령어를 통해 권한 부여 후 이미지 빌드 작업 수행

### ALB 404 문제 발생
- 문제 원인
  - ALB를 통한 Web, Was pod 접속 시 404 에러 발생
  - ALB의 리스너 규칙을 수정하여도 시간이 지나면 초기 설정으로 변경


- 해결 방안
  - ingress.yml 파일의 host 도메인을 정확히 정의하여 해결
  
<br>
<br>

## Feature improvements
### EKS 클러스터 구성에서의 비용 최적화 처리 미흡
- 개선 이유
  - 비용 최적화 문제
  
- 개선 방안
  - 다중 AZ 환경에서의 네트워크 트래픽 비용 최적화
  - IP Mode 를 통해 노드 간의 통신 설정
  - topology aware hints, istio 를 통한 노드 안에서의 pod 통신 설정
  - Karpenter 를 통한 인스턴스 타입 설정

  
### CI / CD 파이프라인 관련 추가 설정 필요
- 개선 이유
    - 애플리케이션 배포 시 빌드 최적화 필요
    - 배포 전 테스트 단계 구축 미흡


- 개선 방안
    - WebPack 을 통한 React 애플리케이션 빌드 최적화
    - JUnit 을 통한 Spring boot 애플리케이션 테스트 항목 추가
    - 빌드할 이미지 용량 줄이기


<br>
<br>

## Contribution
- 🫠 [김선우](https://github.com/sw801733)
- 🫢 [홍준표](https://github.com/hjp1016)
