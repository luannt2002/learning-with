# 11 — Backend chịu tải cao bằng Go: mindset từ 2 buổi livestream

> Hai buổi trên kênh Quân IT về microservice bằng Go. Chi tiết từng buổi: [12 (P1, hạ tầng một service)](12-go-p1-ha-tang-service.md) · [13 (P2, gRPC)](13-go-p2-grpc.md).
>
> | Nhãn | Nghĩa |
> |---|---|
> | 🎬 | ý trong video, lấy qua **bản ghi Gemini xem video ở độ phân giải thấp**. Không phải nguyên văn, **không kiểm được từng chữ**. Mốc `[MM:SS]` là mốc trong video gốc; muốn chắc thì mở YouTube đúng mốc nghe lại |
> | ⚠️ | chỗ người nói chưa chuẩn, hoặc bản ghi chưa chắc |
> | 💡 | ghi chú thêm, **không phải lời người trong video** |

---

## 0. Nguồn

| | Video | Dài | Nội dung |
|---|---|---|---|
| **P1** | [Micro services backend for highload system](https://www.youtube.com/watch?v=6ZxD6jKHOyM) | 1:30:45 | framework nội bộ: Vault, ELK, APM, MongoDB, Kafka, Fiber |
| **P2** | [Golang Micro service for highload backend Part2: GRPC](https://www.youtube.com/watch?v=VZoy3y3P6qY) | 1:42:45 | Monolith vs Microservices, gRPC / Protobuf tới tầng byte, Service Discovery, Load Balancing, bảo mật, demo Wireshark |

- **Người nói:** phần kỹ thuật do diễn giả khách mời (anh Sơn) trình bày ⇒ đừng viết "QuânIT nói" cho phần kỹ thuật, viết "buổi P1 / P2 trình bày".
- **P1 chỉ tính từ [19:00]**: trước đó là chờ khán giả và setup. Riêng [16:00–18:00] về con trỏ thì giữ (xem [12](12-go-p1-ha-tang-service.md) §0).

---

## 1. Host nói về AI khi lập trình (P2 [02:00–05:30]) 🎬

Đoạn này nối thẳng với video A ("AI chỉ là một công nghệ").

- Host đã dựng cả "hệ sinh thái" cho AI: skills, tệp `claude.md`, prompt đủ kiểu.
- Hệ phức tạp, AI sinh code lỗi: prompt qua lại để sửa có thể mất **~30 phút** [03:26]. Người nắm bản chất tự sửa **~5 phút** [03:30].
- Gặp bug sâu hoặc AI tính sai logic mà mình không hiểu nền tảng để debug thì dự án "toang" [04:00].
- Host kể có bài tuyển Senior Dev **chỉ để sửa lỗi** do người "vibe coding" để lại, lương 3.000–3.500 USD/tháng [04:25–04:32].
- Kết: kiến thức cốt lõi về cấu trúc hệ thống là bắt buộc, không phó mặc cho AI. Đó là lý do có buổi chia sẻ.

💡 Cùng tinh thần với 🎙 "anh em đừng có nghĩ tới cái chuyện mà không đụng tay vô" ở video A ([01](01-ai-chi-la-mot-tech-stack.md) §4).

---

## 2. Bức tranh chung

```text
                 REST (public)                       gRPC (nội bộ, HTTP/2 + Protobuf)
 client ───────────────────────► API Go (Fiber) ◄──────────────────────────► service khác
                                   │   │                                   (tìm nhau: Service Discovery / DNS của K8s)
                     publish event │   │ log (Zap)                         (chia tải: L7 / Service Mesh)
                                   ▼   ▼
                              ┌──── Kafka ─────┐
                              │ topic/partition │──► worker (consumer, TIẾN TRÌNH RIÊNG, scale theo partition)
                              └───────┬────────┘
                                      ▼
                         Logstash → Elasticsearch → Kibana   (lọc log theo service.name, level)
 Cấu hình + secret: Vault (thay .env)     Đo từng request: Elastic APM (trace, span, dependency)
 DB: MongoDB (đổi được sang Postgres / MySQL)
```

- **P1:** gói thư viện mã nguồn mở thành **framework nội bộ**, để startup chạy rẻ mà vẫn **quan sát** được hệ: log có cấu trúc, APM, Kafka làm bộ đệm.
- **P2:** tách service rồi thì **cách chúng nói chuyện** quyết định hiệu năng. gRPC thắng REST ở tầng byte và tầng kết nối, nhưng kéo theo bài toán chia tải và **không tự an toàn**.

---

## 3. 25 điều cốt lõi 🎬

**Hạ tầng và quan sát (P1)**

1. **Vault thay `.env`:** cấu hình tập trung, dev tự sửa, có xoay mật khẩu DB tự động. Prod dùng cụm Vault riêng. [P1 21:00–23:00, 33:05]
2. **Thiếu key cấu hình bắt buộc thì `panic` ngay lúc khởi động.** Thà không chạy còn hơn chạy sai mà không báo. [P1 32:25]
3. **Log có cấu trúc** (JSON): `message, host (tên pod), service, level, key (tên hành động), data, trace.id`. Không log chuỗi thô. [P1 39:00–41:00]
4. **Kafka đứng trước Elasticsearch làm bộ đệm**, để log tăng vọt không làm sập ES / Logstash. [P1 22:45–24:00, 47:00]
5. **Kibana:** dùng *Add filter* (`service.name`, `level: error`) và thu hẹp khung thời gian. Full-text search chậm khi có hàng triệu dòng. [P1 43:15]
6. **APM** cho biết request tốn thời gian ở bước nào, có số để phân xử Dev với DevOps. Luôn chọn đúng *environment*. APM backend **không đo** đoạn mạng về client. [P1 51:00–66:00]
7. DB phổ biến được APM tự theo dõi qua plugin. Logic riêng thì **tự tạo transaction / span**. [P1 60:00–63:00]

**Kafka (P1)**

8. **Partition quyết định mức song song.** Topic chỉ 1 partition thì không khác một hàng đợi. [P1 70:13]
9. **Số consumer nên bằng số partition:** thừa thì có con ngồi không, thiếu thì có con gánh 2 partition. [P1 74:50–75:50]
10. **Không có key thì chia round-robin. Có key thì cùng key vào cùng partition, giữ được thứ tự.** Tiền, tồn kho, trạng thái đơn bắt buộc có key, ví dụ Product ID. [P1 76:00–80:45]
11. **Kafka không phải DB:** luôn đặt retention. [P1 68:10]
12. **API chỉ publish; consumer là tiến trình nền riêng**, scale độc lập với API. [P1 35:00]
13. Chống race (trừ tồn kho) **không giao hết cho Kafka**: tầng DB vẫn phải có điều kiện cập nhật hoặc lock. [P1 87:40]

**Kiến trúc (P2)**

14. **Đừng chọn Microservices theo trend.** Team nhỏ hoặc chưa có product-market fit thì **Monolith / Modular Monolith**. [P2 16:00]
15. Microservices có giá: lỗi mạng, retry, timeout, tracing, **mất ACID đơn giản** (phải dùng Saga / event-driven). [P2 14:30–15:30]
16. **Database-per-service** để gỡ nghẽn I/O. Đổi lại không `JOIN` được, phải chịu eventual consistency. Startup dùng chung DB tới khi nghiệp vụ ổn định. [P2 92:13–98:58]
17. **Scale ngang thắng scale dọc.** Nâng dọc thì DB nghẽn ở disk I/O trước cả CPU / RAM. [P2 88:00–91:15]
18. **Graceful degradation:** Order chết thì Product vẫn phục vụ. Tách service theo nghiệp vụ để khoanh vùng thiệt hại. [P2 99:45–101:30]

**gRPC (P2)**

19. JSON nặng **không phải vì là text**, mà vì **bản tin nào cũng mang tên field**. Protobuf chỉ gửi số thứ tự field, wire type và giá trị. [P2 22:00–30:20]
20. **Tag = `(field_number << 3) | wire_type`.** `id=123` → `08 7b`, `name="Alice"` → `12 05 41 6c 69 63 65`. [P2 26:00–29:20]
21. **4 kiểu RPC:** Unary (phổ biến nhất) · Server streaming (trả 900.000 bản ghi theo lô) · Client streaming (ít dùng) · Bidirectional (chat, trading, game). [P2 31:53–38:52]
22. **Không hardcode IP.** Service Discovery (Consul, etcd) trả lời "service X ở đâu, mấy instance". Nay K8s lo phần này. [P2 38:53–45:45]
23. **gRPC + LB tầng L4 = sticky:** scale 100 pod mà 1 pod gánh hết, vì HTTP/2 giữ một kết nối lâu. Cần **LB L7 hoặc Service Mesh**. **Tự kiểm** bằng load test + log tên pod. [P2 45:46–57:30]
24. **Binary không có nghĩa là an toàn.** Không TLS thì Wireshark vẫn đọc được string trong gRPC. Đừng chủ quan với mạng nội bộ. [P2 57:30–61:30, 77:30]
25. HTTP/1.1 có keep-alive (bắt tay một lần) nhưng **header lặp ở mọi request**. HTTP/2 nén header và multiplex nhiều stream trên một kết nối. [P2 71:00–82:30]

---

## 4. Bốn câu nên tự hỏi khi xem lại kiến trúc này 💡

1. Thêm pod, worker hay partition thì **chỗ nghẽn dời sang đâu**? (DB disk I/O ở P2 [88:48]; partition ở P1 [74:55].)
2. **Trần cứng** là gì? Số partition, kết nối HTTP/2 sống lâu, disk I/O của DB.
3. Chỗ nào **mất dữ liệu hoặc chạy sai mà không báo lỗi**? Log bị nuốt lỗi, thiếu cấu hình mà vẫn chạy, gửi trùng message.
4. **Đo bằng gì** để biết hệ chạy đúng? Kibana (log), APM (độ trễ, span), log tên pod khi load test (LB).

---

## 5. ⚠️ Chỗ video nói chưa chuẩn, hiểu cho đúng

| Mốc | Video nói | Hiểu đúng 💡 |
|---|---|---|
| P1 25:30 | Kafka "vượt bậc" so với RabbitMQ / Redis vì hàng đợi cũ chỉ xử lý tuần tự | RabbitMQ cũng cho nhiều worker đọc song song. Khác thật: **hàng đợi** xử lý xong là xoá tin, mỗi tin tới một worker; **log (Kafka)** giữ tin lại, nhiều nhóm đọc độc lập và **đọc lại được** |
| P1 26:00 | "Tải tăng thì thêm worker, ví dụ 100 worker" | Có **trần cứng = số partition** (chính video nói đúng ở [74:55]). Nút thắt còn có thể **dời sang DB** mà worker ghi vào |
| P1 70–80 | Tăng partition để scale | Tăng partition **đổi việc key nào rơi vào partition nào**, thứ tự theo key bị xáo lúc chuyển ⇒ chọn số partition ngay từ đầu. Key quá nóng (flash sale) gây **hot partition** |
| P1 25:00 | "Đổi Mongo sang Postgres / MySQL dễ" | Tách tầng truy cập DB thì đúng, nhưng transaction, query, độ nhất quán mỗi loại một khác, không chỉ là đổi driver |
| P1 27:00 | Một tiến trình đọc DB rồi publish, worker consume | Hợp cho xử lý theo lô, nhưng phải lo checkpoint, đọc trùng. Muốn *phát sự kiện khi dữ liệu đổi* thì đúng cách là **Transactional Outbox / CDC** |
| P1 28:00 | Fiber là thư viện "nhanh nhất" | Nút thắt hiếm khi ở router. Fiber không theo chuẩn `net/http` nên hệ sinh thái hẹp hơn; "nhanh nhất" là khẩu hiệu, đo rồi mới chọn |
| P1 (log) | Log đi qua Kafka làm bộ đệm | Ý đúng. Nhưng nếu app gửi log **đồng bộ** thì Kafka chậm kéo API chậm theo. Nên ghi log ra stdout rồi để một agent thu gom đẩy sang Kafka |
| P2 74:30 | Đoán HTTP/1.1 bắt tay ở mỗi request | HTTP/1.1 bật keep-alive mặc định, client cũng giữ kết nối ⇒ chỉ bắt tay một lần. Diễn giả hỏi ChatGPT mới ra; 💡 đoán xong phải đo (Wireshark) như chính buổi đó làm |

**Video nói đúng và nên nhớ:** Kafka không phải DB · message key để giữ thứ tự · không giao hết việc chống race cho Kafka · thiếu cấu hình thì fail-fast · binary không phải bảo mật · LB cho gRPC phải ở L7 · startup dùng Monolith trước · JSON nặng vì mang tên field.

---

## 6. Nối với mindset của video A 💡

| Video A (AI) | Buổi Go / microservice |
|---|---|
| 🎙 "có khi một số cái chẳng cần ai" | đừng chọn Microservices theo trend; startup dùng Monolith trước |
| Hype bán hàng ≠ giá trị | "nhanh nhất" (Fiber), "vượt bậc" (Kafka) cũng là khẩu hiệu; nút thắt thật thường ở chỗ khác |
| Rác vào rác ra → chuẩn hoá trước | thiếu cấu hình thì `panic`; log có cấu trúc chứ không chuỗi thô |
| "Ai chịu trách nhiệm" / SLA | APM có số để phân xử Dev với DevOps, không cãi bằng cảm giác |
| Không tất định → kéo về hệ quy chiếu | message key giữ thứ tự; DB vẫn kiểm điều kiện, không phó mặc Kafka |
| Thử trước, tin sau | tự kiểm LB bằng load test + log tên pod; tự bắt gói tin bằng Wireshark |
