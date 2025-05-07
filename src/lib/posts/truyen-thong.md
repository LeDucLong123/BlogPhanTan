---
title: 'Truyền Thông'
date: '2025-5-6'
updated: '2025-5-6'
categories:
  - 'sveltekit'
  - 'markdown'
coverImage: '/images/truyen-thong.jpg'
coverWidth: 16
coverHeight: 9
excerpt: Chia nhỏ dữ liệu và xử lý song song để tiết kiệm thời gian.
---

## 1. Tìm hiểu dịch vụ truyền thông điệp

### Dịch vụ được chọn: **RabbitMQ**

#### 1.1 Cơ chế hoạt động

RabbitMQ là một **message broker** hỗ trợ **AMQP (Advanced Message Queuing Protocol)**. Cơ chế hoạt động cơ bản gồm các thành phần:

- **Producer**: Gửi thông điệp đến một exchange.
- **Exchange**: Chuyển tiếp thông điệp đến các hàng đợi (queues) theo một logic định sẵn (direct, topic, fanout,...).
- **Queue**: Hàng đợi lưu trữ thông điệp cho đến khi được xử lý.
- **Consumer**: Nhận và xử lý thông điệp từ queue.

#### 1.2 Chức năng chính

- Giao tiếp bất đồng bộ giữa các hệ thống.
- Đảm bảo thông điệp được gửi đi không bị mất (persisted).
- Có thể mở rộng và phân tán.

#### 1.3 Cài đặt RabbitMQ (trên Linux)

```bash
# Cài đặt Erlang (RabbitMQ yêu cầu Erlang)
sudo apt update
sudo apt install erlang

# Cài đặt RabbitMQ
sudo apt install rabbitmq-server

# Khởi động dịch vụ
sudo systemctl start rabbitmq-server
sudo systemctl enable rabbitmq-server

# Kiểm tra trạng thái
sudo systemctl status rabbitmq-server

```

## 2. Demo hệ thống sử dụng RabbitMQ (Python)

### 2.1 Cài đặt thư viện

```bash
pip install pika
```

### 2.2 Producer (send.py)

```python
import pika

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

channel.queue_declare(queue='hello')

channel.basic_publish(exchange='',
                      routing_key='hello',
                      body='Hello RabbitMQ!')
print(" [x] Sent 'Hello RabbitMQ!'")
connection.close()
```

### 2.3 Consumer (receive.py)

```python
import pika

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

channel.queue_declare(queue='hello')

def callback(ch, method, properties, body):
    print(f" [x] Received {body.decode()}")

channel.basic_consume(queue='hello',
                      on_message_callback=callback,
                      auto_ack=True)

print(' [*] Waiting for messages. To exit press CTRL+C')
channel.start_consuming()
```

---

## 3. RPC với định dạng JSON

### 3.1 Các thư viện RPC ngoài `xmlrpc`:

- **gRPC**
- **JSON-RPC** (thư viện: `json-rpc`, `jsonrpcserver`, `jsonrpcclient`)
- **ZeroRPC**
- **msgpack-rpc**

### 3.2 Demo với `jsonrpcserver` và `jsonrpcclient` (Python)

#### Cài đặt

```bash
pip install jsonrpcserver jsonrpcclient
```

#### Server (rpc_server.py)

```python
from jsonrpcserver import method, serve

@method
def add(a, b):
    return a + b

if __name__ == '__main__':
    serve()
```

#### Client (rpc_client.py)

```python
from jsonrpcclient import request
import requests

response = request("http://localhost:5000", "add", a=5, b=3)
print(response.data.result)  # Kết quả: 8
```

#### Chạy server:

```bash
python rpc_server.py
```
