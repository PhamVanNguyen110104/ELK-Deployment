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



## Winlogbeat

## Logstash

## Elasticsearch

## Kibana

## Tạo rule

## Tạo alert

# Kết quả
