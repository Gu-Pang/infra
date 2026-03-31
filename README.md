‘Gu-Pang’ 프로젝트는 여러 도메인 서버로 분리되어 있지만, 데이터베이스는 모두 동일한 서버를 바라보는 구조를 사용하고 있습니다. 따라서 다른 도메인을 본인의 PC로 내려받아서 연동 테스트를 진행할 때에도 동일한 환경을 유지해야 합니다.

이를 위해 모든 팀원은 개별적으로 DB를 띄우지 않고, 아래의 단일 `docker-compose` 파일을 사용하여 공통된 데이터베이스 인프라를 기동해야 합니다.

## 명령어 가이드

### 실행 (백그라운드 모드)

```bash
docker-compose -f docker-compose.infra.yaml up -d
```

### 중지

```bash
docker-compose -f docker-compose.infra.yaml down
```

### 중지 및 데이터 초기화

```bash
docker-compose -f docker-compose.infra.yaml down -v
```

### 로그 확인

```bash
docker-compose -f docker-compose.infra.yaml logs -f
```

### 예시) 해당 DB와 연동하는 Hub 서버의 docker-compose.yaml 파일
```
services:
  app:
    build: .
    ports:
      - "8080:8080"
    env_file:
      - .env
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://gupang-db:5432/${DB_USERNAME}
    networks:
      - gupang-network

networks:
  gupang-network:
    external: true
```
