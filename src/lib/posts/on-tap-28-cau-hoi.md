---
title: 'Ôn tập 28 câu hỏi'
date: '2025-5-28'
updated: '2025-5-28'
categories:
  - 'sveltekit'
  - 'markdown'
coverImage: '/images/on-tap.jpg'
coverWidth: 16
coverHeight: 9
excerpt: Phân biệt Hệ thống Phân tán, Tập trung, Phi ...
---

## 1. Phân biệt Hệ thống Phân tán, Tập trung, Phi tập trung

| Loại hệ thống                 | Mô tả                                              | Ví dụ                                        |
| ----------------------------- | -------------------------------------------------- | -------------------------------------------- |
| Tập trung (Centralized)       | Một nút trung tâm xử lý mọi yêu cầu.               | Máy chủ cơ sở dữ liệu đơn trong công ty nhỏ. |
| Phân tán (Distributed)        | Dữ liệu và tính toán được phân tán trên nhiều nút. | Hệ thống Hadoop, hệ thống web phân tán.      |
| Phi tập trung (Decentralized) | Không có nút trung tâm, mỗi nút đều ngang hàng.    | Blockchain, BitTorrent.                      |

**Sự khác biệt chính** là ở mức độ **phụ thuộc vào nút trung tâm** và **phân phối trách nhiệm** giữa các nút.

---

## 2. Đặc tính của Hệ thống Phân tán

1. **Transparency**: Che giấu tính phức tạp khỏi người dùng (vị trí, di trú, đồng thời, lỗi…).
2. **Openness**: Hỗ trợ chuẩn mở, dễ tích hợp.
3. **Scalability**: Dễ dàng mở rộng quy mô.
4. **Concurrency**: Hỗ trợ nhiều tiến trình đồng thời.
5. **Fault tolerance**: Chịu lỗi, tiếp tục hoạt động khi một phần bị hỏng.
6. **Resource sharing**: Chia sẻ tài nguyên giữa các nút.
7. **Heterogeneity**: Hoạt động trên nhiều loại máy và hệ điều hành khác nhau.

---

## 3. Mục đích và vai trò của nút chủ

- Nút chủ điều phối, quản lý tài nguyên, lịch trình.
- Nếu nút chủ gặp sự cố:
  - Toàn hệ thống có thể dừng (nếu không có cơ chế thay thế).
  - Cần cơ chế chọn lại leader (Bully, Ring...).

---

## 4. Gossip Protocol trong mạng không gian

- **Tại sao dùng Gossip Protocol**:
  - Hiệu quả: Truyền dần từng phần thay vì broadcast.
  - Bền vững: Không tạo gánh nặng cho trung tâm.
  - Linh hoạt: Hỗ trợ phát hiện lỗi, đồng bộ trạng thái.

---

## 5. Các yếu tố cốt lõi của hệ phân tán

1. **Nút mạng (nodes)**
2. **Kết nối mạng (networking)**
3. **Đồng bộ thời gian**
4. **Truyền thông giữa các tiến trình**
5. **Quản lý lỗi**

---

## 6. Lý do sử dụng hệ phân tán

- Hiệu suất cao hơn
- Khả năng mở rộng
- Độ tin cậy và sẵn sàng cao
- Chia sẻ tài nguyên
- Tính linh hoạt và phân tán địa lý

---

## 7. Định nghĩa Hệ thống Phân tán

> Hệ thống phân tán là tập hợp các máy tính độc lập, giao tiếp với nhau qua mạng để cùng thực hiện một nhiệm vụ, nhưng được nhìn nhận như một hệ thống thống nhất.

---

## 8. Hình ảnh thuật toán đồng bộ

