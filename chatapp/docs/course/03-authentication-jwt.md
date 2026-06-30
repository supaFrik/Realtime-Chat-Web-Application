# Chuong 03 - Authentication voi JWT

## Muc tieu

Co API register/login va tao JWT de dung cho REST API lan WebSocket.

## Can hieu

JWT la ve vao cua client. REST API gui token trong header `Authorization: Bearer ...`. WebSocket cung can token luc connect, nhung vi handshake khac HTTP request thong thuong nen ta se xu ly rieng o chuong 05.

## Bai thuc hanh

### 1. Them dependency JWT

Chon mot thu vien JWT pho bien. Vi du `jjwt`. Muc tieu cua chuong khong phai hoc het JWT spec, ma la tao va validate token co subject la user id hoac username.

### 2. Tao DTO

Can toi thieu:

```java
public record RegisterRequest(
        String username,
        String email,
        String password,
        String displayName
) {}

public record LoginRequest(
        String usernameOrEmail,
        String password
) {}

public record AuthResponse(
        String accessToken,
        String tokenType
) {}
```

Hay them validation annotation khi ban da tao endpoint chay duoc.

### 3. Tao AuthService

Logic:

1. Register: kiem tra username/email chua ton tai.
2. Hash password bang `PasswordEncoder`.
3. Luu user.
4. Login: tim user, so khop password.
5. Tao access token.

Khong tra ve `passwordHash` trong response.

### 4. Cau hinh SecurityFilterChain

Cho phep:

- `POST /api/auth/register`
- `POST /api/auth/login`
- endpoint health neu co

Con lai yeu cau authentication.

## Tu kiem tra

Dung curl hoac Postman:

```bash
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username":"alice","email":"alice@example.com","password":"secret123","displayName":"Alice"}'
```

Sau login, goi mot endpoint protected voi token.

## Loi hay gap

- Luu plain password.
- Token het han qua dai trong moi truong dev roi quen mat.
- Dua role/permission vao token nhung khong co chien luoc refresh. Ban co the lam access token truoc, refresh token sau.

