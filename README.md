
## Phần 1: Build và khơi chạy

Mở một terminal từ thư mục này, tạm gọi đây là **Terminal 1**
```bash
docker-compose build
docker-compose up -d

# Truy cập vào terminal của containter
docker exec -it laravel bash
# Tạo project laravel mới

composer create-project laravel/laravel .

```

Mở 1 terminal mới từ thư mục này, gọi là **Terminal 2**
```bash
sudo chown -R $USER laravel/source
```

Quay lại terminal 1, chạy lệnh sau để sửa quyền

```bash
chown -R $USER:www-data storage
chown -R $USER:www-data bootstrap/cache

chmod -R 775 storage
chmod -R 775 bootstrap/cache
```

Truy cập `http://localhost:7979` để thử

## Phần 2: Thay đổi thông tin cơ sở dữ liệu:

Vào file `.env` trong thư mục `laravel/source`, sửa tham số của phần database như sau:
```dotenv
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=<Tên cơ sở dữ liệu của bạn>
DB_USERNAME=root
DB_PASSWORD=password
```

Sửa file `init.sql` trong thư mục `mariadb-data` như sau:
```sql
create database <Tên cơ sở dữ liệu của bạn>;
```

```bash
docker-compose restart
```

## Phần 3: Thực liện các câu lênh: 

Muốn thao tác các lệnh đối với laravel như `php artisan migrate` hay `composer install`, `npm install` thì tiến hành thực hiện lệnh `docker exec` như sau:


```bash
docker exec -it laravel bash
```

*`Docker exec` là câu lệnh sử dụng để chạy một command bên trong một container đang hoạt động.*

Bây giờ các bạn có thể sửa code Laravel tùy theo ý bạn.

Các bạn có thể truy cập `http://localhost:8080` để truy cập phpAdmin với tài khoan là `root/password`

Các bạn muốn xóa project thì xóa cả thư mục source.

Nếu đã có dữ án rồi thì copy code laravel vào thư mục này là được.

#  Bổ sung
##  1 Cài đặt Docker

FROM DOC DOCKER :  https://docs.docker.com/engine/install/ubuntu/
// Add Docker's official GPG key:

    sudo apt-get update
    sudo apt-get install ca-certificates curl gnupg
    sudo install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    sudo chmod a+r /etc/apt/keyrings/docker.gpg

// Add the repository to Apt sources:

    echo \
	  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
	  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
	  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update

	
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
	
	
##  2 Cài đặt Docker Compose
```bash
    sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose  //cai dat docker-compose:  
	sudo chmod +x /usr/local/bin/docker-compose
```

Dockerfile
Docker-compose
docker-compose.yml


xem cac imae

images => same file cai dat, kieu file - ghost
container => cai dat len tu file images 

##  2 Các Comand thường dùng
```
	docker ps
	docker images
	docker-compose up -d // -d chay ngam, foreround /backround
```

Lab document
Các lệnh với image
- Tìm image để cài đặt :

```
docker search [centos]
```

- List các image đang có: 
```bash
docker image ls
```
- Xóa image 
```bash
sudo docker rmi [Image ID]
```
Chú ý: Các image mà có container đang chạy thì sẽ không xóa được mà phải xóa container liên quan tới nó trước
- Pull image về:  
```
docker pull centos:7
```

Làm việc với container

- Chạy 1 container từ image
```
	docker run --privileged -d -p 80:80 [Image Name] /sbin/init
	docker run --privileged -d -p 80:80 centos:7 /sbin/init
```
- Vào container để chạy lệnh:  
```
docker exec -it [Container ID / Or name ] /bin/bash 
```

- Cài đặt apache
```
yum -y install httpd
systemctl start httpd
systemctl enable httpd
```    

echo "Hello Tin Hoc That La Don Gian" } /var/www/html/index.html

- Thoát ra khỏi container: exit

- Tạo image để triển khai cho máy khác
```
docker commit -m "Comment" -a "Tác giả"  [Container ID] [Image Name]
docker commit -m "Centos Project01" -a "Nguyen Quoc Bao" d452f1a1b69d tinhocthatladongian/project01:v1
```
- Đăng nhập vào docker/hub: 
```docker login```
- Đưa image lên docker hup để mọi người cùng sử dụng
```
docker push [Tên image]
docker push tinhocthatladongian/project01:v1
 ```
- Check các container đang chạy: 
```sudo docker ps -a //Check các container đang chạy: 
```

- Xem trạng thái container: ```docker container ls -a```

- Xóa containner: 
```sudo docker rm [Container ID]```

- Stop container: 
```docker container stop [Container ID]```

- Restart container: ```docker container restart [Container ID]```

- Pause container: ```docker container pause  [Container ID]```

- Truy cập vào các container đang chạy: ```d```ocker container attach [Container ID]```

## =================================

- **Lệnh stop toàn bộ container**
```docker stop $(docker ps -a -q)```

- **Lệnh xóa toàn bộ container**
```docker rm $(docker ps -a -q)```

- **Lệnh xóa toàn bộ image**
```docker rmi -f $(docker images -a -q)```


## =================================
VD1: 
	1. sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose  //cai dat docker-compose:  
	2. sudo chmod +x /usr/local/bin/docker-compose // Phan quyen
	3. docker-compose --version  //Check version docker
	4. Viết docker compose và dockerfile
	
	Viet rieng Dockerfile cho PHP vi can install cacs ext cua php.
	Noi dung: Dockerfile
	
	FROM php:8.1-fpm-alpine
	RUN docker-php-ext-install pdo pdo_mysql mbstring zip exif pcntl
	RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

	
- file docker-compose.yml: Noi dung nhu sau...
```dockerFile
version: '3'

services:
  nginx:
    image: nginx:stable
    ports:
      - "8080:80"
    volumes:
      - ./laravel:/var/www/html
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
  php:
    build:
      context: ./laravel
      dockerfile: Dockerfile
    volumes:
      - ./laravel:/var/www/html
    ports:
      - "9000:9000"
  mysql:
    image: mysql:8.0.29-oracle
    ports:
      - "3307:3306"
    volumes:
      - ./mysql:/var/lib/mysql
    environment: 
      MYSQL_DATABASE: laravel
      MYSQL_USER: pocadi
      MYSQL_PASSWORD: pocadi123
      MYSQL_ROOT_PASSWORD: pocadi123
  phpmyadmin:
    image: phpmyadmin/phpmyadmin:latest
    ports:
      - "8081:80"
    environment:
      - PMA_HOSTS=mysql
      - PMA_PORT=3306
      - PMA_USER=pocadi123
      - PMA_PASSWORD=pocadi123
	  
	  5. docker-compose build // Build container theo file docker-compose
	  6. docker-compose up -d  // run container
```