# 00 — Nguồn và cách đọc

## Ba video, nguồn tới đâu

| Video | Chủ đề | Nguồn có trong tay | Mức tin |
|---|---|---|---|
| **A** — ["AI is Just Another Tech Stack \| AI In Enterprise"](https://www.youtube.com/watch?v=RNU7WCBjyT0) | AI chỉ là một tech stack, 4 vấn đề lõi, báo cáo dầu qua Zalo, YOLO | phụ đề tự động của video (người viết lưu bản riêng, không đăng ở đây) | 🎙 |
| **B** — "AI phù hợp use case nào?" | tổng đài đặt vé, CEO uỷ quyền, portal cho công nhân, "99% chính xác" | chỉ có bản tóm tắt của ChatGPT, không có transcript | 📝 |
| **C** — Rủi ro triển khai AI cho doanh nghiệp | đầu tư theo phong trào, MCP đổi, nhà cung cấp ngừng model, đơn vị triển khai rút | chỉ có bản tóm tắt của ChatGPT, không có transcript | 📝 |

Thêm hai buổi livestream kỹ thuật trên cùng kênh, về backend Go:

| Video | Chủ đề | Nguồn có trong tay | Mức tin |
|---|---|---|---|
| **P1** — "Micro services backend for highload system" | Vault, ELK, APM, MongoDB, Kafka, framework Go nội bộ | bản ghi Gemini xem video ở độ phân giải thấp | 🎬 |
| **P2** — "Golang Micro service for highload backend Part2: GRPC" | Monolith vs Microservices, gRPC / Protobuf, Service Discovery, load balancing, bảo mật | bản ghi Gemini xem video ở độ phân giải thấp | 🎬 |

Hai buổi này người trình bày là anh Sơn, không phải QuânIT của video A.

Video thứ sáu, do chính chủ kênh nói:

| Video | Chủ đề | Nguồn có trong tay | Mức tin |
|---|---|---|---|
| **"Tương Lai Engineer Thời AI \| Lời Khuyên Thật Lòng"** (28/10/2026, 61:58) | nút thắt dời Coding → Review → Test, "What is good?", lời khuyên cho engineer | bản ghi Gemini | 🎬 |

Chi tiết: [14-tuong-lai-engineer-thoi-ai.md](14-tuong-lai-engineer-thoi-ai.md). Chi tiết nguồn: [11-go-microservice-mindset.md](11-go-microservice-mindset.md) §0.

Bản tóm tắt video B, C là ghi chú riêng của người viết, không đăng ở đây.

---

## Ba nhãn: nhìn nhãn trước khi trích

| Nhãn | Nghĩa | Được nói "QuânIT nói…"? |
|---|---|---|
| 🎙 | Nguyên văn video A. Câu trong ngoặc kép khớp từng chữ với phụ đề tự động của video | Được |
| 📝 | Ý của video B / C, đi qua bản tóm tắt của ChatGPT | Không. Chỉ được nói "video B/C (qua tóm tắt) có ý…" |
| 💡 | Mình tự rút ra, hoặc ChatGPT hệ thống hoá lại | Không |
| 🎬 | Ý trong các video chỉ có bản ghi Gemini (buổi Go P1, P2 và video 28/10). Không phải nguyên văn, không kiểm được từng chữ | Không. Viết "buổi P1 / P2 trình bày…" hoặc "theo bản ghi, ông cho rằng…" |

Chữ trong « » là nguyên văn **bản tóm tắt ChatGPT**, không phải lời QuânIT.

Câu nào đẹp mà không có nhãn 🎙 thì đừng gán cho QuânIT. Danh sách các câu hay bị gán nhầm: [09-y-khong-phai-cua-quanit.md](09-y-khong-phai-cua-quanit.md).

---

## Kiểm câu trích

Mọi câu 🎙 đã được đối chiếu từng chữ với phụ đề tự động của video A (lần cuối 28/10/2026: 144 mảnh, 0 mảnh thiếu). Muốn tự kiểm: mở [video A](https://www.youtube.com/watch?v=RNU7WCBjyT0), bật phụ đề tự động, tìm câu. Câu dài có dấu `…` thì tìm từng mảnh.

Câu trích giữ nguyên lỗi chính tả của phụ đề tự động (`noneterministic`, `tocen`, `lí dầu`, `identic router`).

---

## Chuyện đã gặp khi viết

- Câu nổi tiếng "deterministic shell quanh probabilistic core" **không có** trong video A (tìm trong phụ đề ra 0), cũng không có trong hai bản tóm tắt B/C. Nó là tên một mục ("Phase 7") trong bản Study Guide video A do ChatGPT viết.
- "AI is Just Another Tech Stack" chỉ là **tên video**. Trong lời nói không có cụm "just another".
- "Jeff" trong phụ đề thật ra là **Jev** (phụ đề nghe nhầm). Xem [07-jev-deterministic-ai.md](07-jev-deterministic-ai.md).
- Các số 5%, 90%, 95%, 100% là số nói miệng, chưa kiểm nguồn. Dùng để hiểu ý, **không dùng làm ngưỡng**.
