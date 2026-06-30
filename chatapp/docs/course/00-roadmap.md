# Roadmap khoa hoc Real-time Chat

## Muc tieu

Ban se xay dung mot ung dung chat theo huong backend-first. Moi chuong nen duoc hoc theo vong lap:

1. Doc de hieu y tuong.
2. Tu go code theo bai thuc hanh.
3. Chay app hoac test nho.
4. Ghi lai loi gap phai.
5. Refactor khi da hieu.

Tieu chi cua khoa nay khong phai la tao code dep ngay tu dau. Tieu chi la hieu vi sao chat backend can REST API, WebSocket, JWT, database va Redis.

## Thu tu hoc

| Chuong | Noi dung | San pham sau chuong |
| --- | --- | --- |
| 01 | Setup project | Backend chay duoc voi profile dev |
| 02 | Domain va database | Co entity va migration dau tien |
| 03 | Authentication JWT | Register, login, token |
| 04 | REST API phong chat | Tao phong, them member, query phong |
| 05 | WebSocket STOMP co ban | Client connect va subscribe duoc |
| 06 | Real-time messaging | Gui tin va broadcast den room |
| 07 | Online presence voi Redis | Theo doi online/offline |
| 08 | Lich su tin nhan | Pagination va sync khi reconnect |
| 09 | Chia se file | Upload va gan attachment vao message |
| 10 | Offline notification | Luu notification event co ban |
| 11 | Testing va debugging | Test vung quan trong |
| 12 | Deployment va scaling | Docker Compose va scale mindset |
| 13 | Frontend plan | De danh cho Next.js sau |

## Pham vi backend

Backend se gom cac module chinh:

- `auth`: dang ky, dang nhap, JWT.
- `user`: thong tin nguoi dung.
- `room`: phong chat 1-1 va nhom.
- `message`: tin nhan, lich su, pagination.
- `ws`: cau hinh WebSocket va STOMP handler.
- `presence`: online/offline session bang Redis.
- `file`: upload file va attachment metadata.

## Cach lam viec

Moi khi den mot chuong, ban nen tao branch hoac commit nho. Neu dang hoc mot minh, commit message co the don gian:

```bash
git add .
git commit -m "learn: chapter 02 domain model"
```

Neu mot chuong bi kho, dung lai o muc tieu nho nhat. Vi du WebSocket chua can luu database ngay; chi can connect, subscribe, send echo thanh cong truoc.

## Khong copy paste y nguyen

Trong cac chuong, code chi la khung goi y. Ban nen tu dat ten class, tu viet lai theo package trong project cua minh. Neu copy mot doan, hay sua no thanh version ban hieu duoc.

