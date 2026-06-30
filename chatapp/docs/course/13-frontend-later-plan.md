# Chuong 13 - Frontend later plan

## Muc tieu

Ghi lai ke hoach frontend Next.js 15 de lam sau. Hien tai backend la uu tien.

## Pham vi frontend sau nay

Frontend nen gom cac man hinh:

- Login/Register.
- Danh sach room.
- Khung chat.
- Tao group.
- Member list.
- Upload file preview.
- Online indicator.

## Kien truc Next.js de xuat

- App Router.
- TypeScript.
- REST client cho auth, room, history, upload.
- STOMP client cho realtime message va presence.
- State local ban dau, sau do co the them TanStack Query hoac store nhe.

## Luong client

### Login

1. User login bang REST.
2. Luu access token theo chien luoc an toan phu hop.
3. Load danh sach room.
4. Connect WebSocket voi token.

### Mo room

1. Load history REST.
2. Subscribe `/topic/rooms/{roomId}`.
3. Mark read message moi nhat.
4. Render message realtime khi event den.

### Gui file

1. Chon file va preview.
2. Upload REST.
3. Send WebSocket message type `FILE`.

## Design direction

Chat UI nen uu tien su ro rang:

- Sidebar room co unread count.
- Message list doc tu tren xuong.
- Input gan cuoi man hinh.
- Online status nho, khong lam roi mat.
- Mobile chuyen giua room list va chat view.

## Backend contract can on dinh truoc

Truoc khi build frontend, nen co:

- Auth response format.
- Room summary format.
- Message history response format.
- WebSocket message event format.
- Upload response format.

Frontend se nhanh hon neu backend contract on dinh.

