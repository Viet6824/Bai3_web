# BÀI TẬP 3 - PHÁT TRIỂN ỨNG DỤNG TRÊN NỀN WEB  
**Giảng viên:** Đỗ Duy Cốp  
**Lớp học phần:** 58KTP  
**Sinh viên thực hiện:** Nguyễn Đức Việt
## Đề bài 
Yêu cầu     : LẬP TRÌNH ỨNG DỤNG WEB trên nền linux
1. Cài đặt môi trường linux: SV chọn 1 trong các phương án
 - enable wsl: cài đặt docker desktop
 - enable wsl: cài đặt ubuntu
 - sử dụng Hyper-V: cài đặt ubuntu
 - sử dụng VMware : cài đặt ubuntu
 - sử dụng Virtual Box: cài đặt ubuntu
2. Cài đặt Docker (nếu dùng docker desktop trên windows thì nó có ngay)
3. Sử dụng 1 file docker-compose.yml để cài đặt các docker container sau: 
   mariadb (3306), phpmyadmin (8080), nodered/node-red (1880), influxdb (8086), grafana/grafana (3000), nginx (80,443)
4. Lập trình web frontend+backend:
 SV chọn 1 trong các web sau:
 4.1 Web thương mại điện tử
 - Tạo web dạng Single Page Application (SPA), chỉ gồm 1 file index.html, toàn bộ giao diện do javascript sinh động.
 - Có tính năng login, lưu phiên đăng nhập vào cookie và session
   Thông tin login lưu trong cơ sở dữ liệu của mariadb, được dev quản trị bằng phpmyadmin, yêu cầu sử dụng mã hoá khi gửi login.
   Chỉ cần login 1 lần, bao giờ logout thì mới phải login lại.
 - Có tính năng liệt kê các sản phẩm bán chạy ra trang chủ
 - Có tính năng liệt kê các nhóm sản phẩm
 - Có tính năng liệt kê sản phẩm theo nhóm
 - Có tính năng tìm kiếm sản phẩm
 - Có tính năng chọn sản phẩm (đưa sản phẩm vào giỏ hàng, thay đổi số lượng sản phẩm trong giỏ, cập nhật tổng tiền)
 - Có tính năng đặt hàng, nhập thông tin giao hàng => được 1 đơn hàng.
 - Có tính năng dành cho admin: Thống kê xem có bao nhiêu đơn hàng, call để xác nhận và cập nhật thông tin đơn hàng. chuyển cho bộ phận đóng gói, gửi bưu điện, cập nhật mã COD, tình trạng giao hàng, huỷ hàng,...
 - Có tính năng dành cho admin: biểu đồ thống kê số lượng mặt hàng bán được trong từng ngày. (sử dụng grafana)
 - backend: sử dụng nodered xử lý request gửi lên từ javascript, phản hồi về json.
 4.2 Web IOT: Giám sát dữ liệu IOT.
 - Tạo web dạng Single Page Application (SPA), chỉ gồm 1 file index.html, toàn bộ giao diện do javascript sinh động.
 - Có tính năng login, lưu phiên đăng nhập vào cookie và session
   Thông tin login lưu trong cơ sở dữ liệu của mariadb, được dev quản trị bằng phpmyadmin, yêu cầu sử dụng mã hoá khi gửi login.
   Chỉ cần login 1 lần, bao giờ logout thì mới phải login lại.
 - hiển thị giá trị mới nhất của các thông số đang giám sát, khi click vào thì hiển thị đồ thị lịch sử quá trình thay đổi (gọi grafana iframe để hiển thị)
 - backend: Sử dụng nodered để đọc dữ liệu từ các cảm biến (có thể dùng api online để lấy dữ liệu theo giời gian thực), 
   nodered sẽ lưu dữ liệu mới nhất (dạng update) vào cơ sở dữ liệu mariadb (sử dụng phpmyadmin để tạp table và quản trị lần đầu)
   nodered sẽ lưu dữ liệu (insert) vào influxdb để lưu giá trị lịch sử, để cho grafana dùng để hiển thị biểu đồ.
5. Nginx làm web-server
 - Cấu hình nginx để chạy được website qua url http://fullname.com  (thay fullname bằng chuỗi ko dấu viết liền tên của bạn)
 - Cấu hình nginx để http://fullname.com/nodered truy cập vào nodered qua cổng 80, (dù nodered đang chạy ở port 1880)
 - Cấu hình nginx để http://fullname.com/grafana truy cập vào grafana qua cổng 80, (dù grafana đang chạy ở port 3000)

