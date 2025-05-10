---
title: 'Deliverable 1: Giới thiệu InfluxDB'
date: '2025-5-5'
updated: '2025-5-9'
categories:
  - 'sveltekit'
  - 'markdown'
coverImage: '/images/gioi-thieu-influx/gioi-thieu-influx.png'
coverWidth: 16
coverHeight: 9
excerpt: InfluxDB dùng để tương tác với cơ sở dữ liệu chuỗi thời gian ...
---

## 1. Tổng quan về thư viện InfluxDB

### Mục đích của thư viện

Thư viện **InfluxDB** dùng để tương tác với cơ sở dữ liệu chuỗi thời gian (time series database) – đặc biệt phù hợp với các dữ liệu như:

- Dữ liệu cảm biến IoT (Internet of Things)
- Dữ liệu logs hệ thống
- Dữ liệu theo dõi hiệu suất hệ thống
- Dữ liệu đo lường theo thời gian thực

Nó hỗ trợ việc ghi dữ liệu, truy vấn, quản lý cơ sở dữ liệu, và xử lý dữ liệu chuỗi thời gian một cách tối ưu.

### Vấn đề mà thư viện giải quyết

- Lưu trữ và truy vấn dữ liệu chuỗi thời gian hiệu quả.
- Xử lý khối lượng lớn dữ liệu liên tục theo thời gian.
- Giao tiếp với InfluxDB server thông qua giao thức HTTP hoặc gRPC.

### Điểm mạnh

- Tối ưu hóa cho dữ liệu chuỗi thời gian.
- Hỗ trợ giao thức **Flux query** linh hoạt và mạnh mẽ.
- Có khả năng lưu trữ dữ liệu với tần suất cao (ngay cả dữ liệu mili-giây).
- Có client cho nhiều ngôn ngữ như Python, Java, Go, Node.js…
- Tích hợp dễ dàng với Telegraf, Grafana, Kapacitor.

### Điểm yếu

- Chỉ phù hợp cho dữ liệu theo thời gian – không phù hợp cho dữ liệu quan hệ hoặc không có yếu tố thời gian.
- Yêu cầu hệ thống mạnh khi xử lý lượng lớn dữ liệu real-time.
- Flux query có cú pháp mới, cần thời gian học nếu đã quen với SQL.

### So sánh với các thư viện/framework khác

| Thư viện / Hệ thống | Kiểu dữ liệu hỗ trợ  | Mục tiêu chính         | Khả năng xử lý dữ liệu thời gian | Ngôn ngữ tích hợp |
| ------------------- | -------------------- | ---------------------- | -------------------------------- | ----------------- |
| **InfluxDB Core**   | Time Series          | Dữ liệu thời gian thực | Rất mạnh                         | Python, Java, Go… |
| MongoDB             | Document (JSON/BSON) | Dữ liệu linh hoạt      | Yếu nếu xử lý dữ liệu thời gian  | Python, Java, C#… |
| MySQL / PostgreSQL  | Quan hệ (SQL)        | Dữ liệu dạng bảng      | Có thể dùng nhưng không tối ưu   | SQL               |
| Prometheus          | Time Series          | Giám sát hệ thống      | Mạnh nhưng ít tùy biến hơn       | Go (chính), APIs  |

### Ứng dụng thực tế

- Hệ thống giám sát máy chủ.
- Phân tích hiệu suất thiết bị IoT.
- Lưu trữ và trực quan hóa dữ liệu cảm biến môi trường.
- Dự báo tiêu thụ năng lượng theo giờ.

---

## 2. Kế hoạch dự kiến cho bài giữa kỳ

### Chủ đề: **Phân tích dữ liệu cảm biến nhiệt độ từ thiết bị IoT**

#### Mô tả bài toán:

- Mỗi thiết bị IoT gửi dữ liệu nhiệt độ theo thời gian thực (1 lần/5 giây).
- Cần lưu trữ dữ liệu để:
  - Truy vấn biểu đồ nhiệt độ trong 1 giờ / 1 ngày / 1 tuần.
  - Tìm thời điểm nhiệt độ cao nhất, thấp nhất.
  - Phân tích xu hướng nhiệt độ theo giờ.

#### Mục tiêu:

- Dùng **InfluxDB Core Client (Python hoặc Java)** để:
  - Kết nối đến InfluxDB.
  - Ghi dữ liệu thời gian thực.
  - Truy vấn dữ liệu với Flux query.
- Vẽ biểu đồ trực quan bằng Matplotlib / Grafana.

- Mô phỏng dữ liệu cảm biến bằng cách sinh ngẫu nhiên bằng mã Python:

```python
from influxdb_client import InfluxDBClient, Point
from influxdb_client.client.write_api import SYNCHRONOUS
from datetime import datetime, timezone
import random  # Thêm import để tạo giá trị ngẫu nhiên
import time

# ✅ Thay bằng giá trị thật bạn copy được từ giao diện InfluxDB
url = "http://localhost:8086"
token = "theG93XjjKsPWUPFKigUhGLv3absn_6Ws_R6zuT9mK8-3-gcKWe7YAr3uJcu5tST5yEznaCapKLBFz0R-9xkpQ=="
org = "leduclong"
bucket = "leduclong"

client = InfluxDBClient(url=url, token=token, org=org)
write_api = client.write_api(write_options=SYNCHRONOUS)

while True:
    # ✅ Sinh nhiệt độ ngẫu nhiên trong khoảng 20.0°C - 35.0°C
    temperature = round(random.uniform(20.0, 35.0), 2)

    point = Point("temperature") \
        .tag("location", "office") \
        .field("value", temperature) \
        .time(datetime.now(timezone.utc))  # Sử dụng timezone-aware datetime

    write_api.write(bucket=bucket, org=org, record=point)
    print(f"✅ Đã ghi dữ liệu thành công! Giá trị: {temperature}°C")

    time.sleep(1)
```

```bash
✅ Đã ghi dữ liệu thành công! Giá trị: 33.98°C
✅ Đã ghi dữ liệu thành công! Giá trị: 20.42°C
✅ Đã ghi dữ liệu thành công! Giá trị: 30.41°C
✅ Đã ghi dữ liệu thành công! Giá trị: 22.5°C
✅ Đã ghi dữ liệu thành công! Giá trị: 29.41°C
...
```

![Mô phỏng](/images/gioi-thieu-influx/mo-phong.png)

- Hiển thị biểu đồ biểu diễn cho dữ liệu bằng dịch vụ của **InfluxDB**:

![Mô phỏng](/images/gioi-thieu-influx/hien-thi.png)

#### Kế hoạch thực hiện:

| STT | Nội dung công việc                           |
| --- | -------------------------------------------- |
| 1   | Tìm hiểu InfluxDB và thư viện InfluxDB Core  |
| 2   | Cài đặt môi trường (InfluxDB, Python client) |
| 3   | Mô phỏng thiết bị IoT gửi dữ liệu            |
| 4   | Ghi dữ liệu vào InfluxDB                     |
| 5   | Truy vấn dữ liệu bằng Flux                   |
| 6   | Phân tích và trực quan hóa dữ liệu           |
| 7   | Viết báo cáo, chuẩn bị trình bày giữa kỳ     |

---
