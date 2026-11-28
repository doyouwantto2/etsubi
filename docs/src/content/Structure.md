## Quy tắc khi gọi các service

![](../assets/backend-calling-deps.drawio.png)

Các service sẽ gọi repository do nó liên kết với database, các service không gọi chéo nhau để dễ bảo trì. Bù lại thì mọi logic bị gom vào handler và sẽ khó xử lý một số trường hợp phức tạp.

## Những dịch vụ được sử dụng 

- Email: Resend

- Oauth2: Google

- Translation: Gemini / Deepseek
