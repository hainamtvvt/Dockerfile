### Dockerfile with NodeJS

1. Create Dockerfile

```
FROM node:latest

WORKDIR /app

COPY . .

RUN npm install

CMD ["npm", "start"]
```

  - Ở đầu môid file dockerfile luôn phải có FROM. FROM ý chỉ bắt đầu từ môi trường nào, môi trường này phải đã được build thành Image.

  - WORKDIR. Ý chỉ bên trong image này tạo 1 folder tên là app và chuyển đến folder app, tương đương với câu lệnh mkdir  /app && cd /app

  - COPY. Copy toàn bộ code ở đường dẫn đang run docker file vào trong folder /app

  - RUN. RUN để chạy một câu lệnh nào đó khi build Docker Image, ở đây cần chạy lệnh **npm install** để thực hiện cài các dependencies. Chú ý lệnh RUN chỉ thực hiện khi build image

  - CMD. Chỉ câu lệnh mặc định khi mà 1 container được khởi chạy từ image này. CMD sẽ luôn được chạy khi 1 container được khởi tạo từ image 


2. Build image

Run command

```
docker build -t learning-docker/node:v1 .
```

  - Options -t để chỉ định tên của image sau khi thực hiện build

  - Dấu . để chỉ định đến file Dockerfile cùng với folder hiện tại

3. Q&A

Trường hợp chỉ muốn COPY một hoặc 1 số file khi build image

```
COPY app.js .  # Copy app.js ở folder hiện tại vào đường dẫn ta set ở WORKDIR trong Image
COPY app.js /abc/app.js   ## Kết quả tương tự, ở đây ta nói rõ ràng hơn (nếu ta muốn copy tới một chỗ nào khác không phải WORKDIR)
    
# Copy nhiều file
COPY app.js package.json package-lock.json .
# Ở trên ta các bạn có thể copy bao nhiêu file cũng được, phần tử cuối cùng (dấu "chấm") là đích ta muốn copy tới trong Image

```

Sử dụng ADD để copy file????

COPY và ADD trong Docker được sử dụng chung 1 mục đích là copy file từ 1 nơi nào đó vào bên trong image

Khác nhau giữa COPY  và ADD


  - COPY: Nhận vào  đối tượng cần COPY và đích cần COPY tới trong image. COPY chỉ cho phép thực hiện copy **file**  từ local vào trong Image


  - ADD: AĐ cũng làm được tương tự như COPY tuy nhiên có thêm 2 chức năng;


      - Có thể copy từ một địa chỉ URL vào trong image

      - Giải nén một file và copy vào trong image

Hầu hết các trường hợp khi cần đến URL mà muốn download file thì sẽ dùng lệnh RUN curl/wget ...và muốn giải nén file thì sử dụng lệnh RUN tar -xvzf

Do đó lời khuyên nếu luôn dùng COPY để làm rõ thực hiện hành động copy file từ local vào trong image

Nếu cuối file Dockerfile không có CMD có được không???

Mỗi file Dockerfile chỉ cho phép 1 CMD, vậy nếu muốn có nhiều CMD???


Trường hợp CMD phức tạp thì có thể dùng ENTRYPOINT

ENTRYPOINT có thể hiểu theo nghĩa đen là "điểm bắt đầu"  - có thể chạy khá giống như CMD nhưng cũng có thể đùng để cấu hình cho CMD trước khi chạy 1 CMD. Một trường hợp thường dùng với ENTRYPOINT là dùng 1 file shell script.sh để cấu hình mọi thứ cần thiết khi khởi chạy container bằng CMD

```
....
ENTRYPOINT ["sh", "/var/www/html/.docker/docker-entrypoint.sh"]

CMD supervisord -n -c /etc/supervisord.conf

```
