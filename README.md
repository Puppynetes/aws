<img src="https://capsule-render.vercel.app/api?type=waving&color=00C3FF&height=150&section=header" width="1000" />

<div align="center">
<h1 style="font-size: 36px;">🌱 Docker & K8s를 활용한<br> SpringBoot App 배포</h1>
</div>
</br>

## 목차
1. [🙆🏻‍♂️ 팀원](#%EF%B8%8F-팀원)
2. [🌱 프로젝트 개요: Docker & K8s를 활용한 SpringBoot App 배포](#-프로젝트-개요-docker--k8s를-활용한-springboot-app-배포)
3. [#️⃣ 실습 과정](#%EF%B8%8F⃣-실습-과정)
4. [📖 배운 점](#-배운-점)
5. [💜 회고](#-회고)

## 🙆🏻‍♂️ 팀원

#### 팀명 : Puppynetes

|<img src="https://avatars.githubusercontent.com/u/87555330?v=4" width="150" height="150"/>|<img src="https://avatars.githubusercontent.com/u/103871252?v=4" width="150" height="150"/>|<img src="https://avatars.githubusercontent.com/u/179544856?v=4" width="150" height="150"/>|<img src="https://avatars.githubusercontent.com/u/98368034?v=4" width="150" height="150"/>|
|:-:|:-:|:-:|:-:|
|김민성<br/>[@minsung159357](https://github.com/minsung159357)|김우현<br/>[@woody6624](https://github.com/woody6624)|이은준<br/>[@2EunJun](https://github.com/2EunJun)|장수현<br/>[@Aunsxm](https://github.com/Aunsxm)|

## #️⃣ 실습 과정

### 1️⃣ 가상머신에 AWS CLI 설치

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```
---

### 2️⃣ VM에 원하는 **AWS 계정의 IAM 사용자/역할 자격증명 설정**

```bash
aws configure
```

입력할 값:

```
AWS Access Key ID     [AKIAxxxxxxxxxxxx]
AWS Secret Access Key [xxxxxxxxxxxxxxxxxxxxxxxxxxxx]
Default region name   [ap-northeast-2]  # 예시: 서울 리전
Default output format [json]
```

> 이 자격증명은 **EKS 클러스터 생성 권한이 있는 IAM 계정**이어야 해요 (`eks:*`, `ec2:*`, `iam:*`, `cloudformation:*` 등 필요)

---

### 3️⃣ eksctl 설치

```bash
curl -LO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz"
tar -xzf eksctl_Linux_amd64.tar.gz
sudo mv eksctl /usr/local/bin
```

확인:

```bash
eksctl version
```

---

### 4️⃣ EKS 클러스터 생성

예시:

```bash
eksctl create cluster \
  --name my-cluster \
  --region ap-northeast-2 \
  --nodegroup-name linux-nodes \
  --node-type t3.medium \
  --nodes 2 \
  --managed
```

---

### 5️⃣ `kubectl` 설치 및 연결

```bash
curl -LO "https://dl.k8s.io/release/$(curl -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin
```

EKS 연결:

```bash
aws eks --region ap-northeast-2 update-kubeconfig --name my-cluster
```

---

## ✅ 요약하면

| 구성 요소 | 설명 |
|-----------|------|
| AWS CLI   | 자격증명 및 EKS 연동 |
| eksctl    | 클러스터 생성 툴 |
| IAM 권한 | 대상 AWS 계정에 클러스터 생성 가능한 권한 필요 |
| kubectl   | 클러스터 탐색용 |

---

필요하시면 여러 AWS 계정 프로파일 설정법 (`~/.aws/credentials` 다중 계정 관리)도 알려드릴게요.  
혹시 **EKS 설치는 한 번만 하고, 그걸 계속 Prometheus에서 모니터링하려는 상황**인가요?

---


