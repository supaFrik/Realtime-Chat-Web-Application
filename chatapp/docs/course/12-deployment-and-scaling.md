# Chuong 12 - Deployment va scaling

## Muc tieu

Dong goi app bang Docker Compose va hieu cac diem can chuan bi khi scale backend chat.

## Can hieu

Realtime app kho hon REST app o cho connection song lau. Khi scale nhieu instance:

- Load balancer can xu ly WebSocket.
- Session online khong nam trong memory cuc bo.
- Message broadcast can di qua Redis pub/sub hoac broker.
- Database can index tot cho room/message.

## Bai thuc hanh

### 1. Dockerfile backend

Tao Dockerfile build jar va run bang JDK runtime. Giai do hoc co the dung multi-stage don gian.

### 2. Docker Compose day du

Compose nen co:

- backend
- mysql
- redis

Backend doc bien moi truong:

```text
SPRING_DATASOURCE_URL
SPRING_DATASOURCE_USERNAME
SPRING_DATASOURCE_PASSWORD
SPRING_DATA_REDIS_HOST
SPRING_DATA_REDIS_PORT
```

### 3. Health check

Them Actuator neu can:

```text
GET /actuator/health
```

Health check giup Docker va deployment biet app da san sang chua.

### 4. Scaling mindset

Ban chua can scale that ngay. Hay ghi vao note:

- Instance A nhan message cua Alice.
- Bob dang ket noi instance B.
- Lam sao B biet co message moi?

Cau tra loi production thuong la Redis pub/sub, RabbitMQ, Kafka hoac external broker. Khoa nay co the dung Redis lam buoc dau.

## Tu kiem tra

- `docker compose up` chay duoc backend, MySQL, Redis.
- App start bang config tu environment.
- Ban giai thich duoc vi sao memory local khong du de quan ly online users khi scale.

## Loi hay gap

- Hard-code localhost trong container. Trong Docker network, backend ket noi `mysql`, `redis`, khong phai `localhost`.
- Quen expose WebSocket qua reverse proxy.
- Khong index `messages(room_id, id)`.

