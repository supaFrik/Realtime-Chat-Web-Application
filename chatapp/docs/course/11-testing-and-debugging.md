# Chuong 11 - Testing va debugging

## Muc tieu

Viet test vua du de tu tin sua backend chat. Khong can test moi getter/setter, nhung can test rule bao ve du lieu va realtime flow quan trong.

## Can hieu

Chat app co nhieu bug do race condition va permission. Test tot nen tap trung vao:

- User khong phai member khong gui/xem message.
- Message duoc luu truoc khi broadcast.
- Pagination khong lap message.
- Presence dung khi multi-session.

## Bai thuc hanh

### 1. Test service truoc

Vi du `MessageServiceTest`:

```java
@Test
void nonMemberCannotSendMessage() {
    // arrange: room co Alice, Bob khong nam trong room
    // act: Bob gui message
    // assert: nem AccessDeniedException
}
```

Hay viet test theo comment truoc, roi moi code arrange that.

### 2. Test repository query

Dung database test neu co the. Voi MySQL, co the dung Testcontainers sau. Truoc mat, dung test slice de kiem tra query method quan trong.

### 3. Test controller REST

Dung MockMvc cho:

- Register/login.
- Tao room.
- Lay history.
- Mark read.

### 4. Debug WebSocket

Khi WebSocket loi, ghi log theo cac diem:

- CONNECT co token khong.
- Principal da duoc gan chua.
- Client subscribe destination nao.
- MessageMapping co duoc goi khong.
- Service save message co fail khong.
- convertAndSend dung topic khong.

## Tu kiem tra

- Co it nhat 1 test cho auth.
- Co it nhat 1 test cho room permission.
- Co it nhat 1 test cho message pagination.
- Ban co checklist debug WebSocket de dung khi loi.

## Loi hay gap

- Chi test happy path.
- Test qua phu thuoc vao thoi gian hien tai.
- Mock qua nhieu den muc test khong bat duoc loi query.

