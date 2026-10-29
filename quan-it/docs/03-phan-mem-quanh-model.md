# 03 — Phần mềm quanh model (phần "chẳng liên quan gì tới AI")

> Video A. Ý chính: model chỉ là một mảnh nhỏ ở giữa. Phần lớn việc là kiến trúc và lập trình thường.

- 🎙 "cái AI model nó chỉ nằm ở giữa thôi"
- 🎙 "nguyên một cái stack AI á là để nó hỗ trợ cho cái chuyện đó thôi"
- Nhiều thứ trong đó 🎙 "nó chả liên quan mẹ gì tới AI hết"
- Thậm chí 🎙 "có khi một số cái chẳng cần ai"

```text
Dữ liệu doanh nghiệp ─► phân loại từng dạng ─► đọc / chuẩn hoá (data digestion)
                                                    │
Câu hỏi ─► classifier / router ─┬─► tool tất định (SQL, report builder) ─► số
                                └─► context builder: gom A B C D E ─► model ─► câu trả lời
                                         │
                                         └─ thiếu / mâu thuẫn / chưa rõ ─► hỏi lại (clarification)
```

💡 Sơ đồ do mình vẽ lại từ lời video, không phải hình của QuânIT.

### Slide "Satellite systems"

Ngay sau câu 🎙 "cái AI model nó chỉ nằm ở giữa thôi", QuânIT nói 🎙 "nó là dạng vậy nè", tức đang chỉ vào một hình. Hình đó có lẽ là slide dưới đây.

📝 **Nguồn:** ChatGPT tả lại ảnh chụp slide (ghi chú riêng của người viết). Ảnh gốc không còn, và chưa chắc 100% slide thuộc video A.

```text
                RAG
                 │
       MCPs ── AI MODEL ── Router
                / \
           Tools   Skills

       + Recursive training
```

- Slide gọi các thứ quanh model là **satellite systems**: RAG, Router, Skills, Tools, MCPs. Chúng « extend what the model can do ».
- Dòng dưới: « Recursive training — The model at the core keeps improving. »

💡 Cách hiểu cho đúng:

- "Satellite systems" là **hình dung** cho dễ nhớ, không phải chuẩn kiến trúc. Ngoài đời các vệ tinh dính nhau (router chọn RAG hay tool; RAG cần context builder; tool cần kiểm quyền), không tròn đều như hình.
- "Recursive training" **không có nghĩa** là model tự sửa trọng số mỗi lần dùng. Nghĩa thật: model lõi được cải thiện qua các vòng train / fine-tune có kiểm soát. Gần với 🎙 "từ cái base đó mình sẽ phát triển lên thành một cái chuyên gia nào đó bằng cái dữ liệu của riêng mình" ([08](08-cong-9-cau-va-kinh-nghiem-nghe.md) §2).

---

## 1. Classifier / router: phần lớn câu hỏi không cần model

- Ví dụ "làm cho tao cái báo cáo": 🎙 "thì cái classifier nó phải tự biết là nó gọi qua một cái tool là cái cái cái report builder", rồi 🎙 "gọi một cái tool vô trong database để lấy cái dữ liệu ra"
- Dữ liệu là của khách: 🎙 "người ta phải cung cấp cho mình mình mới build được"
- Phải lo cả ca xấu: dữ liệu 🎙 "mâu thuẫn thì sao", 🎙 "thiếu khóa thiếu này thiếu kia thì sao", người hỏi chưa rõ thì phải có 🎙 "clarification question"

**Hai kiểu router:**

- 🎙 "classifier theo bình thường thôi", hoặc agentic router (phụ đề ghi "identic router").
- QuânIT so: 🎙 "cái mức độ tin cậy của nó chỉ là có 90% nhưng mà cái thằng identic router này mức độ của nó là 100%". Số nói miệng.
- Đây là stack mới: 🎙 "anh em cứ phải try and fail thôi"

💡 Router 100% chỉ đúng trên bài dễ. Xem phần thử thật ở [07-jev-deterministic-ai.md](07-jev-deterministic-ai.md).

---

## 2. Context builder: lập trình thuần, không phải AI

