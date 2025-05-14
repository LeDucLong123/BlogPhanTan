---
title: 'Định Danh'
date: '2025-5-15'
updated: '2025-5-15'
categories:
  - 'sveltekit'
  - 'markdown'
coverImage: '/images/dinh-danh/dinh-danh.jpg'
coverWidth: 16
coverHeight: 9
excerpt: Đảm bảo thông điệp được gửi đi không bị mất ...
---

## 1. Cách thức hoạt động của một trang web - Mô hình Client và Server

**<a href="https://viblo.asia/p/dieu-gi-xay-ra-khi-ban-truy-cap-mot-trang-web-E375zaAblGW" target="_blank" style="color:red">- Nguồn tham khảo -</a>**

Mô hình **Client-Server** là một mô hình nổi tiếng trong mạng máy tính, được áp dụng rất rộng rãi và là mô hình của mọi trang web hiện nay.

- **Client** (máy khách): là những thiết bị kết nối mạng của người dùng Internet (ví dụ: máy tính, điện thoại) và phần mềm như trình duyệt (Firefox, Chrome, Safari,...).
- **Server** (máy chủ): là máy tính lưu trữ trang web hoặc ứng dụng. Khi một Client muốn truy cập một trang web, Server sẽ gửi bản sao của trang web đó về Client để hiển thị trong trình duyệt.

> Mô hình hoạt động:  
> **Client gửi request → Server xử lý → Trả response về Client**

---

## 2. Những thành phần khác trong quy trình

Mô hình Client-Server chỉ là phần cơ bản. Để một trang web hoạt động đầy đủ, còn có các thành phần sau:

### 🔌 Mạng Internet bạn kết nối

- Là "con đường" truyền dữ liệu giữa Client và Server.
- Giống như con đường nối từ nhà bạn đến siêu thị (Vinmart) trong ví dụ minh họa.

### 📦 Giao thức TCP/IP

- Là tập hợp các giao thức dùng để truyền dữ liệu qua mạng.
- Mỗi tầng xử lý một nhiệm vụ khác nhau, từ tầng vật lý đến tầng ứng dụng.
- Ví dụ: giống như phương tiện di chuyển của bạn đến Vinmart — xe máy, ô tô, hay đi bộ 🚶‍♂️.

### 🌐 DNS (Domain Name System)

- Hệ thống chuyển đổi tên miền (VD: google.com) thành địa chỉ IP tương ứng.
- Trình duyệt sử dụng DNS để tìm ra IP trước khi truy cập trang web.
- Giống như việc bạn tra địa chỉ Vinmart để đi mua đồ.

### 📡 HTTP (HyperText Transfer Protocol)

- Giao thức tầng ứng dụng dùng để gửi và nhận thông tin giữa Client và Server.
- Giống như "ngôn ngữ giao tiếp" khi bạn nói chuyện với người bán hàng tại Vinmart.

### 🧱 Component files (Tệp thành phần)

Một trang web bao gồm nhiều tệp:

- **Code files**: HTML, CSS, JS, PHP,...
- **Asset files**: hình ảnh, video, tài liệu,...

> Tưởng tượng như nhiều món đồ khác nhau bạn mua ở Vinmart.

---

## 3. Quy trình cụ thể khi duyệt một trang web

1. **Trình duyệt** gọi tới **DNS server** để biên dịch URL thành địa chỉ IP.
2. DNS trả về địa chỉ IP của trang web.
3. Trình duyệt gửi **HTTP request** tới địa chỉ IP thông qua **giao thức TCP/IP**, qua **cổng 80**.
4. **Server** nhận request, phản hồi với mã `200 OK` nếu thành công.
5. Trình duyệt nhận và đọc **mã HTML** từ server.
6. Trang web được **hiển thị** hoàn chỉnh trên trình duyệt.
7. Khi bạn **đóng trình duyệt**, quá trình kết nối với server kết thúc.

---

## 1. 📡 Chord HTB trong Hệ thống Phân tán

**Chord DHT (Distributed Hash Table)** là một thuật toán định tuyến được thiết kế cho hệ thống phân tán. Mỗi node được gán một ID duy nhất, và dữ liệu được lưu trữ dựa trên giá trị hash của key. Chord giúp xác định node nào sẽ lưu trữ một key cụ thể một cách hiệu quả, với độ phức tạp O(log N).

---

## 2. Ví dụ cụ thể về Chord

Giả sử ta có 5 node trong mạng Chord, với ID như sau (trong không gian ID mod 32):

- Node A: ID 1
- Node B: ID 8
- Node C: ID 14
- Node D: ID 21
- Node E: ID 32 (tức 0, vì mod 32)

### Dữ liệu cần lưu: `"apple"`

- Tính `hash("apple") mod 32` → Ví dụ giả sử `hash("apple") = 18`

➡️ Node gần nhất **sau 18** theo chiều kim đồng hồ là **Node D (ID 21)**  
→ `"apple"` sẽ được lưu ở **Node D**

---

## 3. Finger Table minh hoạ

Tại Node A (ID = 1), finger table sẽ như sau:

| Finger Entry | Start (1 + 2^i) mod 32 | Successor |
| ------------ | ---------------------- | --------- |
| i = 0        | 2                      | B (8)     |
| i = 1        | 3                      | B (8)     |
| i = 2        | 5                      | B (8)     |
| i = 3        | 9                      | C (14)    |
| i = 4        | 17                     | D (21)    |

→ Giúp Node A truy vấn nhanh thay vì đi từng bước

---

## 4. Kết quả của một test case

### Test: Tìm nơi lưu `"banana"`

```text
hash("banana") mod 32 = 10


```

- Các node theo ID: 1, 8, 14, 21, 32

- 10 nằm giữa 8 và 14 → Sẽ được lưu ở Node C (ID 14)

✅ Kết quả mong đợi: "banana" được lưu tại node có ID ≥ 10 đầu tiên → là Node 14

- Code thực nghiệm (JavaScript)

```javascript
class Node {
	constructor(id) {
		this.id = id;
		this.data = {};
	}

	store(key, value, hashFn, nodes) {
		const keyId = hashFn(key);
		const target = nodes.find((n) => n.id >= keyId) || nodes[0];
		console.log(`Key "${key}" (hash=${keyId}) stored at Node ${target.id}`);
		target.data[key] = value;
	}
}

// Hàm băm đơn giản (chỉ dùng minh hoạ)
function simpleHash(str) {
	let hash = 0;
	for (let i = 0; i < str.length; i++) {
		hash = (hash + str.charCodeAt(i)) % 32;
	}
	return hash;
}

// Khởi tạo các node
const nodes = [1, 8, 14, 21, 32].map((id) => new Node(id));
nodes.sort((a, b) => a.id - b.id);

// Lưu dữ liệu
nodes[0].store('apple', '🍎', simpleHash, nodes);
nodes[0].store('banana', '🍌', simpleHash, nodes);
```

---