Yêu cầu sinh viên lưu code trên github
có file readme.md có hình ảnh + text: ghi lại nhật ký quá trình làm bài.

CÁCH ĐÁNH GIÁ:
1. Cài đặt được môi trường: 1đ
2. Cài đặt được các docker container với cấu hình phù hợp: 1đ
3. Web chạy được, giao diện phù hợp, chạy trên web sever nginx: 2đ
4. nodered api trả về json, test được: 2đ
5. front-end có js gọi được api nodered, nhận về json, hiển thị được kết quả từ json này. 2đ
6. Bài làm có dấu ấn, giải thích rõ ràng, hiểu vấn đề: 2đ
---
## Bài làm
### 1. Cài đặt môi trường
Bước 1: Kích hoạt WSL và cài đặt Ubuntu
1. Mở powershell  (Run as Administrator)

<img width="1920" height="1080" alt="Ảnh chụp màn hình (553)" src="https://github.com/user-attachments/assets/33b64a34-10d4-4f6f-8e75-c7304095eb97" />

2. Sau khi cài xong khởi động lại máy
3. Mở ứng dụng Ubuntu, thiết lập username và password:

<img width="1920" height="1080" alt="Ảnh chụp màn hình (560)" src="https://github.com/user-attachments/assets/b5655527-e71b-4e59-919a-f73ee1d84ebe" />

4. Cập nhật hệ thống : sudo apt update && sudo apt upgrade -y
5. Cài thêm tool cơ bản: sudo apt install curl wget -y
6. Kiểm tra Ubuntu: lsb_release -a

<img width="1920" height="1080" alt="Ảnh chụp màn hình (561)" src="https://github.com/user-attachments/assets/3c84528c-922f-4756-a81b-26efb86e677f" />

Bước 2: Cài đặt Docker & Docker Compose
1. Trong Ubuntu, chạy lệnh:
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
2. Thêm user vào group để chạy docker không cần sudo :sudo usermod -aG docker $USER
3. Áp dụng thay đổi: sudo reboot
4. Test thử: docker run hello-world

<img width="1920" height="1080" alt="Ảnh chụp màn hình (577)" src="https://github.com/user-attachments/assets/cf486e60-e03c-419e-895e-e023e10b8f90" />

5. Thêm repo docker: sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

<img width="1915" height="224" alt="Ảnh chụp màn hình 2025-11-05 232028" src="https://github.com/user-attachments/assets/d3493bc1-2a19-44a7-9ae3-4f72085dc7fc" />

