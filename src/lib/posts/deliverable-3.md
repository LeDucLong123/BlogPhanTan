---
title: 'Deliverable 3: Tiến độ dự án'
date: '2025-5-19'
updated: '2025-5-19'
categories:
  - 'sveltekit'
  - 'markdown'
coverImage: '/images/deliverable-3.jpg'
coverWidth: 16
coverHeight: 9
excerpt: Kiến trúc hệ thống (System Architecture Diagram)...
---

## 📊 Tóm tắt tiến độ dự án

### 🔧 Tên đề tài:

**Giám sát lưu lượng request trên hệ thống web phân tán sử dụng InfluxDB và Docker**

---

### ✅ Các hạng mục đã hoàn thành:

- [x] **Xác định đề tài và mục tiêu hệ thống**

  - Theo dõi lượng request mỗi giây (RPS) trên hệ thống web phân tán
  - Sử dụng kiến trúc microservices triển khai bằng Docker

- [x] **Xây dựng kiến trúc hệ thống**

  - Client → Nginx Load Balancer → 3 App Node.js → InfluxDB riêng biệt → API → View

- [x] **Cấu hình Docker Compose**

  - 8 container: `influxdb-1/2/3`, `app-1/2/3`, `nginx`, `api`, `view`, `request`
  - Mapping volume cho InfluxDB để đảm bảo dữ liệu bền vững
  - Biến môi trường cấu hình động các thành phần

- [x] **Triển khai middleware ghi log vào InfluxDB**

  - Sử dụng `Point` và `writeApi` để ghi dữ liệu mỗi request
  - Ghi nhận method, path, port, status, duration

- [x] **Thiết kế mô hình dữ liệu InfluxDB**

  - Measurement: `web-requests`
  - Tags: `method`, `path`, `port`
  - Fields: `status`, `duration_ms`

- [x] **Triển khai API đọc dữ liệu**

  - API truy vấn dữ liệu từ các InfluxDB để cung cấp cho frontend

- [x] **Triển khai frontend hiển thị**
  - Hiển thị biểu đồ thể hiện số lượng request mỗi giây theo thời gian

---

### 🚀 Đang triển khai (hoặc sắp thực hiện):

- [ ] Cần cập nhật thêm giao diện
- [ ] Tối ưu hóa hiệu năng khi số lượng request lớn
- [ ] Kết nối đồng bộ các InfluxDB (replication / federated query)
- [ ] Viết báo cáo tổng hợp, biểu đồ kiến trúc, phân tích dữ liệu

---

### 📌 Ghi chú:

- Demo trực tiếp sử dụng Docker để triển khai
- Hệ thống đã hoạt động ổn định qua Docker Compose
- Việc phân chia InfluxDB theo từng app giúp theo dõi độc lập hiệu suất từng node
- Có thể mở rộng thêm node ứng dụng và database một cách linh hoạt

---

### 🗓 Tiến độ: `~90%` ✅
