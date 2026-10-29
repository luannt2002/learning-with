# 06 — Nguyên tắc thiết kế rút từ ba video

> Cả tệp là 💡: ChatGPT hệ thống hoá từ video B + C (bản tóm tắt), cộng phần mình rút ra. Không câu nào ở đây là lời QuânIT.
> Chữ trong « » là nguyên văn bản tóm tắt. Đây là phần "biến mindset thành luật thiết kế", dùng làm bảng kiểm khi làm hệ có AI.

---

## 0. Tuyên ngôn năm vai

« AI xử lý sự mơ hồ; phần mềm xác định bảo vệ bất biến nghiệp vụ; policy kiểm soát quyền; bằng chứng và kiểm thử quyết định mức độ tin cậy; con người phê duyệt những ngoại lệ có rủi ro. »

💡 Cùng hướng với 🎙 "cái AI model nó chỉ nằm ở giữa thôi" của video A, nhưng chia vai rõ hơn: **năm vai, AI chỉ giữ một**.

Câu hay gặp "deterministic shell quanh probabilistic core" (vỏ tất định bọc lõi xác suất) cũng là cách ChatGPT gói ý này (tên mục "Phase 7" trong bản Study Guide video A do ChatGPT viết; không có trong hai bản tóm tắt B/C). Dùng được, nhưng đừng trích là lời QuânIT.

---

## 1. Chọn bài toán trước, chọn công nghệ sau

**Sáu chiều đánh giá:** giá trị · việc này có hợp AI không · dữ liệu sẵn sàng chưa · sai thì hậu quả gì · có kiểm chứng được không · vận hành bền không.

**Luồng quyết định:**

```text
giá trị đo được? ─ không → khảo sát thêm
   └ có → giải pháp tất định đủ tốt? ─ có → SQL / luật / automation, DỪNG ở đây
             └ không → cần hiểu ngôn ngữ / dữ liệu phi cấu trúc? ─ có → LLM / trích xuất / RAG
                                                                     └ kiểm chứng được? ─ có    → workflow có kiểm soát
                                                                                          └ không → thu hẹp / người duyệt / biết im
```

**Ba loại việc:** *tất định* (một đáp án đúng) · *sinh* (nhiều đáp án đều được) · *điều tra* (gom và kiểm bằng chứng). « Không phải tác vụ nào cũng cần Agent. »

---

## 2. Bảy bất biến (thuộc lòng)

« LLM không là source of truth; policy độc lập; thiếu dữ liệu ≠ false; timeout ≠ failed transaction; output hợp schema ≠ đúng; model upgrade cần eval; kết quả audit phải phản ánh sự kiện thực tế. »

| # | Bất biến | Hỏi khi review |
|---|---|---|
| 1 | Model không là nguồn sự thật | con số này đến từ truy vấn hay từ model? |
| 2 | Quyền kiểm độc lập | quyền có được kiểm **trước** khi đọc / ghi, không qua model không? |
| 3 | Thiếu dữ liệu ≠ sai | thiếu thì hệ hỏi lại / im, hay tự điền "không"? |
| 4 | Timeout ≠ giao dịch thất bại | lỗi quanh lúc ghi thì ra "không rõ" và đối soát, hay thử lại mù? |
| 5 | Đúng schema ≠ đúng | có đọc lại / so với bằng chứng không? |
| 6 | Đổi model phải đo lại | có chạy lại bộ ca trước khi bật không? |
| 7 | Vết phản ánh sự thật | lượt lỗi có được ghi không, hay chỉ ghi lượt đẹp? |

💡 Mỗi thay đổi chạm luồng có model phải trả lời được cả bảy câu.

---

## 3. Dữ liệu có trạng thái: « normalized ≠ verified »

```text
Raw (nguyên bản + nguồn + giờ + tenant + quyền)
 → Normalized (đổi về schema, CHƯA mặc định đúng)
 → Verified (qua luật xác minh)   |  Rejected / Inconclusive (thiếu căn cứ, vi phạm luật)
 → Revised (sửa có lịch sử, không ghi đè)
```

- Chuẩn hoá xong chỉ nghĩa là đọc được, chưa nghĩa là đúng.
- Không đủ căn cứ thì để "chưa kết luận", không tự thành sự thật.
- Sửa thì giữ lịch sử, không ghi đè.

