# Chuong 05 - WebSocket va STOMP co ban

## Muc tieu

Client connect duoc vao WebSocket endpoint, subscribe topic va gui message test qua STOMP.

## Can hieu

WebSocket la ket noi hai chieu. STOMP la giao thuc message nam tren WebSocket, giup ta co khung:

- client `CONNECT`
- client `SUBSCRIBE`
- client `SEND`
- server `MESSAGE`

Trong Spring, endpoint WebSocket khac voi REST endpoint.

## Bai thuc hanh

### 1. Tao WebSocket config

Khung y tuong:

```java
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {
    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/ws")
                .setAllowedOriginPatterns("*")
                .withSockJS();
    }

    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {
        registry.enableSimpleBroker("/topic", "/queue");
        registry.setApplicationDestinationPrefixes("/app");
        registry.setUserDestinationPrefix("/user");
    }
}
```

Ban nen hieu y nghia:

- `/ws`: noi client ket noi.
- `/app`: noi client gui len server handler.
- `/topic`: broadcast cho nhieu client.
- `/user/queue`: tin rieng cho mot user.

### 2. Tao handler ping

```java
@Controller
public class RealtimeController {
    @MessageMapping("/ping")
    @SendTo("/topic/pong")
    public String ping(String body) {
        return "pong: " + body;
    }
}
```

Day la bai test ket noi, chua phai chat that.

### 3. WebSocket authentication

O muc dau, cho connect truoc de hieu flow. Sau do them interceptor de doc JWT tu STOMP header:

```text
CONNECT
Authorization: Bearer <token>
```

Server validate token va gan Principal cho session.

## Tu kiem tra

- Client connect den `/ws`.
- Subscribe `/topic/pong`.
- Send `/app/ping`.
- Nhan duoc message tu server.

## Loi hay gap

- Nhap nham `/app/ping` thanh `/topic/ping`.
- Quen subscribe truoc khi send.
- CORS WebSocket khac voi CORS REST.
- Auth REST da chay nhung WebSocket van anonymous vi chua xu ly STOMP CONNECT.

