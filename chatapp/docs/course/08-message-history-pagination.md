# Chuong 08 - Lich su tin nhan va pagination

## Muc tieu

Lay lich su tin nhan theo room, ho tro tai them tin cu va sync sau khi reconnect.

## Can hieu

Chat history khong nen dung offset pagination qua lau. Offset cham khi du lieu lon va de bi lap/mat tin neu co message moi chen vao. Cursor pagination phu hop hon.

Cursor don gian co the la `beforeMessageId`.

## Bai thuc hanh

### 1. Endpoint history

```text
GET /api/rooms/{roomId}/messages?beforeId=123&limit=30
```

Neu khong co `beforeId`, lay 30 tin moi nhat.

### 2. Query repository

Can query:

```java
List<Message> findTop30ByRoomIdOrderByIdDesc(Long roomId);
List<Message> findTop30ByRoomIdAndIdLessThanOrderByIdDesc(Long roomId, Long beforeId);
```

Tra ve client co the dao lai theo thu tu tang dan neu UI muon hien tu cu den moi.

### 3. Response

```java
public record MessagePageResponse(
        List<MessageResponse> items,
        Long nextBeforeId,
        boolean hasMore
) {}
```

`nextBeforeId` thuong la id nho nhat trong page hien tai.

### 4. Sync khi reconnect

Client co the nho `lastSeenMessageId` moi nhat cua moi room. Sau reconnect:

```text
GET /api/rooms/{roomId}/messages?afterId=456
```

Endpoint nay co the them sau. Ban chi can ghi vao backlog neu chuong nay qua dai.

## Tu kiem tra

- Room co 100 message.
- Goi lan dau tra 30 message moi nhat.
- Dung `nextBeforeId` de lay 30 message cu hon.
- User khong phai member khong xem duoc history.

## Loi hay gap

- Quen sort, moi lan query ra thu tu khac nhau.
- Dung offset khi message tang lien tuc.
- Tra ve entity truc tiep lam lo field noi bo.

