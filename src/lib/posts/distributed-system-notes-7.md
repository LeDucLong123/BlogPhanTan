---
title: 'Distributed System notes 7'
date: '2025-5-17'
updated: '2025-5-17'
categories:
  - 'sveltekit'
  - 'markdown'
coverImage: '/images/distributed-system-notes-7.png'
coverWidth: 16
coverHeight: 9
excerpt: So sánh và Mối liên hệ giữa HTTP, TCP/IP, UDP, REST, GraphQL, SOAP, AJAX, RPC, gRPC
---

## 🔗 So sánh và Mối liên hệ giữa HTTP, TCP/IP, UDP, REST, GraphQL, SOAP, AJAX, RPC, gRPC

### 1. 🧱 Giao thức nền tảng truyền dữ liệu

| Giao thức  | Mục đích sử dụng                               | Tính chất                    | Quan hệ                                                |
| ---------- | ---------------------------------------------- | ---------------------------- | ------------------------------------------------------ |
| **TCP/IP** | Giao tiếp mạng đáng tin cậy giữa 2 thiết bị    | Kết nối, tin cậy, tuần tự    | Giao thức tầng thấp nền tảng cho HTTP                  |
| **UDP**    | Truyền dữ liệu nhanh, không đảm bảo độ tin cậy | Không kết nối, không tin cậy | Dùng trong các ứng dụng cần tốc độ cao như video, game |

### 2. 🌐 Giao thức tầng ứng dụng

| Giao thức   | Mục đích sử dụng                                             | Dựa trên       | Quan hệ                                                         |
| ----------- | ------------------------------------------------------------ | -------------- | --------------------------------------------------------------- |
| **HTTP**    | Giao tiếp giữa client và server qua web                      | TCP/IP         | Nền tảng cho REST, GraphQL, SOAP, AJAX                          |
| **REST**    | Giao tiếp client-server qua API theo tài nguyên              | HTTP           | Là kiến trúc API phổ biến                                       |
| **GraphQL** | API cho phép client yêu cầu đúng dữ liệu cần thiết           | HTTP           | Thay thế REST linh hoạt hơn                                     |
| **SOAP**    | Giao thức API dạng XML, dùng trong các hệ thống doanh nghiệp | HTTP hoặc SMTP | Phức tạp, nhưng có tính bảo mật cao                             |
| **AJAX**    | Kỹ thuật gửi yêu cầu HTTP bất đồng bộ từ trình duyệt         | HTTP           | Dùng trong front-end để tương tác server mà không tải lại trang |

### 3. 📦 Giao tiếp từ xa (Remote Procedure Call)

| Công nghệ | Mục đích sử dụng                        | Giao thức sử dụng              | Quan hệ                                          |
| --------- | --------------------------------------- | ------------------------------ | ------------------------------------------------ |
| **RPC**   | Gọi hàm từ xa như gọi hàm nội bộ        | Tuỳ nền tảng (TCP, HTTP, v.v.) | Khái niệm tổng quát                              |
| **gRPC**  | RPC hiệu năng cao dùng protocol buffers | HTTP/2                         | Hiện đại hơn, nhanh hơn REST, hỗ trợ đa ngôn ngữ |

---

### 📌 Tổng kết mối liên hệ

- **TCP/IP** và **UDP** là tầng truyền tải, nền tảng cho các giao thức khác.
- **HTTP** hoạt động trên **TCP/IP**, là nền cho REST, SOAP, GraphQL, AJAX.
- **REST**, **GraphQL**, **SOAP** là các cách xây dựng API hoạt động trên HTTP.
- **AJAX** là kỹ thuật dùng HTTP để giao tiếp bất đồng bộ trong web.
- **RPC** và **gRPC** là mô hình giao tiếp gọi hàm từ xa, gRPC hiện đại hơn, hoạt động trên HTTP/2.

> 🔍 Các giao thức/tầng này có mối quan hệ phân cấp:  
> **TCP/IP** → **HTTP** → **REST / GraphQL / SOAP / gRPC / AJAX**

---

## ⚙️ Tìm hiểu OpenMPI và Giải pháp tính số nguyên tố trên hệ phân tán

