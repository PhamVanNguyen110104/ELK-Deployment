# Triển khai giải pháp SIEM ELK Stack

# ELK Stack là gì?

ELK Stack (ELK) là giải pháp SIEM của Elastic. Nó bao gồm 3 thành phần chính: Elasticsearch, Logstash và Kibana, thường được sử dụng cùng với nhau để thu thập, xử lý, lưu trữ, tìm kiếm và trực quan hóa dữ liệu một cách hiệu quả.

Các thành phần chính của ELK bao gồm:

* Các beat: Làm trung gian để dữ liệu được đẩu vào Logstash hoặc Elasticsearch từ các nguồn tương ứng. Một số beat phổ biến là Filebeat, Winlogbeat,...

* Logstash: Thu thập, xử lý và chuyển tiếp dữ liệu từ các beat.

* Elasticsearch: Tìm kiếm và phân tích dữ liệu đã được chuẩn hóa từ Logstash.

* Kibana: Hiển thị các phân tích từ Elasticsearch một cách trực quan.

Hình dưới đây trực quan các thành phần chính của ELK.

![ELK](image/ELK.png)

# Triển khai

## Kiến trúc hệ thống

Hình dưới đây mô tả kiến trúc hệ thống sẽ triển khai. Cụ thể, Linux log được xử lý như sau: Linux log -> Filebeat -> Logstash -> Elasticearch. Còn Windows log được xử lý như sau: Windows log -> Winlogbeat, Elasticsearch.

![KienTruc](image/KienTruc.png)

## Sơ đồ hệ thống

Các thành phần của ELK sẽ được triển khai trên các máy ảo được tạo bởi Oracle VirtualBox. Hình dưới đây mô tả các máy ảo sẽ được sử dụng. Máy ELK triển khai Logstash, Elasticsearch và Kibana. Máy Ubuntu sinh Linux log và triển khai Filebeat. Máy Windows sinh Windows log và triển khai Winlogbeat. Các máy đều có 2 card mạng ảo, 1 card mạng kết nối Internet và 1 card mạng kết nối với nhau qua dải địa chỉ 192.168.56.x.

![CacMayAo](image/CacMayAo.png)

## Filebeat

Trước khi cài đặt các thành phần của hệ thống, trên máy ELK và máy Ubuntu cần chạy hai lệnh sau:

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list

Câu lệnh đầu tiên sẽ tải tệp có chứa khóa GPG (GNU Privacy Guard) - cặp khóa mật mã cho ký số và xác thực dữ liệu từ Elastic - với chế độ im lặng (quiet mode) nhưng ghi nội dung đã tải được ra màn hình, sau đó thêm khóa GPG này vào hệ thống APT (Advanced Package Tool) - hệ thống quản lý các gói phần mềm mặc định của Ubuntu. Sau khi thực thi câu lệnh đầu tiên, câu lệnh thứ hai sẽ thêm repository của Elastic vào hệ thống APT để tải về các thành phần của ELK bằng lệnh apt.

Tiếp theo, ta chạy câu lệnh tải Filebeat về máy Ubuntu:

sudo apt-get install filebeat

Toàn bộ các tệp cấu hình của Filebeat sau khi tải về sẽ nằm trong thư mục /etc/filebeat. Trong thư mục này, tệp cấu hình chính là filebeat.yml. Tệp này cấu hình các chức năng của Filebeat.

Phần đầu tiên cần được cấu hình trong tệp trên là đầu vào của Filebeat. Hình dưới đây thể hiện các mục trong phần cấu hình đầu vào. Cụ thể như sau:

* type: Định nghĩa loại dữ liệu đầu vào. Ở đây, dữ liệu đầu vào là các dòng log nên mặc định sẽ chọn "filestream".

* id: Gán định danh cho dữ liệu đầu vào trên. Mục này được để mặc định.

* paths: Định nghĩa các đường dẫn tới tệp log trên máy Ubuntu. Ở đây, các đường dẫn được định nghĩa là toàn bộ các tệp có đuôi .log trong thư mục /var/log, tệp syslog và toàn bộ các tệp audit log có đuôi .log trong thư mục /var/log/audit.

![Filebeat1](image/FilebeatConfig1.png)

Phần tiếp theo cần được cấu hình là Kibana endpoint để Kibana trên máy ELK nhận được thông tin từ Filebeat, được thể hiện ở hình dưới đây. Phần này chỉ cấu hình host của Kibana theo dạng: <Địa chỉ IP của máy ELK>:5601. Ở đây, địa chỉ IP của máy ELK là 192.168.56.110.

![Filebeat2](image/FilebeatConfig2.png)

Phần cuối cùng cần được cấu hình là đầu ra của Filebeat. Do đầu ra là Logstash trên máy ELK, nên phần cấu hình là output.logstash như hình dưới đây. Phần này chỉ cấu hình host của Logstash theo dạng: <Địa chỉ IP của máy ELK>:5044. Ở đây, địa chỉ IP của máy ELK là 192.168.56.110.

![Filebeat3](image/FilebeatConfig3.png)

## Winlogbeat

Winlogbeat được tải về từ trang chủ của Elastic (https://www.elastic.co/downloads/past-releases/winlogbeat-7-17-29) và được về thư mục C:\Users\MayWindows\Desktop\winlogbeat. Tệp cấu hình các chức năng chính của Winlogbeat là winlogbeat.yml.

Giống như Filebeat, phần đầu tiên cần được cấu hình là phần đầu vào của Winlogbeat (phần winlogbeat.event_logs). Đó là dữ liệu log mà ELK muốn thu thập như Application, System, Security, Sysmon,. . . như hình dưới đây.

Tiếp theo, Winlogbeat cần được cấu hình Kibana endpoint như Filebeat. Tương tự như Filebeat, host của Kibana có dạng: <Địa chỉ IP của máy ELK>:5601. Ở đây, 192.168.56.110 là địa chỉ IP của máy ELK.

Cuối cùng, Winlogbeat cần được cấu hình đầu ra. Trong đồ án này, dữ liệu log từ Winlogbeat sẽ được gửi trực tiếp đến Elasticsearch mà không cần qua Logstash để xử lý. Phần cấu hình này nằm ở mục output.elasticsearch như hình dưới đây. Các mục cần cấu hình là:

* hosts: Host của Elasticsearch. Nó có dạng <Địa chỉ IP của máy ELK>:5601. Ở đây, 192.168.56.110 là địa chỉ IP của máy ELK.

* username: Tên đăng nhập vào Elasticsearch.

* password: Mật khẩu đăng nhập vào Elasticsearch.

Tên đăng nhập và mật khẩu đăng nhập vào Elasticsearch sẽ được đề cập cụ thể ở phần sau.

## Logstash

## Elasticsearch

## Kibana

## Tạo rule

## Tạo alert

# Kết quả