- Cristian's Algorithm
  ![Cristian's Algorithm](/images/algorithm/cristian.png)
- Berkeley Algorithm
  ![Berkeley Algorithm](/images/algorithm/berkeley.png)
- RBS (Reference Broadcast Synchronization)
  ![RBS](/images/algorithm/RBS.png)
- Lamport Clock
  ![Lamport Clock](/images/algorithm/clocks-lamport.png)
- Bully Algorithm
  ![Bully Algorithm](/images/algorithm/bully.png)
- Ring Election Algorithm
  ![Ring Election Algorithm](/images/algorithm/ring-election.png)

---

## 9. Kỹ thuật phân tán và mô hình lập trình

| Loại kỹ thuật     | Hỗ trợ mô hình lập trình             |
| ----------------- | ------------------------------------ |
| RPC               | Thủ tục (Procedural)                 |
| CORBA             | Hướng đối tượng (OOP)                |
| REST/Web Services | Lập trình web                        |
| RMI               | Hướng đối tượng                      |
| Actor Model       | Lập trình song song, message-passing |

---

## 10. Tiến trình nhẹ, tiến trình, luồng

| Loại                 | Ưu điểm                | Nhược điểm             | Khi gọi hệ thống                  |
| -------------------- | ---------------------- | ---------------------- | --------------------------------- |
| Tiến trình           | Bảo mật tốt, tách biệt | Chi phí cao            | Bị chặn toàn bộ                   |
| Luồng                | Nhẹ, chia sẻ bộ nhớ    | Gây xung đột dữ liệu   | Gọi hệ thống chặn thread hiện tại |
| Tiến trình nhẹ (LWP) | Kết hợp ưu điểm        | Phức tạp khi lập trình | Gọi hệ thống chặn LWP đó          |

**Quan hệ**: Tiến trình chứa nhiều luồng, mỗi luồng có thể là LWP.

---

## 11. Mô hình Client-Server

- **Client**: Gửi yêu cầu, tương tác người dùng.
- **Server**: Xử lý, phản hồi yêu cầu.
- **Client-Server**: Kiến trúc phân chia nhiệm vụ.

---

## 12. Gọi thủ tục từ xa (RPC)

- Cho phép gọi hàm từ máy khác như gọi cục bộ.
- Ẩn chi tiết truyền thông mạng.
- Cần serialize/deserialize dữ liệu.

---

## 13. Mô hình phân tầng giao thức

- Mỗi tầng xử lý một nhiệm vụ (ví dụ: TCP/IP).
- **Lợi ích**:
  - Dễ quản lý, phát triển.
  - Tách biệt trách nhiệm.
  - Tăng tính bền vững và tái sử dụng.

---

## 14. Sharding là gì

- **Sharding**: Kỹ thuật chia nhỏ dữ liệu theo chiều ngang.
- **Mục tiêu**: Tăng hiệu năng, giảm tải mỗi máy.
- Ví dụ: MongoDB, Cassandra.

---

## 15. Nhiệm vụ của gói luồng (Thread package)

- Tạo luồng
- Quản lý chuyển ngữ cảnh
- Lập lịch luồng
- Đồng bộ hóa luồng (mutex, semaphore)

---

## 16. Phân loại luồng

| Loại                          | Ưu điểm                 | Nhược điểm            |
| ----------------------------- | ----------------------- | --------------------- |
| Luồng người dùng (User-level) | Nhanh, không cần kernel | Không tận dụng đa lõi |
| Luồng nhân (Kernel-level)     | Đa lõi, mạnh mẽ         | Gọi hệ thống chậm hơn |

---

## 17. Các hàm chính trong RPC

1. `client_stub()` - serialize và gửi yêu cầu
2. `server_stub()` - nhận và deserialize yêu cầu
3. `bind()` - kết nối client-server
4. `dispatch()` - gọi hàm tương ứng
5. `reply()` - gửi lại kết quả

---

## 18. Định nghĩa

- **Process (Tiến trình)**: Chương trình đang thực thi, độc lập.
- **Thread (Luồng)**: Đơn vị nhỏ hơn process, chia sẻ bộ nhớ.
- **Multithread client/server**: Cho phép xử lý nhiều yêu cầu cùng lúc bằng nhiều luồng.

---

## 19. MapReduce

- **Map**: Chuyển dữ liệu đầu vào thành cặp key-value.
- **Reduce**: Tổng hợp dữ liệu theo key.
- **Mục tiêu**: Xử lý dữ liệu lớn hiệu quả trong hệ phân tán.

---

## 20. Ảo hóa (Virtualization)

- Là kỹ thuật tạo ra môi trường ảo thay vì vật lý.
- **Mục tiêu trong hệ phân tán**:
  - Tăng hiệu quả sử dụng tài nguyên.
  - Hỗ trợ dễ dàng mở rộng, triển khai.
  - Tách biệt ứng dụng, tăng độ an toàn.

---

## 21. Review các kiến trúc của Server Đa luồng

Các kiến trúc phổ biến của **Server đa luồng**:

1. **Thread-per-request (Mỗi yêu cầu 1 luồng)**

   - Mỗi yêu cầu từ client tạo 1 thread xử lý.
   - Dễ triển khai nhưng khó mở rộng nếu số lượng kết nối lớn.

2. **Thread pool**

   - Tạo sẵn một nhóm luồng, tái sử dụng cho các yêu cầu.
   - Cân bằng giữa hiệu suất và tài nguyên.

3. **Event-driven with thread pool (Kết hợp bất đồng bộ + đa luồng)**
   - Luồng chính xử lý I/O, thread pool xử lý logic.
   - Tối ưu cho các ứng dụng web hiệu năng cao (như Node.js + worker threads).

---

## 22. Review các hướng tiếp cận cài đặt luồng

1. **User-level threads (ULT)**

   - Tạo và quản lý trong không gian người dùng.
   - Ưu điểm: nhanh, không cần kernel.
   - Nhược điểm: không tận dụng được đa lõi CPU.

2. **Kernel-level threads (KLT)**

   - Kernel quản lý, hỗ trợ scheduling.
   - Ưu điểm: hiệu quả với đa lõi, hỗ trợ blocking tốt.
   - Nhược điểm: gọi hệ thống chậm hơn.

3. **Hybrid (n:m model)**
   - Kết hợp ULT và KLT.
   - Tận dụng lợi ích của cả hai loại.

---

## 23. Tại sao sử dụng Bảng băm phân tán (DHT)

### Mục đích của DHT:

- Lưu trữ phân tán các cặp khóa-giá trị mà không cần máy chủ trung tâm.
- Tự mở rộng và tự phục hồi.

### Consistent Hashing là gì?

- Kỹ thuật ánh xạ khóa vào không gian vòng tròn (hash ring).
- Khi thêm hoặc xóa node, chỉ ảnh hưởng đến một phần nhỏ dữ liệu.

### Finger Table là gì?

- Cấu trúc dữ liệu trong thuật toán Chord.
- Giúp mỗi node biết khoảng `log N` nút khác để định tuyến nhanh.

### Tại sao dùng Finger Table?

- **Tăng tốc tìm kiếm**: từ O(N) xuống còn O(log N).
- **Giảm tải mạng**: ít hop hơn giữa các node.

---

## 24. Không gian phẳng và định danh

### Không gian phẳng (Flat Name Space):

- Không có cấu trúc phân cấp, mỗi thực thể chỉ có một định danh duy nhất.

### Định danh (Identifier):

- Chuỗi ký tự (thường là hash hoặc số nguyên) dùng để xác định duy nhất một thực thể (node, dữ liệu, thiết bị…).

### Đặc điểm của không gian phẳng:

- **Toàn cục**: định danh không trùng.
- **Không cấu trúc**: không phân cấp như DNS.
- **Khó tìm kiếm** nếu không có bảng tra cứu.

---

## 25. Đồng bộ hóa đồng hồ logic và đồng hồ vật lý

### Vì sao đồng hồ vật lý không đảm bảo?

- Sai lệch do tốc độ dao động khác nhau.
- Giao tiếp mạng có độ trễ không ổn định.

### Mục đích của đồng bộ hóa:

- Đảm bảo thứ tự các sự kiện phân tán.
- Xác định "ai đến trước".

### Các thuật toán đồng bộ:

1. **Cristian’s Algorithm** – dùng máy chủ thời gian.
2. **Berkeley Algorithm** – dùng đồng bộ tập thể.
3. **NTP** – dùng trong thực tế qua Internet.
4. **RBS** – broadcast timestamp qua kênh chung.
5. **Lamport Clock** – đồng hồ logic, không cần thời gian thực.

---

## 26. Đồng hồ Lamport

### Giải quyết vấn đề gì?

- Thứ tự **nhân quả (causality)** của các sự kiện trong hệ thống phân tán.
- Không cần đồng hồ vật lý chính xác.

### Các quy tắc (Rules) của Lamport Clock:

1. **LC1 - Rule nội bộ**:

   - Mỗi khi tiến trình xảy ra sự kiện, tăng đồng hồ: `L := L + 1`.

2. **LC2 - Rule gửi**:

   - Trước khi gửi thông điệp, tăng đồng hồ: `L := L + 1`.
   - Gửi đồng hồ hiện tại đi kèm thông điệp.

3. **LC3 - Rule nhận**:
   - Khi nhận thông điệp, đồng hồ tiến trình cập nhật:
     ```txt
     L := max(L, Lm) + 1
     ```
   - `Lm` là giá trị đồng hồ từ thông điệp.

### Kết quả:

- Nếu sự kiện A xảy ra trước B, thì `L(A) < L(B)`, nhưng ngược lại chưa chắc đúng.

---
