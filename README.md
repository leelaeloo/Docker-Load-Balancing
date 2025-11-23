# Load Balancing Example (Nginx + Docker Compose)

> 간단한 라운드로빈 방식의 로드밸런싱 예제 프로젝트입니다.

---

## 프로젝트 구조

```pqsql
load-balancing-pro/
├── docker-compose.yml
├── nginx.conf
├── webserver1/
│   ├── Dockerfile
│   └── index.html
├── webserver2/
│   ├── Dockerfile
│   └── index.html
└── webserver3/
    ├── Dockerfile
    └── index.html
```

---

## 실행 방법

```bash
docker compose up -d --build
```

## 접속 방법

### 로드밸런서 (Round Robin)

```arduino
http://localhost
```

- 새로고침할 때마다
- 1 ➡️ 2 ➡️ 3 ➡️ 다시 1 순서로 서버가 응답합니다.

### 개별 서버 직접 접속

```arduino
http://localhost:8001   (Web Server 1 - Blue)
http://localhost:8002   (Web Server 2 - Red)
http://localhost:8003   (Web Server 3 - Green)
```

---
