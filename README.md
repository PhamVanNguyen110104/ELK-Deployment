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

Hình dưới đây mô tả kiến trúc hệ thống sẽ triển khai. Cụ thể, Linux log được xử lý như sau: Linux log -> Filebeat -> Logstash -> Elasticearch. Còn Windows log được xử lý như sau: Windows log -> Winlogbeat, Elasticsearch.



# Kết quả
