# 14 — Tương lai engineer thời AI: nút thắt dời từ Coding sang Review, Test

> Video [Tương Lai Engineer Thời AI | Lời Khuyên Thật Lòng | Quân IT](https://www.youtube.com/watch?v=uSA8p0zm8RE), đăng 28/10/2026, dài 61:58.
> **Cả tệp là 🎬** (ý qua bản ghi, không phải nguyên văn), trừ chỗ ghi 🎙 / 💡 / ⚠️. Chữ trong ngoặc kép ở thân bài là lời bản ghi chép lại, không phải trích nguyên văn.
>
> | Nhãn | Nghĩa |
> |---|---|
> | 🎬 | ý trong video, lấy qua **bản ghi Gemini** (chạy 28/10/2026). Không phải nguyên văn, **không kiểm được từng chữ**. Mốc `[MM:SS]` là mốc trong video gốc |
> | « » | chữ **trên slide** (bản ghi chép lại) |
> | ⚠️ | số nói miệng / chỗ bản ghi chưa chắc |
> | 💡 | mình rút ra, không phải lời trong video |
>
> **Người nói:** chủ kênh Quân IT (video đăng trên kênh Quân IT; bản ghi ghi người nói là "Quân IT"). Ông nói mình có khoảng 20 năm làm nghề [17:05] và đang làm AI cho ngành xây dựng (vết nứt, lỗi sơn, nhà tiền chế PEB) [59:00]. Cả video là **nhận định cá nhân**; ông nói thấy đúng thì tiếp thu, thấy sai thì phản biện công khai [02:30].

---

## 0. Một trang

```text
Phương pháp khoa học:   Observation → Hypothesis → Experiment → Refine hypothesis → Theory
                                 ║ (cùng một hình)
Làm sản phẩm:           Ideas → Brainstorming → System design → Planning
                        Coding → Review → Test → Release        (không thẳng: Review/Test sai thì quay lại Design/Planning)

Nút thắt (Constraints):  trước đây = CODING  ──AI gỡ được──►  giờ = REVIEW  ──►  kế tiếp = TEST
Câu khó nhất còn lại:    « What is good? »  — tốt là "phù hợp nhất" với ngữ cảnh + chi phí, không phải "tốt nhất"
Engineer nên:            « Personal image » · « Be curious, imaginative » · « Cohesive » (cởi mở)
```

---

## 1. Hai cực sai khi nhìn AI [01:55–02:30]

- **"AI wash":** thần thánh hoá, coi AI làm được mọi thứ.
- **"Non-believer":** coi AI là bong bóng sắp vỡ.
- Ở giữa có nhóm coi AI "chỉ là một tool". Theo bản ghi, ông cho rằng cách nhìn này **cũng chưa đủ**.

💡 Cùng giọng với video A: 🎙 "ai chỉ là một công nghệ thôi", không thần thánh, cũng không chê bỏ ([01](01-ai-chi-la-mot-tech-stack.md) §1).

---

## 2. Nền chung: con người luôn tìm cách sống dễ hơn [03:00–06:00]

- « As a human, we always find a way to improve life »
- Ví dụ: người thường nay đi máy bay, tắm nước nóng, sống lâu hơn vua chúa xưa.
- Cách con người cải thiện: **phát minh**. Công cụ tạo phát minh thời hiện đại: **phương pháp khoa học**.

**Phương pháp khoa học [06:00–09:00]:** « Observation → Hypothesis → Experiment → Refine hypothesis → Theory »

- Thực nghiệm chứng minh giả thuyết **sai** (*falsify*) cũng là thành công, vì loại được một giả định sai [08:16].
- Không ngành nào áp máy móc được hết: thiên văn không đem hành tinh vào phòng lab, chủ yếu quan sát [08:58].

💡 Đây là khung để đọc mọi phần sau: mỗi trào lưu công nghệ chỉ là **giả thuyết** cho tới khi qua thực nghiệm.

---

## 3. Lịch sử coding: lập trình càng dễ, coder càng **nhiều** [10:00–16:30]

```text
« Big machines with 0, 1 switches » → « Low level language, assembly, C » → « High level, compiler, interpreter » → « More descriptive language: Python »
```

- Coder đầu tiên là nhà khoa học, cầm sơ đồ đi gạt từng công tắc 0/1.
- Mỗi bậc sau làm việc viết code dễ hơn, gần tiếng người hơn. Ông mở code Python cơ bản trên GitHub của mình để chỉ cú pháp gần tiếng Anh [14:00].
- Câu hỏi then chốt: code dễ hơn thì số coder có giảm không? **Không, càng ngày càng tăng** [15:26]. Chi phí làm phần mềm giảm ⇒ dự án bùng nổ ⇒ cần thêm kỹ sư.
- Điều duy nhất đổi: **skillset**. Ai bám kỹ năng cũ thì bị thay [16:27].

💡 Bản ghi gọi hiện tượng này là nghịch lý Jevons (hiệu suất tăng thì tổng tiêu thụ tăng). Đó là nhận xét của Gemini, không phải lời QuânIT.

---

## 4. Làm sản phẩm không phải đường thẳng — đừng "one-shot" [16:48–26:00]

**Slide:** « Coding future with AI — How building a tech product actually looks »

```text
Ideas → Brainstorming → System design → Planning
Coding → Review → Test → Release
« Constraints »        « New descriptive language — What is good? »
```

- Quy trình này là **phương pháp khoa học mở rộng**: Ideas = quan sát đời thật; System design = module nào, đăng nhập ra sao, video lưu đâu, lazy load, DB gì [18:00–20:00]; Planning = DB trước, API sau, giao diện sau cùng [20:00].
- **Sai lầm lớn nhất hiện nay:** muốn AI giải trọn gói "one-shot", ném AI vào cả chuỗi từ ý tưởng tới release [21:00].
- Ngoài đời là **vòng lặp**: Review / Test thấy sai thì quay lại System design / Planning, lặp nhiều lần [22:00].
- **Hype là giả thuyết:** Metaverse, Web3 từng được cả thị trường tin; avatar đầu tiên "không có chân". Nhìn lại thì dễ, lúc đó ai cũng tin [23:00–25:30].
- AI hấp dẫn và nguy hiểm vì **bán một giấc mơ**: ngồi không, AI làm hết, cuối tháng lĩnh lương [26:00].

💡 Trùng ý video A: 🎙 "cái gì cũng slap yolo vô" và 🎙 "những cái công ty lớn thì nó đang bán cái ước mơ đó anh em".

---

## 5. Nút thắt dời chỗ: Coding → Review → Test [27:00–36:00]

- **Constraints** = nút thắt cổ chai của cả quy trình [17:00, 22:20].
- Bấy lâu nút thắt là **Coding**: founder có ý tưởng nhưng không biết code thì tắc; business hỏi "tuần này release được không" thì dev nói "không biết, phải code và test xong". Vì thế mới sinh ra PM, CTO làm cầu nối [28:00–29:30].
- **AI hiện mới giải được đúng một khâu: Coding** [27:12].
- ⇒ Nút thắt **lập tức dời sang Review**: AI sinh hàng trăm nghìn dòng mỗi ngày, người review tay **không nổi** [29:55–30:15, 33:17].
- Một số công ty thử mô hình "Coding Factory": nhiều mô-đun tự động phối hợp review rồi thử nghiệm, tinh chỉnh (ông quên tên công ty) [34:00].
- **Test** là nút thắt kế tiếp: AI mới làm được test đơn giản (so giao diện với bản thiết kế, lệch chữ / màu, trang 404, ping API đo độ trễ qua APM, bắt dữ liệu null / sai định dạng). Test logic sâu thì **AI còn bó tay** [34:30–35:00].
- AI gỡ **từng nút thắt một**, không đổi mọi thứ sau một đêm [35:00].

**Năng suất:**

- AI viết code + người review + người test ⇒ năng suất cả đội tăng khoảng **30%** [30:55]. ⚠️ Số nói miệng, ông bảo "anh em có thể fact-check", không nhớ nguồn.
- Trước cần ~4 người (frontend, backend, DB / lưu trữ, fullstack / DevOps); nay một senior "cứng" làm trọn: giao code cho AI, **tự review và giữ kiến trúc** [31:50–32:55].
- Lương kỹ sư Mỹ / châu Âu thời đỉnh $20.000–30.000 / tháng vì coding là khâu khó nhất [36:50]. ⚠️ Số nói miệng.

💡 **Bài học cho mình:** khi AI viết code, việc đắt nhất không còn là viết mà là **tin được nó đúng**. Cổng review / test phải chạy bằng máy, người chỉ đọc phần máy không chặn được.

---

## 6. "What is good?" và ngôn ngữ mô tả mới [36:00–53:00]

**Ngôn ngữ tự nhiên thiếu định nghĩa:**

- Ví dụ: hỏi AI "mày có tắt notification không?". AI đáp "không, vẫn để `notification = true`". Nhưng danh sách người nhận lại là **mảng rỗng** ⇒ không ai nhận được gì [44:00–44:35].
- Theo định nghĩa thì AI **không nói xạo**. Lỗi ở chỗ lời nói của người không chặt [44:30].
- Vì thế lập trình phải dừng ở ngôn ngữ có cú pháp khắt khe: để nói rõ người muốn máy làm gì [45:00].
- Từ nhiều nghĩa theo ngành; MoE định tuyến chuyên gia mà vẫn khó phân ranh giới nghĩa [46:00].

**Ngôn ngữ mô tả mới:**

- Ông dẫn giả thuyết của Elon Musk: tương lai không còn ngôn ngữ lập trình, người viết mô tả, máy dịch thẳng ra mã máy [38:00–39:00]. Ông nhấn: **vẫn chỉ là giả thuyết** [39:30].
- Ông đoán: prompt hôm nay "tự do, tuỳ tiện" sẽ thành **ngôn ngữ mô tả có quy tắc riêng** cho từng hệ AI; mô tả sai quy tắc thì AI hiểu sai [51:00].
- Không tin sẽ có một model tối thượng làm hết không cần người: **nhu cầu người đổi mỗi ngày**, chính người còn chưa biết mình muốn gì [52:00].

**"What is good?" là câu khó nhất [39:46, 48:00–50:30]:**

- Thế nào là system design tốt, code review tốt? Giải pháp tốt ở dự án này có thể tồi ở dự án khác (microservices vs monolith).
- Còn có **chi phí**: giải pháp tối ưu lý thuyết mà tốn thêm vài nghìn đô hạ tầng mỗi tháng thì có còn "tốt"?
- ⇒ Tìm **"phù hợp nhất"**, không tìm **"tốt nhất"**.
- Chỉ senior / director / CTO có thực chiến mới cân được. LLM là mô hình thống kê đoán token tiếp theo, không tự cân hết các chiều ngữ cảnh [48:30–50:30].

**Ý tưởng "chưa ai làm" thường là ý tưởng tồi [40:00–43:00]:**

- Nhiều bạn trẻ gọi lúc 1–2 giờ sáng rủ làm "sản phẩm AI đột phá" nhưng giữ bí mật vì "chưa ai nghĩ ra".
- Sản phẩm chưa ai làm rất có thể giải **bài toán không có thật**.
- Ví dụ mở tiệm điện thoại: Thế Giới Di Động, FPT Shop mở đầy đường **chứng minh** nhu cầu là có thật.

💡 Nối với video A: 🎙 "nó đem lại giá trị gì cho người dùng" và cổng câu 1 "giá trị thật là gì" ([08](08-cong-9-cau-va-kinh-nghiem-nghe.md) §1). Ví dụ notification là bản "không tất định" ở tầng **ngôn ngữ yêu cầu**: model làm đúng chữ, sai ý ⇒ phải có test kiểm **kết quả** (có ai nhận được không), không kiểm lời AI tự khai.

---

## 7. Engineer nên làm gì bây giờ [53:00–61:58]

**Slide:** « What it means to engineers right now » — « Personal image » · « Be curious, imaginative » · « Cohesive »

- **Hết thời hoàng kim** (lương nghìn đô, snack, stock). Vào nghề vì hào nhoáng thì sẽ khủng hoảng. Ngành vốn luôn đổi, không đợi AI; giai đoạn thanh lọc buộc ai cũng học lại [53:30–54:30].
- **Be curious, imaginative:** đừng hỏi "AI code hết rồi em bỏ nghề à?". Thấy coding khó mà AI làm được thì phải **tò mò nó làm thế nào**: LLM, RAG, harness, computer use [55:00]. Ông từng làm web, mobile, ráp máy, blockchain, giờ AI; skillset đổi theo chu kỳ khoảng 5 năm; vẫn tự bỏ tiền thử AI hằng ngày [56:00]. Hiểu bản chất thì hết sợ, như người xưa hết sợ sấm sét khi khoa học giải thích được [57:00].
- **Cohesive:** ông dùng theo nghĩa **cởi mở, không bảo thủ**, không phải nghĩa "cohesion" của module [57:30].
- **Personal image:** AI làm ai cũng khoe được "hệ thống ERP", nhưng chưa ai kiểm chất lượng ⇒ **uy tín công khai** là thứ tách mình khỏi đám đông. Ông làm YouTube chia sẻ quá trình (giả thuyết, thử, hỏng, chỉnh) nên công ty xây dựng tự tìm tới [58:00–60:00]. Đừng sợ bình luận chửi [60:45].
- Kỹ năng làm slide, trình bày rõ là kỹ năng để lên senior: phải nói chuyện với nhà đầu tư, đối tác, sếp [00:43–01:03].

💡 "Personal image" ở đây không phải đánh bóng: là **công khai quá trình làm thật** (giả thuyết → thử → sai → chỉnh). Đó chính là phương pháp khoa học ở §2 đem ra trước mặt người khác.

---

## 8. Nối với các video khác 💡

| Video mới | Đã có ở |
|---|---|
| Đừng "one-shot", đừng ném AI vào cả chuỗi | video A: 🎙 "cái gì cũng slap yolo vô" ([03](03-phan-mem-quanh-model.md) §7) |
| AI "bán một giấc mơ" ngồi không | video A ([01](01-ai-chi-la-mot-tech-stack.md) §4) |
| Hype Metaverse / Web3 là giả thuyết | video A: bài học blockchain ([01](01-ai-chi-la-mot-tech-stack.md) §2) |
| LLM là mô hình thống kê đoán token | video A: 🎙 "nó cũng chỉ là một cái statistical model thôi" |
| MoE và từ nhiều nghĩa theo ngành | video A: bịa vì định tuyến chuyên gia sai ([02](02-bon-van-de-loi.md) §3) |
| Ý tưởng "chưa ai làm" ⇒ bài không có thật | video A: hỏi giá trị thật; video C: đầu tư theo phong trào ([05](05-rui-ro-dau-tu-ai.md) §1) |
| AI chỉ test được phần đơn giản, test logic sâu còn của người | Jev: đúng kiểu ≠ đúng nghĩa ([07](07-jev-deterministic-ai.md) §2) |
| "Phù hợp nhất", có tính chi phí | buổi P2: startup dùng Monolith trước ([11](11-go-microservice-mindset.md) §3) · video C: chi phí trên việc được chấp nhận |
| Hiểu bản chất mới hết sợ, phải tò mò | buổi P2: host cảnh báo phụ thuộc AI mà không hiểu nền thì "toang" ([11](11-go-microservice-mindset.md) §1) |

---

## 9. ⚠️ Chỗ chưa chắc

- **30% năng suất**, **$20.000–30.000 lương**, **20 năm kinh nghiệm**, **chu kỳ 5 năm** đều là số nói miệng, không có nguồn trong video.
- Câu "giả thuyết Elon Musk" là lời QuânIT kể lại qua bản ghi, chưa đối chiếu với phát ngôn gốc.
- Phần "💡 Nhận xét của AI" trong bản ghi (Copilot 55%, McKinsey 20–45%, Reality Labs lỗ 40 tỷ, Theory of Constraints) là **Gemini tự thêm**, không phải lời video, nên không đưa vào tệp này.