### 🧠 1. Thư viện OpenMPI

**OpenMPI** là một thư viện **Message Passing Interface (MPI)** mã nguồn mở, dùng để **lập trình song song phân tán** trên nhiều máy tính hoặc nhiều core.

#### 🔹 Tính năng chính:

- Giao tiếp giữa các tiến trình qua mạng hoặc nội bộ (intra-node và inter-node).
- Hỗ trợ cả song song chia sẻ bộ nhớ và phân tán bộ nhớ.
- Khả năng mở rộng tốt cho các hệ thống HPC (High Performance Computing).
- Dễ sử dụng với ngôn ngữ C/C++ và Fortran.

#### 🔧 Một số hàm MPI cơ bản:

| Hàm                                    | Mô tả                                                |
| -------------------------------------- | ---------------------------------------------------- |
| `MPI_Init(&argc, &argv)`               | Khởi tạo môi trường MPI                              |
| `MPI_Comm_size(MPI_COMM_WORLD, &size)` | Lấy tổng số tiến trình                               |
| `MPI_Comm_rank(MPI_COMM_WORLD, &rank)` | Lấy ID của tiến trình hiện tại                       |
| `MPI_Send()` / `MPI_Recv()`            | Gửi/nhận dữ liệu giữa các tiến trình                 |
| `MPI_Bcast()`                          | Phát dữ liệu từ một tiến trình đến tất cả tiến trình |
| `MPI_Reduce()`                         | Tổng hợp dữ liệu từ nhiều tiến trình                 |
| `MPI_Finalize()`                       | Kết thúc chương trình MPI                            |

---

### 📌 2. Bài toán: Tính 10,000,000 số nguyên tố đầu tiên bằng 32 core (16 máy, mỗi máy 2 core)

#### 🎯 Mục tiêu:

- Tính đúng 10 triệu số nguyên tố.
- Tận dụng tối đa sức mạnh của 32 core.
- Có thể linh hoạt với số core khác: 8, 10, 12, v.v.

---

### 🛠️ 3. Ý tưởng giải pháp sử dụng MPI

#### ✅ Phân tích:

- Tính số nguyên tố là bài toán **CPU-bound**, lý tưởng để song song hóa.
- Dễ chia thành các phần độc lập vì mỗi tiến trình có thể kiểm tra các số riêng biệt.

#### ✅ Hướng tiếp cận:

1. **Phân chia phạm vi kiểm tra số nguyên tố** cho từng tiến trình.
2. Mỗi tiến trình sẽ:
   - Tìm các số nguyên tố trong khoảng được giao.
   - Gửi danh sách hoặc số lượng về cho tiến trình `rank 0`.
3. **Tiến trình gốc (master)** sẽ tổng hợp kết quả.

#### 🔄 Chiến lược phân chia:

- Tổng số tiến trình = số core (tự động xác định bằng `MPI_Comm_size`)
- Mỗi tiến trình tìm số nguyên tố trong khoảng:  
  `start = rank * N / size`  
  `end = (rank + 1) * N / size`

---

### 💡 4. Đảm bảo đúng và linh hoạt số core

- **Đúng**: Kết quả được tổng hợp bởi master, không phụ thuộc số core vì mỗi đoạn xử lý riêng biệt và không chồng lặp.
- **Linh hoạt**: Khi chạy chương trình, MPI sẽ tự điều chỉnh `size` và `rank` → chỉ cần chạy lệnh với số core tương ứng:

```bash
mpirun -np 32 ./prime_calculator     # 32 core
mpirun -np 12 ./prime_calculator     # 12 core
```

- VD:

```c

#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

int is_prime(int n) {
    if (n < 2) return 0;
    for (int i = 2; i <= sqrt(n); i++)
        if (n % i == 0) return 0;
    return 1;
}

int main(int argc, char** argv) {
    int rank, size, count = 0;
    const int target = 10000000;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    int number = 2 + rank;
    while (1) {
        if (is_prime(number)) count++;
        if (count >= target / size) break;
        number += size;
    }

    int total = 0;
    MPI_Reduce(&count, &total, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Tổng số nguyên tố tìm được: %d\n", total);
    }

    MPI_Finalize();
    return 0;
}


```
