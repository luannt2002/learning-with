# 13 — P2: gRPC và giao tiếp giữa các service

> Video [Golang Micro service for highload backend Part2: GRPC](https://www.youtube.com/watch?v=VZoy3y3P6qY) (1:42:45). Cả tệp là 🎬 (bản ghi Gemini), trừ chỗ ghi 💡 / ⚠️. Nhãn xem [11](11-go-microservice-mindset.md).

---

## 1. Dàn ý

| Mốc | Nội dung |
|---|---|
| 00:00–05:40 | setup; host cảnh báo lạm dụng AI khi code (xem [11](11-go-microservice-mindset.md) §1) |
| 06:40–17:15 | Monolith vs Microservices: ưu, nhược, khi nào dùng |
| 17:15–20:00 | gRPC là gì, so với REST |
| 20:00–31:52 | Protobuf tới **tầng byte** |
| 31:53–38:52 | 4 kiểu RPC, so với WebSocket |
| 38:53–45:45 | Service Discovery |
| 45:46–57:30 | Load balancing L4 vs L7, sticky connection |
| 57:30–61:30 | bảo mật: binary không có nghĩa là an toàn |
| 61:30–74:45 | demo REST HTTP/1.1 + Wireshark |
| ~74:48–83:30 | demo gRPC + Wireshark, tổng kết |
| 87:30–101:30 | hỏi–đáp: scale dọc / ngang, mỗi service một DB, cascading failure |

---

## 2. Monolith hay Microservices

| | Monolith | Microservices |
|---|---|---|
| Hình dạng | 1 codebase, 1 DB, "deploy là deploy cả hệ thống" | mỗi service có code, deploy, scale, DB riêng |
| Ưu | dễ hiểu, dễ debug, team nhỏ làm rất nhanh | scale đúng chỗ nghẽn, cô lập lỗi |
| Nhược | scale một tính năng nặng phải nhân cả khối; đổi một module phải restart cả hệ; team lớn conflict liên tục | phức tạp; lỗi mạng, retry, timeout, tracing; khó giữ dữ liệu nhất quán, mất ACID đơn giản |
| Khi nào | team nhỏ, chưa có product-market fit | tách dần khi đã có PMF và có chỗ nghẽn thật |

Giao tiếp: REST cho public · gRPC cho nội bộ · message queue cho bất đồng bộ.

**Đừng chọn Microservices theo trend.** Bắt đầu bằng Monolith / Modular Monolith. [16:00]

---

## 3. gRPC và Protobuf

- **gRPC:** framework RPC mã nguồn mở của Google. Gọi hàm của service khác như gọi hàm local. Chạy trên HTTP/2, dùng Protobuf, nhiều ngôn ngữ, strongly typed. **Hợp nhất cho backend–backend nội bộ.**
- **`.proto` là hợp đồng** giữa hai bên; code sinh tự động từ đó.
- **JSON nặng vì mang tên field ở mọi bản tin**, không phải vì là text. Protobuf chỉ gửi số thứ tự field + wire type + giá trị.

**Tầng byte** (slide dùng bản tin `User` có 4 field: id, name, email, active):

```text
tag = (field_number << 3) | wire_type        wire type 0 = Varint, 2 = Length-delimited
id=123        → 08 7b                         (JSON "id":123 tốn 8 byte, Protobuf 2 byte)
name="Alice"  → 12 05 41 6c 69 63 65          (12 = field 2 kiểu chuỗi, 05 = độ dài)
field 3 kiểu chuỗi → (3 << 3) | 2 = 26 = 0x1a
```

💡 Đã tính lại ngày 28/10/2026 bằng Python: cả bốn dòng trên khớp.

Slide ngắt dòng cho dễ nhìn; trên đường truyền là một chuỗi byte liền. *Senior phải hiểu tầng byte để debug.* [29:50]

**4 kiểu RPC:**

| Kiểu | Dùng khi |
|---|---|
| Unary | một hỏi một đáp, phổ biến nhất |
| Server streaming | trả nhiều: lọc ra 900.000 bản ghi mà dùng Unary thì UI treo / tràn bộ nhớ; stream theo lô (vd 300.000) thì client hiện được ngay lô đầu [33:45] |
| Client streaming | client gửi nhiều, ít dùng |
| Bidirectional | chat, trading, game. Khác WebSocket ở chỗ gắn với một RPC do client mở |

---

## 4. Service Discovery

- **Không hardcode IP.** Pod sinh rồi chết liên tục.
- Instance tự đăng ký IP:port kèm heartbeat lên registry (Consul, etcd); client hỏi registry rồi gọi.
- Trên K8s, dev chỉ cần thống nhất service name và namespace với DevOps, nhưng **vẫn phải hiểu bản chất**.

---

## 5. Load balancing: bẫy sticky của gRPC [46:00–55:00]

- REST (HTTP/1.1) thường mở kết nối mới ⇒ LB tầng **L4** chia đều được.
- gRPC (HTTP/2) giữ **một** kết nối TCP rất lâu, mỗi RPC là một stream bên trong ⇒ LB L4 chỉ chọn pod lúc mở kết nối ⇒ **mọi request dồn vào một pod**. Scale 100 pod mà 1 pod gánh hết.
- HTTP/2 giữ kết nối lâu là để tránh tốn bắt tay (và TLS). Chính tối ưu đó làm LB L4 vô dụng.
- **Chữa:** LB **L7** chia theo từng stream, hoặc **Service Mesh** có proxy sidecar (Envoy) giữ sẵn kết nối tới mọi pod (Istio, Linkerd).
- **Tự kiểm:** load test + log tên pod xem có chia đều không. [56:00]

---

## 6. Bảo mật [57:30–61:30]

- "gRPC an toàn vì là binary" là **sai**: luật encode Protobuf công khai, ai bắt được gói tin cũng giải ngược được.
- Mạng nội bộ cũng không an toàn: package bên thứ ba có backdoor, máy DevOps nhiễm spyware đều nghe lén được. **Phải dùng TLS.**

---

## 7. Demo Wireshark

- Interface "Adapter for loopback traffic capture", filter `tcp.port == 32000`, chuột phải *Follow → TCP Stream*.
- **REST:** thấy SYN, SYN-ACK, ACK, GET, 200 OK; header và JSON hiện nguyên văn. Gửi lặp mỗi 5 giây chỉ có **một** lần SYN, vì HTTP/1.1 bật keep-alive mặc định. Nhưng header vẫn lặp nguyên ở mỗi request.
- **gRPC:** tên và email **vẫn đọc được bằng mắt** (demo dùng `insecure`, không TLS). Nhiều request chỉ một lần bắt tay, header được nén (HPACK).

---

## 8. Hỏi–đáp [87:30–101:30]

- **Scale dọc hay ngang?** Dọc sẽ chạm trần vật lý; với DB thì nghẽn disk I/O trước (100 câu SELECT cùng lúc phải xếp hàng). Ngang: 1 node 100.000 rps thì 2 node 200.000 rps.
- **Mỗi service một DB thì đồng bộ sao?**
  - Tách DB để gỡ nghẽn I/O. Đổi lại không `JOIN` được, phải chịu eventual consistency.
  - Dữ liệu tài chính, kho hàng thì đồng bộ nghiêm ngặt hoặc gọi API trực tiếp sang service kia.
  - Startup cứ dùng chung DB trước. Đã tách DB là mất ACID, phải tự thiết kế compensating transaction.
- **Cascading failure?** Giảm phụ thuộc bằng graceful degradation: cổng thanh toán chết thì Order vẫn ghi đơn ở trạng thái "Chờ thanh toán". Phần "debug thế nào" **không có câu trả lời** trong bản ghi.
- **DDD / Saga:** hẹn buổi chuyên đề riêng.

---

## 9. ⚠️ Chỗ bản ghi chưa chắc

- Tên người bị bản ghi ghi nhiều kiểu khác nhau.
- Hex dump demo `08 7b 12 05 41 6c 61 73 20 53 74 61 72 …`: byte độ dài ghi `05` nhưng chuỗi theo sau "Alas Star" dài **9** ký tự ⇒ có lẽ Gemini chép nhầm `05` từ slide Alice. Byte độ dài của email `0x14` = 20 thì đúng.
- JSON demo `{"id":123,"name":"Alas Star","email":"alasstar@example.com","active":true}` dài **74** ký tự, Wireshark báo **78** ⇒ tên hoặc email có thể bị đọc sai.
- Các chỗ nhắc mTLS, Least Privilege, client-side LB, "Stubby" là **Gemini tự thêm**, không phải lời diễn giả.
- Câu "giảm 75%" trong một bản tóm tắt cũ **không có trong video**.

💡 Độ dài 9, 20, 74 ở trên đã đếm lại ngày 28/10/2026 bằng Python.
