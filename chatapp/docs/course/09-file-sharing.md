# Chuong 09 - Chia se file

## Muc tieu

Upload file qua REST API, luu metadata vao MySQL, va gui message type `FILE` qua WebSocket.

## Can hieu

File sharing co 2 phan:

- File binary nam o storage: local disk, S3, MinIO.
- Metadata nam o database: ten file, size, content type, URL, uploader.

Trong giai do hoc, dung local disk la du. Sau nay co the doi sang S3/MinIO.

## Bai thuc hanh

### 1. Endpoint upload

```text
POST /api/files
Content-Type: multipart/form-data
```

Response:

```java
public record FileUploadResponse(
        Long fileId,
        String originalName,
        String contentType,
        long size,
        String url
) {}
```

### 2. Validate file

Rule toi thieu:

- Gioi han size, vi du 10 MB.
- Khong cho filename rong.
- Chi cho mot so content type trong giai do dau: image, pdf, text.

### 3. Gan file vao message

Client flow:

1. Upload file bang REST.
2. Nhan `fileId`.
3. Send WebSocket message type `FILE` kem `fileId`.
4. Server tao message va attachment record.
5. Server broadcast event co attachment metadata.

Khong gui binary file qua WebSocket trong giai do nay.

### 4. Local storage

Luu file vao folder cau hinh:

```properties
chatapp.storage.local-root=./uploads
```

Ten file tren disk nen la UUID, khong dung truc tiep original filename.

## Tu kiem tra

- Upload anh nho thanh cong.
- Upload file qua lon bi reject.
- Gui message type `FILE` thi history tra ve attachment.

## Loi hay gap

- Dung original filename lam path, de loi path traversal.
- Khong validate content type va size.
- Xoa message nhung khong co chien luoc file cleanup. Giai do hoc co the ghi vao backlog.