---

## 4. Hành động có trạng thái: không bao giờ đoán "đã xong"

« `PROPOSED → AUTHORIZED → EXECUTING → COMMITTED/FAILED/UNKNOWN → VERIFIED`. Không được coi `UNKNOWN` là `FAILED`. »

- Kết quả có **ba** trị: xong / hỏng / **không rõ**.
- "Xong" chỉ tính khi **đọc lại** thấy đúng.
- Gửi lại cùng khoá thì phát lại kết quả cũ, không làm lần hai.
- Phân loại tool theo rủi ro: « `READ_ONLY`, `REVERSIBLE_WRITE`, `IRREVERSIBLE_WRITE`, `HIGH_RISK` ».

---

## 5. Phê duyệt phải gắn với nội dung

« Approval phải gắn với hash/nội dung hành động, input, người phê duyệt, thời hạn và scope. Thay đổi amount/target sau approval phải yêu cầu xét duyệt lại. »

💡 Lỗi kinh điển: duyệt "chuyển 10 triệu", rồi lệnh bị đổi thành 100 triệu mà phê duyệt vẫn còn hiệu lực. Gắn phê duyệt với băm nội dung chặn đúng lỗi này.

Uỷ quyền: « Identity → Role → Permission → Delegation scope/time → Approval → Audit → Revocation ».

---

## 6. Tách bốn trường, đừng gộp thành một "điểm tin cậy"

« Tách `model_confidence`, `evidence_sufficiency`, `validation_result`, `decision_status`. »

```text
model_confidence      model TỰ nói nó chắc bao nhiêu
evidence_sufficiency  bằng chứng có đủ không
validation_result     code kiểm có qua không
decision_status       hệ quyết gì: giao / hỏi lại / chặn
```

💡 **Confidence ≠ correctness.** Chỉ hai trường giữa (bằng chứng và kiểm bằng code) mới đáng để ra quyết định. Ví dụ thật ở [07](07-jev-deterministic-ai.md) §2: cùng một ý, model nói 0,90 ở câu hỏi này, 0,63 ở câu hỏi kia.

---

## 7. Mười hai nguyên tắc

| Nguyên tắc | Nghĩa ngắn |
|---|---|
| Problem-first | bài toán trước công nghệ |
| Automation-before-agent | tự động hoá thường trước, agent sau |
| Data-before-reasoning | dữ liệu đúng trước, suy luận sau |
| Evidence-before-conclusion | có bằng chứng mới kết luận |
| Policy-before-action | kiểm quyền trước khi làm |
| Verification-before-trust | đọc lại trước khi tin |
| Contracts-before-integrations | hợp đồng nội bộ trước, nối hãng sau |
| Evaluation-before-upgrade | đo trước khi đổi model |
| Failure-is-a-valid-state | "không biết", "không rõ" là trạng thái hợp lệ |
| Observability-by-design | vết từng bước ngay từ đầu |
| Economics-per-outcome | tính chi phí trên việc được chấp nhận |
| Progressive complexity | chỉ thêm công nghệ khi có lỗi đo được |

---

## 8. Năm tầng

```text
5. Giá trị kinh doanh
4. Kết quả kiểm chứng được   (bằng chứng, kiểm, đo, audit)
3. Thực thi có kiểm soát      (workflow, quyền, giao dịch, duyệt)
2. Trí tuệ thay được          (model, RAG, adapter)
1. Nền tin được               (danh tính, dữ liệu, hợp đồng)
```

💡 Tầng 2 là tầng **thay được**. Tầng 1, 3, 4 là tầng **giữ đúng**. Đổi model chỉ được chạm tầng 2.

---

## 9. Chỗ không nên theo bản tóm tắt

💡 Bản tóm tắt ChatGPT đề xuất dựng hẳn một "nền tảng AI" ôm model gateway, tool gateway, orchestration, classifier bằng model ở đầu luồng, roadmap P0–P7, 14 chức năng. Đó là **đề xuất kiến trúc của ChatGPT**, không phải mindset của QuânIT. Tự nó cũng ghi « Không nhất thiết là 14 service hoặc 14 model call. ».

Đi ngược tinh thần 🎙 "có khi một số cái chẳng cần ai" nếu dựng hết từ đầu. Chỉ thêm khi có lỗi đo được.
