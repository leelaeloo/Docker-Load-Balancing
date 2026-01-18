# Docker Load Balancing

> Nginx를 활용한 라운드로빈 로드밸런싱 실습

<p align="center">
  <img src="https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker" alt="Docker" />
  <img src="https://img.shields.io/badge/Nginx-Alpine-009639?logo=nginx" alt="Nginx" />
  <img src="https://img.shields.io/badge/Load%20Balancing-Round%20Robin-FF6B6B" alt="Load Balancing" />
</p>

---

## 프로젝트 소개

여러 대의 웹 서버에 트래픽을 분산시키는 **로드밸런싱**을 Docker Compose와 Nginx로 구현한 실습 프로젝트입니다.

### 학습 목표

- Docker Compose로 멀티 컨테이너 환경 구성
- Nginx upstream을 활용한 로드밸런싱 설정
- 라운드로빈 방식의 트래픽 분산 이해

---

## 아키텍처

```
                    ┌─────────────────┐
                    │     Client      │
                    │   (Browser)     │
                    └────────┬────────┘
                             │
                             ▼ :80
                    ┌─────────────────┐
                    │     Nginx       │
                    │  Load Balancer  │
                    └────────┬────────┘
                             │
           ┌─────────────────┼─────────────────┐
           ▼                 ▼                 ▼
    ┌─────────────┐   ┌─────────────┐   ┌─────────────┐
    │  Web Server │   │  Web Server │   │  Web Server │
    │      1      │   │      2      │   │      3      │
    │   (Blue)    │   │    (Red)    │   │   (Green)   │
    │    :8001    │   │    :8002    │   │    :8003    │
    └─────────────┘   └─────────────┘   └─────────────┘
```

---

## 폴더 구조

```
Docker-Load-Balancing/
├── docker-compose.yml       # 컨테이너 오케스트레이션
├── nginx.conf               # 로드밸런서 설정
├── webserver1/              # 웹서버 1 (Blue)
│   ├── Dockerfile
│   └── index.html
├── webserver2/              # 웹서버 2 (Red)
│   ├── Dockerfile
│   └── index.html
└── webserver3/              # 웹서버 3 (Green)
    ├── Dockerfile
    └── index.html
```

---

## 빠른 시작

### 실행

```bash
# 클론
git clone https://github.com/leelaeloo/Docker-Load-Balancing.git
cd Docker-Load-Balancing

# 빌드 및 실행
docker compose up -d --build

# 상태 확인
docker compose ps
```

### 종료

```bash
docker compose down
```

---

## 접속 방법

| URL | 설명 |
|-----|------|
| http://localhost | 로드밸런서 (Round Robin) |
| http://localhost:8001 | Web Server 1 (Blue) |
| http://localhost:8002 | Web Server 2 (Red) |
| http://localhost:8003 | Web Server 3 (Green) |

> 로드밸런서 주소를 새로고침하면 1 → 2 → 3 → 1 순서로 서버가 응답합니다.

---

## 기술 스택

| 구분 | 기술 |
|------|------|
| **컨테이너** | Docker, Docker Compose |
| **로드밸런서** | Nginx (upstream) |
| **웹서버** | Nginx Alpine |
| **네트워크** | Docker Bridge Network |

---

## Nginx 설정

```nginx
upstream backend {
    server web1:80;
    server web2:80;
    server web3:80;
}

server {
    listen 80;

    location / {
        proxy_pass http://backend;
    }
}
```

---

## 참고 자료

- [Docker Compose 공식 문서](https://docs.docker.com/compose/)
- [Nginx Load Balancing](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/)
