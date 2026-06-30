# Chuong 10 - Notification va offline delivery

## Muc tieu

Luu su kien tin nhan moi cho user offline hoac user khong dang mo room, de client dong bo khi quay lai.

## Can hieu

Offline delivery trong chat khong nhat thiet la day push notification ngay. Buoc dau tien la backend biet user nao chua doc tin nao.

Can model:

- Message da luu trong MySQL.
- Room member co `last_read_message_id`.
- Notification event neu muon hien badge hoac danh sach thong bao.

## Bai thuc hanh

### 1. Them last read vao room member

Migration them cot:

```sql
alter table room_members
    add column last_read_message_id bigint null;
```

Khi user mo room va doc den message moi nhat, client goi:

```text
POST /api/rooms/{roomId}/read
```

Body:

```json
{"messageId": 123}
```

### 2. Tinh unread count

Khi lay danh sach room, tinh so message co id lon hon `last_read_message_id`.

Ban co the lam query don gian truoc. Khi du lieu lon moi toi uu bang denormalized counter.

### 3. Offline user

Khi message moi duoc tao:

1. Lay members cua room.
2. Bo qua sender.
3. Neu user offline, khong can gui WebSocket.
4. Khi user reconnect, REST room list se co unread count.

### 4. Notification event

Neu user online nhung khong subscribe room hien tai, co the gui:

```text
/user/queue/notifications
```

Event chi can co roomId, messageId, preview.

## Tu kiem tra

- Bob offline, Alice gui 3 tin.
- Bob login lai, room list hien unread count 3.
- Bob mark read, unread count ve 0.

## Loi hay gap

- Dong nhat offline delivery voi push notification. Day la hai viec khac nhau.
- Luu read status tren user thay vi tren room member.
- Tinh unread bang cach load tat ca message len memory.

