# Môn: Phát triển ứng dụng với mã nguồn mở-TEE0421
### Họ tên: Dương Quang Minh
### MSSV: K225480106047
### Lớp: 58KTPM

### Bài 2: SỬ DỤNG DJANGO ĐỂ TẠO WEB QUẢN LÝ TIỆM CẦM ĐỒ
#### 1. TỔ CHỨC CSDL CHO HỆ THỐNG QUẢN LÝ TIỆM CẦM ĐỒ:
###### csdl:
<img width="1280" height="960" alt="ba489db513a292fccbb3" src="https://github.com/user-attachments/assets/638ea690-d695-4a79-9033-266bfb91d492" />

#### 2. 
###### thư mục manguonmo_bt2
###### cấu trúc
```
pawnshop/
├── docker-compose.yml
├── django_app/
```
##### file docker-compose.yml:
```
version: '3.8'

services:
  db:
    image: mariadb:10.11
    restart: always
    environment:
      MYSQL_DATABASE: camdo_db_opensource_bt2_k3y4
      MYSQL_USER: user_camdo
      MYSQL_PASSWORD: 123456
      MYSQL_ROOT_PASSWORD: 123456
    volumes:
      - db_data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin
    restart: always
    ports:
      - "8080:80"
    environment:
      PMA_HOST: db
    depends_on:
      - db

  web:
    build: ./django_app
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - ./django_app:/app
    ports:
      - "8000:8000"
    depends_on:
      - db

volumes:
  db_data:
```
##### File django_app/requirements.txt
```
django==4.2
mysqlclient==2.2.0
```
##### 