- 🎙 "không liên quan gì tới ai cả mà nó chỉ thuần là về architecture và lập trình thuần túy thôi"
- Phải biết lấy dữ liệu nào từ hệ doanh nghiệp, mỗi loại xử lý thế nào, đưa vào model dạng gì (🎙 "jon hay là text hay là những cái file XML").
- Mức cần đạt: 🎙 "cái tỉ lệ chính xác của nó phải là gần như là tuyệt đối"

---

## 3. Phân loại dữ liệu và đọc tài liệu khó (data digestion)

- Bước đầu: 🎙 "phải phân phân loại được categorize", rồi 🎙 "mỗi một dạng thì mày sẽ xử lý như thế nào"
- Có tool hỗ trợ (QuânIT nhắc LangChain, LlamaIndex: 🎙 "Len hay là Lama Index"), nhưng không giải hết.
- PDF in ra như bảng nhưng không kẻ cột, dòng total ở dưới. Người nhìn thì 🎙 "mặc dù là nếu mà đối với đối với con người thì rất là dễ", máy thì khó.
- Tool trả phí mạnh nhất cho dạng này: 🎙 "cái độ chính xác nó cũng chỉ 95% thôi"
- Phải đi vào từng chi tiết như vậy thì phần 🎙 "data digestion" mới đạt độ chính xác cao.

---

## 4. Fine-tune khác RAG

| | Fine-tune | RAG |
|---|---|---|
| Làm gì | 🎙 "mình sẽ trend lại trend lại một cái base model trên cái dữ liệu của người ta", chỉnh thẳng trọng số | 🎙 "mình sẽ tối ưu cái phần context", không đụng model |
| Việc của hệ | dữ liệu để train | lo gom thông tin: 🎙 "thì cái system này á nó sẽ phải có trách nhiệm" (xem 3 bước dưới) |

**RAG theo lời QuânIT là bài gom context:**

1. 🎙 "tao cần A B C D E thông tin"
2. Mỗi loại thông tin một cách lấy: 🎙 "đối với cái thông tin A tao sẽ sử dụng cái skill này" (A: sinh SQL truy vấn database; B: vào thư mục lấy PDF, đổi ra embedding; C: …)
3. 🎙 "gom hết cái đống thông tin đó mới quăng qua cho cái con model"

💡 RAG ở đây là **kỹ thuật phần mềm**: gom đúng, lọc đúng, rồi mới hỏi. Không phải "nhét tài liệu vào vector rồi cầu may".

---

## 5. Semantic search: chỗ code cũ không làm được

- if/else cũ chỉ có đúng / sai. Ca 🎙 "cái này tao có vẻ như là 95% là true thì sao thì nó là false hết"
- Tìm theo nghĩa xử lý được ca "gần đúng": 🎙 "cái semantic search này á nó xử lý được"

💡 Độ giống nhau ≠ xác suất đúng. Điểm cosine 0,95 không có nghĩa "95% là đúng".

---

## 6. MCP: để model gọi tool thay vì tự đoán

- MCP là chuẩn 🎙 "để cho các cái con AI nó có khả năng nó gọi tới các API và nó gọi tới các cái tool của nó"
- 1 + 1 bằng mấy thì 🎙 "cứ gọi cái tool calculator". Thời tiết Biên Hoà thì gọi API thời tiết.
- Phải biết từ một API dựng MCP thế nào.

💡 Cùng tinh thần với router: việc tất định (tính toán, tra cứu) giao cho tool, model không tự sinh đáp án.

---

## 7. Train và đồ nghề phải biết

- Đừng 🎙 "cái gì cũng slap yolo vô". Model ảnh tốt nhất vẫn trượt nếu không hợp hiện trường: 🎙 "mỗi cái mỗi cái mỗi cái công ty enterprise thì nó lại có một cái đặc điểm khác" (camera khác, độ phân giải, góc quay, ánh sáng).
- Phải biết tự train: Google Colab tốn bao nhiêu với dataset cỡ nào, mua GPU thì sao, điểm mạnh yếu từng đường.
- Biết thêm: skill (🎙 "anh em cũng phải biết về skill"), Roboflow để gán nhãn và quản lý model, các hướng mới (xem [08](08-cong-9-cau-va-kinh-nghiem-nghe.md) §2).
- Mục đích: để nói chuyện với khách 🎙 "nói một cách rất là tự tin và thông minh intelligently"

💡 **Model tốt ≠ hệ tốt.** Model chung phải được chỉnh theo dữ liệu và điều kiện của từng khách.