6. Thay đổi quyền thực thi cho file: sudo chmod +x /usr/local/bin/docker-compose
7. Kiểm tra Docker: docker compose version
Bước 3: Cấu hình Docker Compose
1. Tạo thư mục và chuyển đến nó
mkdir webapplinux
cd webapplinux
2. Tạo file nano docker-compose.yml:
```
version: '3.8'

services:
  mariadb:
    image: mariadb:10.11
    container_name: mariadb
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root123
      MYSQL_DATABASE: shopviet
      MYSQL_USER: viet
      MYSQL_PASSWORD: 123456
    ports:
      - "3306:3306"
    volumes:
      - ./mariadb/data:/var/lib/mysql
    networks:
      - ecommerce-network

  phpmyadmin:
    image: phpmyadmin:latest
    container_name: phpmyadmin
    restart: always
    environment:
      PMA_HOST: mariadb
      PMA_PORT: 3306
      MYSQL_ROOT_PASSWORD: root123
    ports:
      - "8080:80"
    depends_on:
      - mariadb
    networks:
      - ecommerce-network

  nodered:
    image: nodered/node-red:latest
    container_name: nodered
    restart: always
    environment:
      - TZ=Asia/Ho_Chi_Minh
    ports:
      - "1880:1880"
    volumes:
      - ./node-red/data:/data
    user: "1000:1000"
    depends_on:
      - mariadb
      - influxdb
    networks:
      - ecommerce-network
    command: >
      sh -c "
      npm install -g node-red-node-mysql &&
      node-red
      --httpNodeRoot=/api
      --httpAdminRoot=/nodered
      --functionGlobalContext.mysql=require('mysql').createPool({host:'mariadb',user:'viet',password:'123456',database:'shopviet',port:3306,charset:'utf8mb4',connectionLimit:10})
      --functionGlobalContext.crypto=require('crypto')
      "

  influxdb:
    image: influxdb:2.7
    container_name: influxdb
    restart: always
    environment:
      - DOCKER_INFLUXDB_INIT_MODE=setup
      - DOCKER_INFLUXDB_INIT_USERNAME=admin
      - DOCKER_INFLUXDB_INIT_PASSWORD=admin123
      - DOCKER_INFLUXDB_INIT_ORG=ecommerce
      - DOCKER_INFLUXDB_INIT_BUCKET=statistics
      - DOCKER_INFLUXDB_INIT_ADMIN_TOKEN=my-super-secret-auth-token
    ports:
      - "8086:8086"
    volumes:
      - ./influxdb/data:/var/lib/influxdb2
    networks:
      - ecommerce-network

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    restart: always
    environment:
      - GF_SERVER_HTTP_PORT=3000
      - GF_SERVER_SERVE_FROM_SUB_PATH=true
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=admin123
    ports:
      - "3000:3000"
    volumes:
      - ./grafana/data:/var/lib/grafana
    depends_on:
      - influxdb
    networks:
      - ecommerce-network

  nginx:
    image: nginx:latest
    container_name: nginx
    restart: always
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/conf.d/default.conf:/etc/nginx/conf.d/default.conf:ro
      - ./nginx/certs:/etc/nginx/certs:ro
      - ./web:/usr/share/nginx/html:ro
    depends_on:
      - nodered
      - grafana
    networks:
      - ecommerce-network

networks:
  ecommerce-network:
    driver: bridge
```
Sau khi mọi thứ đã ổn, sẽ thấy các container chạy:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5e864219-b2e6-478c-8ac0-b3f3fed06ab9" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/004ece1f-5311-4d8f-a02c-d1f80a50187d" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cee40677-0999-4ae8-8ccf-d0be015db90e" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8271f571-4d89-4fb6-8b75-5bf1956934a2" />

### 3. CẤU HÌNH NGINX
1. Chạy lệnh: nano ~/vietlinux/nginx/conf.d/default.conf trong Ubuntu  
2. Nội dung file default.conf:  
```
server {
    listen 80;
    server_name nguyenducviet.com www.nguyenducviet.com;

    location / {
        root /usr/share/nginx/html;
        index index.html;
        try_files $uri $uri/ /index.html;

        # Cache static assets
        location ~* \.(js|css|png|jpg|jpeg|gif|svg|ico|woff2?|ttf|eot)$ {
            expires 1y;
            add_header Cache-Control "public, immutable";
        }
    }

    # === API Backend (Node-RED) ===
    location /api/ {
        proxy_pass http://nodered:1880/;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # === API User Orders (Node-RED) ===
    location /user/ {
        proxy_pass http://nodered:1880/;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # === Node-RED UI (Subpath) ===
    location ^~ /nodered/ {
        proxy_pass http://nodered:1880/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # Fix tài nguyên tĩnh (CSS/JS) cho subpath
        sub_filter_once off;
        sub_filter 'href="/'  'href="/nodered/';
        sub_filter 'src="/'   'src="/nodered/';
        sub_filter 'action="/' 'action="/nodered/';
        sub_filter_types text/css text/javascript text/xml application/javascript;
        proxy_set_header Accept-Encoding "";
    }

    # === Grafana (Subpath) ===
    location /grafana/ {
        proxy_pass http://grafana:3000/;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto http;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        
        # Fix redirects từ Grafana
        proxy_redirect http://grafana:3000/ /grafana/;
        proxy_redirect / /grafana/;
        
        # thay the trong html
        sub_filter_once off;
        sub_filter_types text/html;
        sub_filter 'href="/' 'href="/grafana/';
        sub_filter 'src="/' 'src="/grafana/';
        sub_filter 'href="public/' 'href="/grafana/public/';
        sub_filter 'src="public/' 'src="/grafana/public/';
        
        proxy_set_header Accept-Encoding "";
    }
    # === 404 Fallback cho SPA ===
    error_page 404 /index.html;
}
```
Kết quả

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/82b12a26-4677-41f4-852a-f5a4f5035c14" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/adbb45b1-f088-47a8-8690-5dfb3b94998d" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c79231fe-bfe9-4e1f-b809-9d19cb1b7ad9" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ade780aa-205b-4df3-95ba-7859cc4e9436" />
