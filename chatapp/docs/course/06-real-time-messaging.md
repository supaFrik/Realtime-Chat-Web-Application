# Chuong 06 - Real-time messaging

## Muc tieu

Gui tin nhan vao room qua WebSocket, luu vao MySQL, sau do broadcast den cac member dang subscribe.

## Can hieu

Thu tu nen la:

1. Client gui message den `/app/rooms/{roomId}/messages`.
2. Server xac thuc user va quyen trong room.
3. Server validate noi dung.
4. Server luu message vao MySQL.
5. Server publish message den `/topic/rooms/{roomId}`.

Khong broadcast truoc roi moi luu, vi neu save fail client se thay message khong ton tai trong history.

## Bai thuc hanh

### 1. DTO WebSocket

```java
public record SendMessageCommand(
        String clientMessageId,
        String type,
        String content
) {}

public record MessageEvent(
        Long id,
        Long roomId,
        Long senderId,
        String senderName,
        String type,
        String content,
        String createdAt
) {}
```

`clientMessageId` giup frontend doi chieu message optimistic sau nay.

### 2. Handler gui tin

Y tuong:

```java
@MessageMapping("/rooms/{roomId}/messages")
public void sendMessage(@DestinationVariable Long roomId,
                        SendMessageCommand command,
                        Principal principal) {
    MessageEvent event = messageService.saveMessage(roomId, principal, command);
    messagingTemplate.convertAndSend("/topic/rooms/" + roomId, event);
}
```

### 3. Service rule

`messageService.saveMessage` nen lam:

- Lay user tu principal.
- Kiem tra user la member cua room.
- Reject content rong voi type `TEXT`.
- Luu entity `Message`.
- Map sang `MessageEvent`.

### 4. Subscribe theo room

Client nao dang o room nao thi subscribe:

```text
/topic/rooms/{roomId}
```

Khong subscribe tat ca room neu user co nhieu room. Viec do ton tai nguyen va kho kiem soat.

## Tu kiem tra

- Alice va Bob cung subscribe room.
- Alice send message.
- Bob nhan event realtime.
- Refresh app va lay history thay message vua gui.

## Loi hay gap

- Bo qua check member, user co the gui vao room bat ky.
- Dung local time string lung tung. Nen luu `Instant` hoac timestamp UTC.
- Tra ve message khac format giua REST history va WebSocket event.

