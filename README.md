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
      MYSQL_DATABASE: camdo_db
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
###### Tạo file Migration:
chạy: docker compose exec web python manage.py makemigrations quanly
<img width="826" height="161" alt="image" src="https://github.com/user-attachments/assets/98ad997d-a3ef-4807-bcca-014243c56368" />

###### tạo bảng dữ liệu trong MariaDB:
chạy: docker compose exec web python manage.py migrate
<img width="707" height="194" alt="image" src="https://github.com/user-attachments/assets/155d71fc-23df-43ff-ace8-33ff3a7cd58a" />

###### Tạo tài khoản quản trị (Superuser):
chạy: docker compose exec web python manage.py createsuperuser
<img width="498" height="185" alt="image" src="https://github.com/user-attachments/assets/1be28211-2959-401d-893b-592f6e937362" />

##### Database:
<img width="224" height="85" alt="image" src="https://github.com/user-attachments/assets/9185a1f2-2627-4034-8fae-cf3c8bb9aa59" />

#### Nhập dữ liệu từ trang admin:
###### đường dẫn: http://172.30.171.9:8000/admin
(ip của ubuntu :8000.....)
<img width="870" height="489" alt="image" src="https://github.com/user-attachments/assets/e56f8c40-123e-4e16-b082-9027a9bd3137" />
###### sử dụng tài khoản admin đã tạo ở trên
##### nhập dữ liệu:
<img width="672" height="444" alt="image" src="https://github.com/user-attachments/assets/df55cdf3-b206-4993-94d2-665c726ed0cb" />
<img width="787" height="450" alt="image" src="https://github.com/user-attachments/assets/6b01b112-6c74-4890-803d-6f586b3df4ea" />
<img width="558" height="517" alt="image" src="https://github.com/user-attachments/assets/cf989637-dd67-4422-9c11-c116b094a7e0" />
<img width="445" height="497" alt="image" src="https://github.com/user-attachments/assets/694b1f0d-60c5-4d2a-9a79-9c64a429ec71" />
###### dữ liệu đã có trong mariadb:
<img width="699" height="244" alt="image" src="https://github.com/user-attachments/assets/c1f41bcc-e898-4974-a330-7d0994701858" />

###### tạo file html giao diện cho trang chủ

#### Trang chủ
###### trang chủ đã hiển thị
<img width="1158" height="257" alt="image" src="https://github.com/user-attachments/assets/fe26f9a2-7ee9-4bc2-99ed-5fcaf5f3aa1a" />

### cloudflare tunnel để public kết quả lên 1 sub-domain:
#### sub-domain: https://camdo.tnut58ktp047.id.vn/
###### tải cloudflared, login cloudflare: cloudflared tunnel login
tạo tunnel: cloudflared tunnel create camdo-tunnel
Trỏ Sub-domain về Tunnel: cloudflared tunnel route dns camdo-tunnel camdo.tnut58ktp047.id.vn
<img width="1126" height="37" alt="image" src="https://github.com/user-attachments/assets/e4285f65-7ac8-4289-8811-ebdf8ff24c96" />
###### cloudflare/DNS:
<img width="715" height="57" alt="image" src="https://github.com/user-attachments/assets/c13c3365-936c-4922-8b5b-74cc28c8d23e" />

##### kích hoạt tunnel:
###### kiểm tra nhanh: cloudflared tunnel run --url http://127.0.0.1:8000 camdo-tunnel
<img width="1148" height="250" alt="image" src="https://github.com/user-attachments/assets/d1f9b613-ee09-40f4-b834-0ae57e39f125" />
###### kiểm tra với điện thoại sử dụng mạng khác:
<img width="590" height="1280" alt="f616ed5bbe4d3f13665c" src="https://github.com/user-attachments/assets/a795fbfe-f83a-46fe-a790-4e1a2d60d654" />
###### tắt terminal ubuntu là dừng

##### chạy ngầm: nohup cloudflared tunnel run --url http://127.0.0.1:8000 camdo-tunnel > tunnel.log 2>&1 &
##### tắt terminal ubuntu, 
##### kiểm tra: máy tính và điện thoại đều có thể truy cập
<img width="955" height="563" alt="image" src="https://github.com/user-attachments/assets/c946b741-19c6-41ca-9e9b-1454754e6aac" />
