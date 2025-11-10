## EC2 (Elastic Compute Cloud)

---

### 🧩 개념

> AWS에서 제공하는 가상 서버(컴퓨팅 인스턴스) 서비스
> 
> 
> 사용자가 필요할 때 원하는 사양의 서버를 빠르게 생성·구성·관리할 수 있도록 하는
> 
> **확장 가능하고 안전한 클라우드 컴퓨팅 서비스**
> 
- **Elastic (탄력적)**
    
    → 트래픽이나 부하에 따라 서버를 **자동으로 확장/축소 가능**
    
- **Compute (연산 자원)**
    
    → CPU, RAM, Disk 등 물리 서버 자원을 가상화하여 제공
    
- **Cloud (클라우드 환경)**
    
    → 사용량 기반 과금(Pay-as-you-go), 인프라 유지보수 불필요
    

---

### ⚙️ 주요 개념

| 용어 | 설명 |
| --- | --- |
| **인스턴스 (Instance)** | 실제 클라우드 상에서 실행 중인 **가상 서버** |
| **AMI (Amazon Machine Image)** | OS와 설정이 포함된 서버 이미지 (Ubuntu, Amazon Linux 등) |
| **Instance Type** | CPU, 메모리, 네트워크 성능을 조합한 서버 사양 (예: t2.micro, t3.small 등) |
| **EBS (Elastic Block Store)** | 인스턴스에 연결되는 디스크 (하드디스크 역할) |
| **Elastic IP** | 고정 IP 주소, 인스턴스 재시작 시에도 변경되지 않음 |
| **Security Group** | 인스턴스에 대한 방화벽 규칙 (인바운드/아웃바운드 제어) |
| **Key Pair (.pem)** | SSH 접속용 공개/비공개 키 파일, 인증 수단 역할 |

---

### 📦 Elastic의 의미

> EC2의 핵심은 “유연한 확장성(Elasticity)”
> 
> 
> → 사용량이 급증하면 자동으로 인스턴스를 늘리고,
> 
> → 부하가 줄면 자동으로 줄여 비용 효율을 유지
> 
- **Auto Scaling Group (ASG)**: 트래픽 증가 시 자동 인스턴스 생성
- **Elastic Load Balancer (ELB)**: 여러 인스턴스에 요청을 자동 분산
- **CloudWatch**: CPU 사용량·네트워크 트래픽 감시 후 트리거 설정 가능

---

### ⚙️ 사용 방법 (실습 흐름)

### 1️⃣ 인스턴스 생성

- AWS 콘솔 → EC2 → “인스턴스 시작(Launch Instance)”
- AMI 선택 (예: Ubuntu 22.04, Amazon Linux 2 등)
- 인스턴스 타입 선택 (t2.micro 프리티어 가능)
- 키 페어(.pem) 생성 또는 선택
- 보안 그룹 설정 (예: SSH 22, HTTP 80, HTTPS 443 포트 열기)
- 스토리지(EBS) 크기 설정 → 인스턴스 시작

### 2️⃣ SSH 접속 (Mac/Linux 기준)

```bash
chmod 400 my-key.pem
ssh -i "my-key.pem" ubuntu@<EC2-퍼블릭-IP>

```

> Windows에서는 PuTTY, VSCode SSH Extension, MobaXterm 등 사용 가능
> 

### 3️⃣ 웹 서버 구성 (예시)

```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl start nginx

```

→ 브라우저에서 `http://<EC2-IP>` 접속 시 Nginx 기본 페이지 확인 가능

### 4️⃣ 고정 IP 할당 (Elastic IP)

- EC2 → “Elastic IP 주소” → 새 주소 할당
- 인스턴스에 연결

> 💬 일반 퍼블릭 IP는 인스턴스 재시작 시 변경됨.
> 
> 
> 운영 서버는 반드시 Elastic IP 사용.
> 

### 5️⃣ 스냅샷 / AMI 백업

- 서버 세팅 완료 후, AMI를 생성해두면 동일 설정 서버를 손쉽게 복제 가능
- EBS 스냅샷을 통해 데이터 백업 가능

---

### 🔐 주의 사항

- 인스턴스 IP는 **재부팅 시 변경될 수 있음** → 하드코딩 금지
- 보안 그룹은 반드시 최소 권한 원칙 적용 (SSH는 관리자 IP만 허용)
- 프리티어 사용 시 CPU 초과 사용에 주의 (Burst 한도 초과 시 중지됨)
- 인스턴스 중지해도 EBS는 과금 계속됨 (삭제하지 않으면 디스크 요금 발생)

---

### 💡 활용 예시

- 웹 서버(Nginx, Apache, Spring Boot 등)
- 백엔드 애플리케이션 서버
- 데이터 분석/AI 모델 서빙 서버
- CI/CD 빌드 서버 (Jenkins 등)
- 테스트용 임시 서버 환경

---

### ✅ 요약

> EC2 = AWS의 가상 컴퓨터
> 
> - 필요할 때 즉시 생성, 삭제 가능
> - 탄력적 확장(Elastic), 사용량 기반 과금
> - 다양한 OS·사양 조합으로 워크로드 맞춤 구성
> - 실제 물리 서버와 동일한 환경에서 애플리케이션 실행 가능
