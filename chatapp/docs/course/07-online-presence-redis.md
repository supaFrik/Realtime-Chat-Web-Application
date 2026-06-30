# Chuong 07 - Online presence voi Redis

## Muc tieu

Theo doi user online/offline dua tren WebSocket session va Redis.

## Can hieu

Online status khong nen chi luu trong memory cua mot instance backend. Khi scale nhieu backend, user co the connect vao instance khac nhau. Redis giup cac instance cung nhin thay trang thai chung.

Can phan biet:

- User online: co it nhat mot WebSocket session active.
- User offline: khong con session active nao.
- Multi-device: mot user co the mo laptop va dien thoai cung luc.

## Bai thuc hanh

### 1. Them Redis dependency

Them Spring Data Redis neu project chua co. Muc tieu la dung `StringRedisTemplate` truoc, chua can object serialization phuc tap.

### 2. Thiet ke key

Goi y:

```text
presence:user:{userId}:sessions -> set session ids
presence:session:{sessionId} -> user id
```

Dat TTL cho session key de tranh session chet khong duoc xoa.

### 3. Lang nghe WebSocket event

Spring co event:

- `SessionConnectEvent`
- `SessionDisconnectEvent`

Khi connect:

1. Lay user id tu Principal.
2. Lay session id tu header.
3. Add session id vao Redis set cua user.
4. Broadcast user online neu day la session dau tien.

Khi disconnect:

1. Xoa session id.
2. Neu set rong, broadcast user offline.

### 4. Broadcast presence

Goi y destination:

```text
/topic/presence
```

Event:

```java
public record PresenceEvent(
        Long userId,
        String status
) {}
```

## Tu kiem tra

- Mo 2 client cung user: disconnect 1 client thi user van online.
- Disconnect tat ca client thi user offline.
- Restart backend: Redis TTL giup session cu khong song mai.

## Loi hay gap

- Luu online flag boolean, khong xu ly multi-device.
- Khong dat TTL.
- Broadcast offline ngay khi mot tab dong, trong khi user con tab khac.

