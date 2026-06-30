# Chuong 01 - Project setup

## Muc tieu

Chay duoc backend Spring Boot, ket noi MySQL va Redis bang Docker Compose, co cau hinh rieng cho moi truong local.

## Can hieu

Ung dung chat can 3 lop ha tang:

- Spring Boot xu ly REST API va WebSocket.
- MySQL luu du lieu ben vung: user, room, message.
- Redis luu du lieu ngan han: session WebSocket, online users, cache.

Redis khong thay MySQL. Redis giup realtime nhanh hon, nhung message cuoi cung van phai luu vao MySQL.

## Bai thuc hanh

### 1. Kiem tra backend hien tai

Chay tai thu muc backend:

```bash
cd backend
./mvnw test
```

Tren Windows PowerShell:

```powershell
cd backend
.\mvnw.cmd test
```

Neu test fail vi chua cau hinh database, tam thoi chua sua voi muc tieu "lam pass bang moi gia". Hay doc loi de biet app dang can datasource hay dependency nao.

### 2. Tao Docker Compose

Tao `docker-compose.yml` o root `chatapp`. Noi dung nen co MySQL va Redis:

```yaml
services:
  mysql:
    image: mysql:8.0
    environment:
      MYSQL_DATABASE: chatapp
      MYSQL_USER: chatapp
      MYSQL_PASSWORD: chatapp
      MYSQL_ROOT_PASSWORD: root
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql

  redis:
    image: redis:8
    ports:
      - "6379:6379"

volumes:
  mysql_data:
```

Hay tu go lai file nay. Muc tieu la nho duoc database name, username, password va port.

### 3. Cau hinh application local

Dung `application.properties` hoac tach thanh `application-dev.properties`. Vi du:

```properties
spring.application.name=chatapp

spring.datasource.url=jdbc:mysql://localhost:3306/chatapp
spring.datasource.username=chatapp
spring.datasource.password=chatapp

spring.jpa.hibernate.ddl-auto=validate
spring.jpa.open-in-view=false

spring.flyway.enabled=true
```

Khong dung `ddl-auto=update` cho khoa hoc nay. Ta muon hoc migration ro rang bang Flyway.

## Tu kiem tra

- `docker compose up -d` chay duoc MySQL va Redis.
- Backend start toi muc ket noi datasource.
- Ban giai thich duoc vi sao MySQL va Redis co vai tro khac nhau.

## Loi hay gap

- Port 3306 da bi MySQL local chiem: doi thanh `"3307:3306"`.
- Sai password datasource.
- Chua bat Docker Desktop.
- Spring Boot start fail vi chua co migration, se xu ly o chuong 02.

