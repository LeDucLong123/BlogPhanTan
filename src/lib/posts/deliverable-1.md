---
title: 'Deliverable 1: Giới thiệu InfluxDB'
date: '2025-5-5'
updated: '2025-5-18'
categories:
  - 'sveltekit'
  - 'markdown'
coverImage: '/images/gioi-thieu-influx/gioi-thieu-influx.png'
coverWidth: 16
coverHeight: 9
excerpt: InfluxDB dùng để tương tác với cơ sở dữ liệu chuỗi thời gian ...
---

## Đề xuất đề tài: Giám sát lưu lượng request theo thời gian thực trong hệ thống web phân tán sử dụng InfluxDB và Docker

### 1. Mô tả đề tài

Hệ thống web hiện đại thường được triển khai theo mô hình **phân tán**, nhằm mục tiêu mở rộng (scalability), tăng khả năng chịu lỗi (fault tolerance) và đáp ứng lượng người dùng lớn. Một vấn đề đặt ra trong các hệ thống như vậy là **giám sát lưu lượng truy cập** theo thời gian thực, đặc biệt là trong môi trường có **cân bằng tải (load balancing)**, nơi mỗi máy chủ backend chỉ biết đến một phần lưu lượng.

Đề tài này đề xuất xây dựng một hệ thống **giám sát lưu lượng request theo mỗi giây (requests per second - RPS)** sử dụng:

- **InfluxDB**: cơ sở dữ liệu chuỗi thời gian (time-series database) để ghi nhận và phân tích dữ liệu request.
- **NGINX**: đóng vai trò cân bằng tải và có khả năng ghi log request.
- **Docker Compose**: để triển khai toàn bộ hệ thống theo kiến trúc phân tán.
- **Node.js backend (hoặc ứng dụng web)**: nơi tạo ra request hoặc thu thập từ log.

---

### 2. Vấn đề cần giải quyết

- Làm sao để ghi nhận chính xác số lượng request truy cập hệ thống trong môi trường có nhiều container backend?
- Làm sao để thu thập dữ liệu thống kê đó theo thời gian thực và lưu trữ hiệu quả?
- Làm sao để xử lý, trực quan hóa hoặc cảnh báo khi lưu lượng tăng đột biến?

---

### 3. Lý do chọn đề tài

- **Thực tiễn cao**: Bài toán theo dõi RPS rất phổ biến trong hệ thống microservices và cloud-based systems.
- **Phù hợp với mô hình phân tán**: Kết hợp nhiều thành phần độc lập (InfluxDB, NGINX, Node.js) theo kiểu loosely-coupled.
- **Tính mở rộng và đo lường tốt**: Có thể dễ dàng áp dụng cho hệ thống thật hoặc mở rộng để tích hợp cảnh báo (alerting), dashboard (Grafana)...

---

### 4. Tổng quan về thư viện InfluxDB

#### ✔️ Bản chất

InfluxDB là một cơ sở dữ liệu **chuỗi thời gian (time-series)**, được thiết kế để lưu trữ dữ liệu dạng `(timestamp, value)` tối ưu. Ví dụ: số lượng request tại mỗi thời điểm.

#### ✔️ Điểm mạnh

- Ghi dữ liệu tốc độ cao (hàng ngàn bản ghi/giây)
- Truy vấn mạnh với ngôn ngữ Flux/InfluxQL
- Hỗ trợ retention policy, continuous queries, downsampling...
- Tối ưu cho dữ liệu thời gian (CPU, request, memory...)

#### ❌ Điểm yếu

- Không phù hợp cho dữ liệu phi-thời-gian (user info, sản phẩm, v.v.)
- Cần cấu hình phù hợp nếu tải lớn để tránh mất dữ liệu
- Không thay thế được database như MySQL/PostgreSQL trong nhiều bài toán

#### ✅ Trường hợp áp dụng tốt

- Monitoring (CPU, memory, request/sec)
- IoT sensor data
- Log aggregation theo thời gian
- Tạo dashboard realtime (kết hợp với Grafana)

---

### 5. Kết luận

Đề tài không chỉ giúp áp dụng các kiến thức về hệ thống phân tán như cân bằng tải, containerization, thu thập log, mà còn tiếp cận được với công nghệ phổ biến trong giám sát hệ thống thực tế. Việc sử dụng InfluxDB như một backend lưu trữ dữ liệu chuỗi thời gian là lựa chọn phù hợp cho mục tiêu đo đạc và theo dõi RPS một cách hiệu quả.
