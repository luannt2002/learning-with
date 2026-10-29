# 04 — Use case đáng nhớ

> Bốn ca đầu từ video A (🎙). Các ca sau từ video B, chỉ có qua bản tóm tắt ChatGPT (📝): đọc để hiểu ý, không trích là lời QuânIT.

---

## 1. ⭐ Báo cáo ngày qua Zalo, công ty cầu đường (video A, ca gốc)

**Bối cảnh:** một dự án 🎙 "bên cầu đường". Yêu cầu nghe rất đơn giản: tài xế nhắn Zalo hôm nay chạy bao nhiêu, đổ bao nhiêu dầu, bao nhiêu km.

**Cùng một con số, mỗi người viết một kiểu:**

- 🎙 "4000 lí dầu thì bốn chấm"
- 🎙 "có ông thì nhắn là 4k"
- 🎙 "có ông thì nhắn là 4000 viết riêng"
- 🎙 "bốn rồi chữ ngàn"

**Hậu quả khi đọc sai:** 🎙 "đổ 4k lí dầu cái anh em nhập vô cái báo cáo của người ta 4 L". Lệch 1.000 lần.

**Mức cần đạt:** 🎙 "nó phải chính xác 100% tại vì cái đó là critical"

**Cam kết:** làm cho doanh nghiệp thì phải ký SLA. Như dịch vụ internet cam kết thời gian online, hệ AI phải cam kết độ chính xác. Và câu hỏi: 🎙 "ai là người chịu trách nhiệm"

💡 Bẫy cần nhớ khi tự làm bài này:

| Bẫy | Cách xử |
|---|---|
| Một lượng viết nhiều cách (4k, 4.000, bốn ngàn) | chuẩn hoá về một dạng + đơn vị |
| Ước lượng ("khoảng 4k") khác số chính xác | giữ cờ "ước lượng" |
| Tin sửa, tin gửi trùng | không cộng đôi; sửa có lịch sử |
| Hai nguồn lệch (phiếu kho 3.850 L, đội trưởng báo 4k) | giữ cả hai, không tự chọn, hỏi người |
| Không chắc | cách ly kèm lý do, không đoán |

💡 Phần "hiểu tin nhắn" ở bài này làm được bằng luật + từ điển, chưa cần model. Model chỉ đáng thêm khi luật đo được là trượt.

---

## 2. Hỏi doanh thu 100 lần (video A)

- 🎙 "doanh thu của 2026 là bao nhiêu" mà 🎙 "anh em hỏi 100 lần nó ra 100 con số khác nhau là chết mẹ rồi"
- Ví dụ ngắn nhất để giải thích vì sao **số liệu không được do model sinh ra**. Số phải từ truy vấn hoặc tool; model chỉ diễn đạt.

📝 Bản tóm tắt video B bổ sung: phải định nghĩa "doanh thu nào, đơn vị nào, kỳ nào, chốt lúc nào". Hai con số khác nhau có thể đều đúng nếu chụp ở hai thời điểm khác nhau. Luồng:

```text
câu hỏi → doanh thu nào / đơn vị / thời gian / chốt lúc nào → kiểm quyền → truy vấn có kiểm soát
        → tính bằng code → kèm nguồn → model chỉ diễn đạt
```

---

## 3. "Anh đừng làm AI" (video A)

- Khách gọi tới vì chuyên môn kỹ thuật. Lời khuyên: 🎙 "thôi anh đừng anh đừng có làm ai", chuẩn hoá phần mềm nội bộ trước.
- Khách cãi lại rằng AI làm được thế này thế kia thì QuânIT thôi: 🎙 "thôi anh anh nói đúng quá thế thì em xin phép"

💡 Kinh nghiệm nghề: lời khuyên đúng đôi khi là **bán ít đi**.

---

## 4. YOLO: model tốt ≠ hệ tốt (video A)

Xem [03-phan-mem-quanh-model.md](03-phan-mem-quanh-model.md) §7. Model ảnh số một vẫn trượt khi camera, góc quay, ánh sáng của từng khách khác nhau.

---

## 5. 📝 Tổng đài đặt vé xe (video B)

AI giỏi phần **nói chuyện**: nghe khách, hỏi lại khi thiếu. Còn **đặt vé** là giao dịch tất định.

```text
khách gọi → nghe + bóc ý / trường ─ thiếu? ─► hỏi lại / chuyển nhân viên
          → tra tuyến, giờ, ghế ─ không có? ─► đề xuất chuyến khác
          → KHÁCH XÁC NHẬN → gọi API đặt vé + khoá chống trùng
          → không rõ đã ghi chưa? ─► đối soát, KHÔNG thử lại mù
          → đọc lại từ DB → báo mã vé
```

Bất biến:

- AI không bao giờ tự nói "đã đặt".
- Giữ ghế, chống trùng là nghiệp vụ tất định.
- Timeout **không** có nghĩa là chưa ghi.

---

## 6. 📝 CEO uỷ quyền cho trợ lý (video B)

- Ma trận quyền vẫn giữ. Thêm **uỷ quyền có phạm vi, có thời hạn, thu hồi được**.
- « Identity → Role → Permission → Delegation scope/time → Approval → Audit → Revocation »
- Agent không được có quyền vượt người uỷ quyền.

---

## 7. 📝 Ép công nhân dùng portal (video B)

- Ép công nhân dùng portal dễ thất bại.
- 💡 Đầu vào hợp với người (nhắn tin như cũ), đầu ra máy kiểm được. Ý của bản tóm tắt: « Human-compatible input, machine-verifiable output ».

---

## 8. 📝 "Doanh nghiệp đòi 99% chính xác" (video B)

99% của cái gì? Hiểu ý, trích trường, làm đúng hành động hay hoàn thành cả việc?

💡 Tự tính: 5 bước độc lập, mỗi bước 99% ⇒ cả luồng 0,99⁵ ≈ 95,1%. Đừng lấy một con số chung để cam kết cả luồng.

---

## 9. 📝 Báo cáo công trường từ Zalo (video B)

Video B có một ca riêng, gần giống ca 1 nhưng rộng hơn: đầu vào có cả ảnh, video, audio, báo cáo không theo mẫu.

```text
Zalo / ảnh / video / audio → nhận + ghi nguồn → lưu bản gốc → bóc dữ liệu → đổi về schema sự kiện
  → chống trùng / bản sửa / ghép → đủ bằng chứng? ─ không ─► "chưa xác minh" / hỏi thêm
                                                 └ có ───► kiểm luật nghiệp vụ → bản ghi đã xác minh → báo cáo
```

Ca biên đáng nhớ: báo trùng; gửi muộn; sửa số; hai tổ cùng báo một khối lượng; ảnh không xác minh được khối lượng; không rõ người gửi; giờ xảy ra khác giờ nhận; tài liệu có câu lệnh nhắm vào AI.

Nguyên tắc: « normalized ≠ verified ». Đổi được về schema chưa có nghĩa là đúng.

---

## 10. 📝 AI chỉ là "phần nổi của tảng băng"? (video B)

- Với dữ liệu đã có cấu trúc: đúng, AI chủ yếu là giao diện ngôn ngữ.
- Với nội dung tự do (tin nhắn, ảnh): AI có thể là năng lực lõi để biến chữ / ảnh thành dữ liệu có cấu trúc.
- Dù vậy, khâu xác minh và "sổ gốc" (nơi giữ sự thật) vẫn bắt buộc.
