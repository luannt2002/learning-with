# Quân IT — mindset

> Ghi chép cá nhân khi học từ kênh YouTube [Quân IT](https://www.youtube.com/@quanit6428). **Không phải tài liệu chính thức** của tác giả. Câu trích ngắn để học tập, kèm link video (video có bản ghi thì ghi cả mốc giờ); chỗ nào sai là của người ghi.

Mindset từ kênh QuânIT, viết lại cho dễ ôn:

- **Đưa AI vào doanh nghiệp:** ba video. Video A đối chiếu được với phụ đề tự động; video B và C chỉ có bản tóm tắt.
- **Backend chịu tải cao bằng Go:** hai buổi livestream (P1 hạ tầng, P2 gRPC), chỉ có bản ghi Gemini.
- **Nghề engineer thời AI:** video 28/10/2026, chỉ có bản ghi Gemini.

## Đọc nhãn trước khi trích

| Nhãn | Nghĩa | Được nói "QuânIT nói…"? |
|---|---|---|
| 🎙 | Nguyên văn video A, khớp từng chữ với phụ đề tự động của [video A](https://www.youtube.com/watch?v=RNU7WCBjyT0) | Được |
| 📝 | Ý video B / C, đi qua bản tóm tắt của ChatGPT | Không |
| 💡 | Mình rút ra, hoặc ChatGPT hệ thống hoá | Không |
| 🎬 | Ý trong các video chỉ có bản ghi Gemini (buổi Go P1 / P2, video 28/10); không phải nguyên văn | Không |

## Mục lục

| # | Tệp | Đọc để biết |
|---|---|---|
| 00 | [Nguồn và cách đọc](docs/00-nguon-va-cach-doc.md) | ba video, nguồn tới đâu, cách kiểm câu trích |
| 01 | [AI chỉ là một tech stack](docs/01-ai-chi-la-mot-tech-stack.md) | luận điểm gốc: đừng thần thánh hoá, hype ≠ giá trị, demo ≠ doanh nghiệp |
| 02 | [Bốn vấn đề lõi](docs/02-bon-van-de-loi.md) | rác vào rác ra · không tất định · bịa · benchmark |
| 03 | [Phần mềm quanh model](docs/03-phan-mem-quanh-model.md) | router, context builder, đọc dữ liệu, RAG vs fine-tune, semantic search, MCP, train |
| 04 | [Use case](docs/04-use-case.md) | ⭐ 4k lít dầu qua Zalo, hỏi doanh thu 100 lần, tổng đài đặt vé, CEO uỷ quyền, 99% |
| 05 | [Rủi ro đầu tư AI](docs/05-rui-ro-dau-tu-ai.md) | video C: phong trào, MCP đổi, hãng ngừng model, model mạnh hơn làm hỏng, vendor rút |
| 06 | [Nguyên tắc thiết kế](docs/06-nguyen-tac-thiet-ke.md) | năm vai, bảy bất biến, trạng thái dữ liệu / hành động, phê duyệt gắn băm, 12 nguyên tắc |
| 07 | [Jev: "deterministic AI"](docs/07-jev-deterministic-ai.md) | "Jeff" trong video là gì, tất định thật nằm ở đâu, kết quả thử thật |
| 08 | [Cổng 9 câu + kinh nghiệm nghề](docs/08-cong-9-cau-va-kinh-nghiem-nghe.md) | hỏi gì trước khi đưa AI vào một việc; dám nói "không" |
| 09 | [Ý không phải của QuânIT](docs/09-y-khong-phai-cua-quanit.md) | câu hay bị gán nhầm, kèm số đếm trong phụ đề video A |
| 10 | [Tự kiểm](docs/10-tu-kiem.md) | 34 câu ôn có đáp án (AI + Go + nghề) |
| | **Backend Go / microservice** | |
| 11 | [Mindset backend chịu tải cao](docs/11-go-microservice-mindset.md) | host nói về AI khi code, bức tranh chung, 25 điều cốt lõi, chỗ video nói chưa chuẩn |
| 12 | [P1: hạ tầng một service](docs/12-go-p1-ha-tang-service.md) | Vault, ELK, APM, MongoDB, Kafka, framework nội bộ, hỏi–đáp |
| 13 | [P2: gRPC](docs/13-go-p2-grpc.md) | Monolith vs Microservices, Protobuf tới tầng byte, Service Discovery, LB sticky, TLS |
| | **Nghề engineer thời AI** | |
| 14 | [Tương lai engineer thời AI](docs/14-tuong-lai-engineer-thoi-ai.md) | video 28/10: nút thắt dời Coding → Review → Test, "What is good?", đừng "one-shot", 3 lời khuyên |

Đọc nhanh 15 phút: 01 → 02 → 03 → 04 §1 (AI), hoặc 11 (Go). Đọc đủ: theo thứ tự.

## Một trang nhớ cả bài

```text
AI chỉ là MỘT công nghệ  ──►  model nằm Ở GIỮA, phần lớn việc là phần mềm thường quanh nó
        │
        ├─ Demo ≠ doanh nghiệp: doanh nghiệp = tiền + vận hành ⇒ critical
        │
        ├─ 4 vấn đề lõi:  rác vào rác ra · không tất định · bịa · đánh giá bằng benchmark
        │
        ├─ Đồ nghề quanh model: khảo sát + phân loại dữ liệu → đọc / chuẩn hoá → router / classifier
        │                        → context builder → (RAG / fine-tune / tool qua MCP) → kiểm
        │
        ├─ Sống lâu (video C): đổi model, đổi hãng, đổi giao thức mà không mất đúng
        │
        ├─ Câu hỏi nghề: sai thì mất gì? ai chịu trách nhiệm? phần nào CHẲNG CẦN AI?
        │
        ├─ Backend Go: đừng microservice theo trend · quan sát được (log cấu trúc, APM) · Kafka chia theo partition + key
        │              · gRPC cần LB L7 + TLS · thêm máy thì nghẽn dời sang đâu?
        │
        └─ Nghề thời AI: AI gỡ nút Coding ⇒ nút thắt dời sang Review rồi Test · đừng "one-shot"
                         · "What is good?" = phù hợp nhất với ngữ cảnh + chi phí · tò mò, cởi mở, công khai quá trình
```

Ba câu thuộc lòng:

- 🎙 "ai chỉ là một công nghệ thôi"
- 🎙 "cái AI model nó chỉ nằm ở giữa thôi"
- 🎙 "có khi một số cái chẳng cần ai"

## Kiểm câu trích

Câu 🎙 nào cũng khớp từng chữ với phụ đề tự động của video A. Cách kiểm: [00 — Nguồn và cách đọc](docs/00-nguon-va-cach-doc.md).

## Cấu trúc

```text
README.md                 mục lục (tệp này)
docs/
  00-…10-*.md             mindset AI trong doanh nghiệp, mỗi tệp một chủ đề
  11-…13-*.md             backend Go / microservice (P1, P2)
  14-*.md                 nghề engineer thời AI (video 28/10)
```
