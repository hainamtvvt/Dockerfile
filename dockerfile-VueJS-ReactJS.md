***VueJS và ReactJS: Là 2 ngôn ngữ lập trình được sử dụng để phát triển phần FE***

### Dockerfile Project VueJS

```
# build stage
FROM node:16-alpine as build-stage
WORKDIR /app
COPY . .
RUN npm install
RUN npm run build
## các bạn có thể dùng yarn install .... tuỳ nhu cầu nhé

# production stage
FROM nginx:1.17-alpine as production-stage
COPY --from=build-stage /app/dist /usr/share/nginx/html
CMD ["nginx", "-g", "daemon off;"]
```

***Đối với project Vue hoặc React là dạng full frontend - không có tý backend nào, chúng chỉ được chạy trên trình duyệt. Mà khi ra tới trình duyệt, thì thứ trình duyệt có thể hiểu được là HTLM, CSS và JS***

Do đó để dockerfile ứng dụng Vue/React thì việc của ta chỉ cần lấy được các file build cuối cùng cần thiết để chạy trình duyệt còn các thứ khác có hay không không quan trọng, như vậy image sẽ giảm được size, giảm thiểu tối đa các thứ không cần thiết trong image

Giải thích dockerfile trên:

  - Ở dockerfile trên được chia làm hai giai đoạn khi build image: build stage và production stage

  - Ở build stage bắt đầu với image base là node:16-alpine

  - Trong build stage bắt đầu từ wordir là /app, sau đó thực hiện copy toàn bộ file ở folder hiện tại ở môi trường bên ngoài vào bên trong image ở folder /app

  - Tiếp theo thực hiện chạy npm install để cài đặt các depencies và cuối cùng thực hiện build project

  - Ở production stage bắt đầu với image nginx:1.17-alpine làm 

  - Thực hiện COPY từ build-stage lấy folder ở đường dẫn /app/dist (kết quả cuối cùng ở stage build) vào đường dẫn /usr/share/nginx/html trong image

  - Cuối cùng dùng CMD để khởi động nginx


### Dockerfile Project ReactJS

Tương tự như VueJS -> điều khác biệt duy nhất  React khi build sinh ra project **build** chứ không phải **dist** như 







