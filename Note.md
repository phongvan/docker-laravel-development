Dockerfile
Docker-compose
docker-compose.yml


xem cac imae

images => same file cai dat, kieu file - ghost
container => cai dat len tu file images 


Comand: 
	docker-compose ps
	docker-compose ps
	docker images
	docker-compose up -d // -d chay ngam,
	
	#1 CAI DAT
	***Cai Dat Docker 
	sudo atp-get update -y
	sudo atp install -y docker  // cai dat docker 
	sudo service docker start  // start no len 
	sudo usermod -a -G docker ec2-user  // k ro
	
	FROM DOC DOCKER :  https://docs.docker.com/engine/install/ubuntu/
	# Add Docker's official GPG key:
	sudo apt-get update
	sudo apt-get install ca-certificates curl gnupg
	sudo install -m 0755 -d /etc/apt/keyrings
	curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
	sudo chmod a+r /etc/apt/keyrings/docker.gpg

	# Add the repository to Apt sources:
	echo \
	  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
	  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
	  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
	sudo apt-get update

	
	sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
	
	

	***Cai Dat Docker Compose

	sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose  //cai dat docker-compose:  

	sudo chmod +x /usr/local/bin/docker-compose


Lab document
Các lệnh với image
- Tìm image để cài đặt :  docker search [centos]
- List các image đang có: docker image ls
- Xóa image: sudo docker rmi [Image ID]
Chú ý: Các image mà có container đang chạy thì sẽ không xóa được mà phải xóa container liên quan tới nó trước
- Pull image về:  docker pull centos:7

Làm việc với container

- Chạy 1 container từ image
	docker run --privileged -d -p 80:80 [Image Name] /sbin/init
	docker run --privileged -d -p 80:80 centos:7 /sbin/init

- Vào container để chạy lệnh:  docker exec -it [Container ID / Or name ] /bin/bash 

- Cài đặt apache
	yum -y install httpd
	systemctl start httpd
	systemctl enable httpd

echo "Hello Tin Hoc That La Don Gian" } /var/www/html/index.html

- Thoát ra khỏi container: exit

- Tạo image để triển khai cho máy khác
	docker commit -m "Comment" -a "Tác giả"  [Container ID] [Image Name]
	docker commit -m "Centos Project01" -a "Nguyen Quoc Bao" d452f1a1b69d tinhocthatladongian/project01:v1

- Đăng nhập vào docker/hub: docker login

- Đưa image lên docker hup để mọi người cùng sử dụng
	docker push [Tên image]
	docker push tinhocthatladongian/project01:v1
 
- Check các container đang chạy: sudo docker ps -a

- Xem trạng thái container: docker container ls -a

- Xóa containner: sudo docker rm [Container ID]

- Stop container: docker container stop [Container ID]

- Restart container: docker container restart [Container ID]

- Pause container: docker container pause  [Container ID]

- Truy cập vào các container đang chạy: docker container attach [Container ID]


- Lệnh stop toàn bộ container: docker stop $(docker ps -a -q)

***- Lệnh xóa toàn bộ container: docker rm $(docker ps -a -q)

***- Lệnh xóa toàn bộ image: docker rmi -f $(docker images -a -q)

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

	
	file docker-compose.yml: Noi dung nhu sau...

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
	  






