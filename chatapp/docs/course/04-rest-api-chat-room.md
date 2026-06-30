# Chuong 04 - REST API phong chat

## Muc tieu

Tao API de user co the tao phong chat 1-1, tao nhom, xem danh sach phong va quan ly thanh vien co ban.

## Can hieu

REST API dung cho hanh dong khong can realtime ngay lap tuc:

- Tao room.
- Them/xoa member.
- Lay danh sach room cua user.
- Lay thong tin room.

Gui message realtime se xu ly bang WebSocket o chuong sau.

## Bai thuc hanh

### 1. Thiet ke endpoint

Goi y:

```text
POST   /api/rooms/direct
POST   /api/rooms/groups
GET    /api/rooms
GET    /api/rooms/{roomId}
POST   /api/rooms/{roomId}/members
DELETE /api/rooms/{roomId}/members/{userId}
```

### 2. Tao request/response DTO

Vi du:

```java
public record CreateDirectRoomRequest(Long targetUserId) {}

public record CreateGroupRoomRequest(
        String name,
        List<Long> memberIds
) {}

public record RoomSummaryResponse(
        Long id,
        String type,
        String name,
        int memberCount
) {}
```

DTO khong can giong entity. DTO la hop dong voi client, entity la cach backend luu du lieu.

### 3. Viet RoomService

Luon lay current user tu SecurityContext, khong nhan `currentUserId` tu request body.

Rule toi thieu:

- Direct room can dung 2 user.
- Group room can co ten va it nhat 2 member.
- Chi member cua room moi xem duoc room.
- Chi nguoi tao group hoac admin moi them/xoa member. Ban co the de rule don gian truoc.

### 4. Them repository query

Can query room theo user:

```java
List<RoomMember> findByUserId(Long userId);
boolean existsByRoomIdAndUserId(Long roomId, Long userId);
```

Voi direct room, de don gian co the cho phep tao trung trong lan dau. Sau do refactor de tim room direct da co.

## Tu kiem tra

- Alice tao direct room voi Bob.
- Alice tao group co Bob va Charlie.
- User khong nam trong room bi reject khi xem room.

## Loi hay gap

- Tin vao userId client gui len.
- Tra entity JPA truc tiep ra API.
- Query danh sach room gay N+1 qua som. Dau tien lam dung, sau do toi uu.

