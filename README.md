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
    ├── requirements.txt
    ├── Dockerfile
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
##### File django_app/Dockerfile
<img width="571" height="54" alt="image" src="https://github.com/user-attachments/assets/39ede1bd-472f-4c5e-bb1c-3c0d1b3eacf3" />
<img width="738" height="407" alt="image" src="https://github.com/user-attachments/assets/1bf7cb75-c0d7-4d4f-8163-bb7454ac7487" />

##### chạy: docker compose run --rm web django-admin startproject camdo_pro .
##### docker compose up -d
<img width="753" height="166" alt="image" src="https://github.com/user-attachments/assets/a3007aa6-be19-4926-a29d-0b15a14b87f2" />
<img width="1107" height="430" alt="image" src="https://github.com/user-attachments/assets/9d2c1842-a806-4f44-a647-02523ed38a89" />

##### cấu hình database:
###### file django_app/camdo_pro/settings.py:
sửa mục DATABASES
<img width="648" height="25" alt="image" src="https://github.com/user-attachments/assets/03e9cfd1-1ca6-462f-bb47-3fd35a865ac2" />
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'camdo_db',
        'USER': 'user_camdo',
        'PASSWORD': '123456',
        'HOST': 'db',
        'PORT': '3306',
    }
}
ALLOWED_HOSTS = ['*'] # Cho phép Cloudflare truy cập
```
<img width="515" height="247" alt="image" src="https://github.com/user-attachments/assets/753e675c-ce67-436b-9dd3-653674bab37c" />

##### Định nghĩa các bảng (Models):
###### tạo app quanly (quản lý)
lệnh: docker compose exec web python manage.py startapp quanly
sửa file django_app/quanly/models.py:
<img width="605" height="38" alt="image" src="https://github.com/user-attachments/assets/e669d038-2720-488f-8a17-5c2d1e6924ab" />
<img width="769" height="426" alt="image" src="https://github.com/user-attachments/assets/10e9af12-cf5c-41ee-806f-0fa0b5bc323a" />

##### Sửa file django_app/quanly/admin.py để có giao diện thêm/xóa/sửa:
<img width="406" height="152" alt="image" src="https://github.com/user-attachments/assets/c37dd50a-d00a-4151-bee1-8ccdf6b0648f" />

##### Chạy hệ thống và Tạo dữ liệu
