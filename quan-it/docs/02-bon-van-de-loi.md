# 02 — Bốn vấn đề lõi của AI hiện nay

> Video A. Bốn vấn đề: rác vào rác ra · không tất định · bịa · đánh giá bằng benchmark.
> Ngay sau đoạn benchmark và bịa, QuânIT nói: 🎙 "hai cái vấn đề lớn nhất của ai hiện tại". Ngay trước đó là 🎙 "evaluation nó thông qua các cái các cái benchmark" và 🎙 "hallucination thì là một cái vấn đề lớn", nên "hai cái" là **đánh giá** và **bịa**.

---

## 1. Rác vào, rác ra → khảo sát và chuẩn hoá trước

- 🎙 "garbage in garbage out". AI còn đói dữ liệu hơn web app thường (phụ đề ghi "đối"): 🎙 "AI nó lại càng là đối data càng đối data"
- Dữ liệu doanh nghiệp thật: báo cáo tài chính mỗi bảng một số, lúc Excel lúc sheet lúc PDF lúc text, database không chuẩn, nhiều bảng rác, khoá null.
- Ảo tưởng "cứ đưa hết cho model frontier": 🎙 "đâm hết vô trong cái thằng Frontier đó thì nó sẽ giải quyết hết vấn đề không có". Đưa dữ liệu bẩn vào thì model mạnh nhất cũng 🎙 "nó cũng ngu luôn nó cũng ngáo luôn"
- Cách đúng: 🎙 "vẫn phải đi theo một cái quy trình là triển khai phần mềm". Xuống phân tích, đánh giá dữ liệu của khách, chọn tool cho từng loại dữ liệu.

**Dám nói ngược khách:**

- 🎙 "thôi anh đừng anh đừng có làm ai"
- 🎙 "bắt anh chuẩn hóa cái phần mềm nội bộ của anh trước đi"
- Nghịch lý: 🎙 "người ta sẵn sàng bỏ rất nhiều tiền để tích hợp AI nhưng mà không bỏ tiền để có thể chuẩn hóa được lại cái hệ thống hiện có"

💡 Thứ tự đúng: quy trình chuẩn → dữ liệu đúng → rồi mới gắn AI vào.

---

## 2. Không tất định → kéo về một hệ quy chiếu

- 🎙 "AI nó là noneterministic"
- Với giao tiếp, đó là điểm tốt: 🎙 "mày làm cho tao một bài thơ về đồng quê", 100 lần ra 100 bài khác nhau là tuyệt vời.
- Với số liệu, đó là thảm hoạ: 🎙 "anh em hỏi 100 lần nó ra 100 con số khác nhau là chết mẹ rồi"
- Việc cần làm: 🎙 "làm sao để ép nó về một cái hệ quy chiếu", để 🎙 "cho dù là cái cách trình bày nó khác nhau nhưng mà cái thông tin nó truyền đạt nó phải chính xác"

**Cách kéo về phần lớn không phải AI:** classifier / router nhận loại câu hỏi rồi gọi tool tất định lấy số từ database. Chi tiết ở [03-phan-mem-quanh-model.md](03-phan-mem-quanh-model.md) §1.

💡 **Hiểu đúng:** router làm cho **hệ** dễ đoán hơn. Bản thân model vẫn là xác suất. Con số phải đến từ truy vấn hoặc tool; model chỉ diễn đạt.

---

## 3. Bịa (hallucination)

- QuânIT không tin hướng giải là model frontier to hơn: 🎙 "cái hướng giải quyết không phải là các cái frontier model"
- Lý do 1, dữ liệu train lẫn nhiều mảng: ví dụ model train trên dữ liệu của X, mà 🎙 "X là không phải là một cái platform mà người ta nói chuyện cái dạng formal". Cùng một từ, mỗi ngành một nghĩa.
- Lý do 2, từ chuyên ngành: 🎙 "những cái từ ngữ chuyên ngành khi mà nó nó ép về cái hệ vector embeding á thì nó so sánh như thế nào"
- Lý do 3, mixture-of-experts định tuyến sai: giống như 🎙 "thằng bác sĩ không thể nào trả lời được một cái câu hỏi về luật"

💡 Ví dụ bác sĩ là cách ví cho dễ hiểu. Bịa không chỉ do định tuyến expert sai.

---

## 4. Đánh giá: benchmark ≠ bài toán thật

**Train là chuyện hên xui:**

- 🎙 "mà mình đang train AI á là một cái cách rất là hên xui"
- Một trong các cách train: 🎙 "reinforcement with reward", trả lời đúng thì được thưởng.
- Chỉnh tham số trúng thì ra model thông minh, trượt thì ra model ngốc: 🎙 "tùy nhân phẩm"

**Đo bằng benchmark thì sai chỗ:**

- Benchmark giống 🎙 "những cái bài kiểm tra trong cái lúc mà anh em đi học vậy chứ không phải là những cái bài toán thực tế"
- Có bên 🎙 "cố tình nó overfit" model vào benchmark: 🎙 "cái điểm B max cực kỳ cao nhưng mà tới cái lúc ra đời thì nó cực kỳ ngu". Tên model và hãng nghe từ phụ đề không rõ, đừng trích tên.

💡 **Bài học:** tự dựng bộ ca trên việc thật của mình. Giữ kín, băm trước, chạy một lần. Đừng tin điểm benchmark chung, càng đừng tin benchmark do chính hãng tự dựng.

---

## Bảng nhớ nhanh

| Vấn đề | Dấu hiệu | Cách chống |
|---|---|---|
| Rác vào rác ra | dữ liệu nhiều định dạng, khoá null, số mỗi bảng một kiểu | khảo sát, phân loại, chuẩn hoá **trước** AI |
| Không tất định | hỏi lại ra số khác | router + tool tất định; model chỉ diễn đạt |
| Bịa | trả lời tự tin cái không có | context đúng, có bằng chứng; không đủ thì "biết im" |
| Benchmark | điểm cao, ra đời kém | bộ ca riêng trên việc thật, giữ kín |
