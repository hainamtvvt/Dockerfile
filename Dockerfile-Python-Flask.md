## Dockerfile with Project Python & Flask

```
FROM python:3.6-alpine

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

CMD ["python", "app.py"]
```

Giải thích:

  - Dòng đầu tiên FROM thực hiện quá trình build image với 1 image base đã được cài đặt sẵn Python phiên bản 3.6

  - Tiếp theo trong Dockerfile khởi tạo thư mục làm việc và chuyển đến thư mục làm việc /app

  - Tiếp theo COPY toàn bộ file từ folder ở môi trường gốc (bên ngoài) vào bên trong đường dẫn /app bên trong image

  - Tiếp theo thực hiện cài đặt các dependencies - cần cài đặt các packet nào thì thực hiện define ở file requirement.

  - Cuối cùng sử dụng lệnh CMD để chỉ định command line mặc định khi 1 container được khởi tạo từ image này: Khởi động file app.py

