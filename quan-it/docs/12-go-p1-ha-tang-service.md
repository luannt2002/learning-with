# 12 — P1: hạ tầng cho một service Go (Vault, ELK, APM, MongoDB, Kafka)

> Video [Micro services backend for highload system](https://www.youtube.com/watch?v=6ZxD6jKHOyM) (1:30:45). Cả tệp là 🎬 (bản ghi Gemini), trừ chỗ ghi 💡 / ⚠️. Nhãn xem [11](11-go-microservice-mindset.md).

---

## 0. Ý giữ lại từ đoạn mở đầu [16:00–18:00]

- Host chia ngôn ngữ làm 3 nhóm: cấp thấp (C / C++, tự quản con trỏ và cấp phát) · cấp cao có GC (C#, Java, JS, Python) · hiện đại / lai (Go, Rust).
- **"1 + 1 không bằng 2":** đọc từ một **bản sao** chưa đồng bộ trong khi **ô nhớ gốc** đã đổi, nên ra kết quả cũ.
- 💡 Cái bị cũ là **bản sao**, không phải **con trỏ**. Con trỏ giữ địa chỉ ô nhớ gốc nên luôn đọc ra giá trị mới, trừ khi có data race giữa các goroutine.

---

## 1. Dàn ý

| Mốc | Nội dung |
|---|---|
| 19:00–21:00 | anh Sơn giới thiệu framework nội bộ, 5 thành phần: Vault, ELK, APM, MongoDB, Kafka |
| 21:00–23:00 | Vault thay `.env`, xoay mật khẩu tự động |
| 22:30–25:00 | log đi Zap → Kafka → ELK; vì sao cần APM |
| 25:00–27:15 | MongoDB; Kafka so với queue truyền thống; mẫu "một tiến trình đọc DB rồi publish, worker consume" |
| 27:15–32:00 | bố cục một module: `dto / handler / routes / service` |
| 32:00–36:30 | thiếu cấu hình thì dừng ngay lúc khởi động; Vault UI; vì sao API chỉ có `pub` |
| 36:30–41:00 | bật / tắt từng thành phần bằng cờ; log có cấu trúc |
| 41:00–51:00 | demo Kibana: lọc ra lỗi "topic chưa tồn tại"; vai trò từng phần ELK |
| 51:00–66:00 | demo APM: Services, Latency, Dependencies, trace waterfall, span tự tạo |
| 66:00–76:00 | Kafka: topic, partition, consumer group, retention, round-robin, consumer so với partition |
| 76:00–82:00 | message key giữ thứ tự; ví dụ trừ tồn kho |
| 82:00–89:30 | hỏi–đáp: Mongo hay Postgres? Kafka cho payment / inventory? module giao tiếp thế nào? |

---

## 2. Kiến thức chính

**Mục tiêu framework:**

- Người mới dựng service trên khung có sẵn ⇒ code style và bố cục đồng nhất.
- `framework/` bọc sẵn `apm, db, encrypt, kafka, log, pubsub, server, utils`. Dev chỉ bật cờ rồi gọi, không lo kết nối hay connection pool.

**Vault:**

- Mỗi môi trường (dev / testnet / staging) là một secret engine riêng. Prod nên là cụm riêng.
- Path theo service, ví dụ `http/<service>/{apm, config, db, pub}`. Secret bản chất là key–value.
- Thay thế được bằng Consul, etcd.

**ELK:**

- Elasticsearch: lưu và đánh index (dùng được cho full-text search).
- Logstash: thu gom, biến đổi log; **đọc từ Kafka** rồi nạp vào ES.
- Kibana: chỉ là giao diện.
- Hệ chạy ổn thì rất ít log lỗi ⇒ khi có sự cố chỉ cần lọc `error`.
- Ví dụ thật [44:00]: lọc `level: error` ra đúng 1 lỗi `Request was for a topic or partition that does not exist on the broker`. Biết ngay là **topic chưa được tạo**, không phải bug trong code Go.

**APM [51:00–66:00]:**

- Throughput tính theo tpm. Tăng vọt lên hàng nghìn có thể là bị DDoS.
- Dependency `mongodb` 66 ms là chấp nhận được; thấy 10 giây thì thiếu index hoặc DB có vấn đề.
- Trace `POST /hello` tổng 2.092 ms: Mongo 49 ms, span `DoSomeThing` 2.000 ms ⇒ thủ phạm là `DoSomeThing`.
- Một trace lạ ~6 giây: publish Kafka nghẽn vì máy test đi qua Internet.

**MongoDB hay SQL [82:45–86:10]:**

- SQL bắt định nghĩa schema từ đầu, mỗi lần đổi chạy migration. Mongo linh hoạt, hợp startup.
- Nhưng ACID xuyên nhiều collection còn hạn chế. Ngân hàng / kế toán thì dùng SQL.

**Cấu trúc module:**

- `handler` giống Controller, nhận Fiber ctx · `service` chứa nghiệp vụ · `dto` là struct truyền giữa các tầng.
- Tách handler khỏi service để sau này đổi REST sang gRPC chỉ sửa handler.
- Microservice là chia hệ để quản lý, scale, deploy độc lập, **không phụ thuộc** REST hay gRPC.
- Chạy chung một process (modular monolith) thì import package gọi thẳng. Tách service riêng thì gọi HTTP hoặc gRPC, nên ưu tiên gRPC [88:50].

**Video không demo deploy.** Chỉ nhắc K8s pod, Dockerfile, docker-compose.

---

## 3. Hỏi–đáp

- **Vì sao không đẩy log thẳng vào ES?** Log tăng đột biến làm sập ES. Kafka giữ log làm bộ đệm để Logstash rút về theo sức của nó. [23:00, 47:25]
- **Vì sao API chỉ có `pub`, không có `sub`?** API theo mô hình request / response; consumer là tiến trình nền chạy liên tục. Gộp chung thì khó scale riêng. [35:05]
- **Sleep 1 giây mà span đo 2 giây, có lúc 6 giây?** Span bao cả bước publish Kafka. Mạng hoặc broker chậm thì span dài theo. [62:00–64:30]
- **Mongo hay Postgres?** Mongo linh hoạt schema, hợp startup. Postgres hợp nghiệp vụ đã ổn định, cần ACID trên nhiều bảng. [82:45]
- **Kafka cho payment / inventory?** Hệ nhỏ chỉ cần DB transaction + row lock. Hệ lớn thì Kafka với key = Product ID, và **tầng DB vẫn phải kiểm điều kiện**. [86:17–88:30]
