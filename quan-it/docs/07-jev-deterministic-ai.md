# 07 — "Jeff là deterministic AI": thật ra là gì

> Video A có nhắc "Jeff" như một hướng giúp AI tất định. Tệp này trả lời: đó là gì, đúng tới đâu, thử thật ra sao.
>
> | Nhãn | Nghĩa |
> |---|---|
> | 🎙 | nguyên văn video A |
> | **[ĐO-THỬ]** | số đo trong báo cáo thử Jev của một dự án nội bộ (không công khai), chạy ngày 24/10/2026; **chưa chạy lại** |
> | **[ĐO-NGOÀI]** | số do tác giả các repo cộng đồng tự chạy; mình chưa chạy lại |
> | **[VENDOR]** | bên làm Jev tự nói |
> | 💡 | mình rút ra |

---

## 0. Một trang

```text
QuânIT (🎙):  classifier ~90%  vs  "identic router" 100%  →  "thằng Jeff… đưa về một cái hệ nhất định… deterministic"
Thực tế:      Jev = model QUYẾT ĐỊNH zero-shot: đáp án nằm trong tập ĐÓNG + XÁC SUẤT cho từng đáp án
              ├─ "đóng"     → ĐÚNG: không bao giờ trả ngoài danh sách đáp án
              ├─ "tất định" → SAI nếu nói về model: cùng câu, xác suất dao động quanh 0,5 giữa các lần gọi
              └─ "100%"     → 30 task tổng hợp, chạy một lần; task thật 54–100%
Thử thật:     phân loại ý định: luật 23/30 · Jev 24/30 → không đáng thêm
Bài học:      TẤT ĐỊNH nằm ở CODE quanh model (ngưỡng, tổ hợp, đường lùi)
```

---

## 1. QuânIT nói gì

Ngữ cảnh là đoạn nói về router:

- 🎙 "cái mức độ tin cậy của nó chỉ là có 90% nhưng mà cái thằng identic router này mức độ của nó là 100%"
- 🎙 "một số thằng như thằng Jeff hay là những cái thằng gì đó nó đang giải quyết á nó đưa nó đưa về một cái hệ nhất định thôi", để 🎙 "nó trở thành deterministic chứ không phải là non deterministic", rồi 🎙 "anh em phải thử"
- Cuối video, trong danh sách hướng mới phải biết: 🎙 "Jeff Jeff là một cái cái deterministic deterministic AI"

**"Jeff" là Jev** (của TypeSafe), phụ đề tự động nghe nhầm. Có một repo mở tên `jeff` nói cùng giao thức, nhưng điểm Decision Index chỉ 8,04 so với Jev 57,91 [ĐO-NGOÀI]. 💡 Nhiều khả năng QuânIT đang nói sản phẩm chính.

💡 Tinh thần đáng giữ: **"phải thử"**. QuânIT không bảo dùng ngay.

---

## 2. Jev là gì, "deterministic" đúng sai ở đâu

**Jev:** gửi một trạng thái + nhiều **câu hỏi có kiểu** → nhận **xác suất** cho từng đáp án, trong một lượt, **không sinh chữ**. Ba kiểu câu hỏi: chọn 1 trong nhiều nhãn · chấm thang điểm · có / không. Weights đóng, chỉ gọi qua API [VENDOR]. Có bí danh "bản mới nhất" tự đổi theo phiên bản ⇒ **phải ghim mã phiên bản**.

| Nếu hiểu "deterministic" là… | Đúng? | Bằng chứng |
|---|---|---|
| Đầu ra luôn thuộc tập đáp án cho trước | **Đúng** | thiết kế của câu hỏi có kiểu |
| Cùng câu hỏi → cùng đáp án | **Sai** | tài liệu của chính hãng: cùng câu dao động 0,43–0,53, cắt qua ngưỡng 0,5 [VENDOR]; sân thử cộng đồng chạy 4 lần: một câu ra *yes 50% / no 51% / yes 52% / no 51%*, 3 lần lật trên 132 câu [ĐO-NGOÀI] |
| Xác suất nói đúng độ tin | **Một phần** | gọi thử: cùng một ý, câu chọn nhãn ra 0,90 nhưng câu có / không chỉ 0,63 ⇒ không dùng ngưỡng mặc định [ĐO-THỬ] |
| Router đúng 100% | **Chỉ trên bài dễ** | 25/25 tool + 5/5 "không tool" là **30 task tổng hợp, chạy một lần**; task tài liệu thật 54–100% tuỳ việc, công cụ chuyên dụng bằng hoặc hơn [ĐO-NGOÀI] |
| "Không bịa" | **Một phần** | nghĩa thật: đầu ra **không lệch schema**. Vẫn có thể **sai nghĩa** |

💡 **Kết luận:** gọi đúng tên là "model quyết định zero-shot, tập đáp án đóng, có xác suất". Phần **tất định** nằm ở **code bọc quanh**: ngưỡng do ứng dụng giữ, chia 3 vùng (chặn / cho qua / chuyển người), lỗi thì quay về đường tất định. Tài liệu cộng đồng cũng ghi: "Code decides; the model only supplies evidence", và "một phán quyết không phải là giấy phép để hành động".

Muốn tất định ở chính model thì phải tự host: một bản mở thay Jev (kev, 0,8B–27B) cho kết quả giống hệt từng bit khi chạy cùng kernel [ĐO-NGOÀI]. Đó là tất định nhờ **cố định môi trường chạy**, không nhờ model.

→ Câu QuânIT "đưa về một cái hệ nhất định" **đúng nếu "hệ" là cả hệ thống**, sai nếu hiểu là bản thân model. Khớp với 🎙 "cái AI model nó chỉ nằm ở giữa thôi".

---

## 3. Chọn công cụ nào cho việc nào

| Công cụ | Làm tốt | Không làm |
|---|---|---|
| Luật / code | số học, so ngày, quyền, kiểm nhất quán | hiểu câu nói lạ |
| Classifier nhỏ fine-tune | việc cố định **có nhãn để train** (79,7–81,4 điểm, hơn Jev 72,5 [ĐO-NGOÀI]) | việc mới chưa có nhãn |
| Model quyết định kiểu Jev | quyết định nhanh **không cần train**, có xác suất | suy luận nhiều bước, đếm, số học |
| LLM | suy luận, viết | rẻ và ổn định |

---

## 4. Thử thật (một dự án nội bộ, 24/10/2026) [ĐO-THỬ]

Bộ 64 câu soạn mù, băm trước, mỗi phương án chạy **một lần**. Bộ này đã dùng rồi: lần sau muốn đo lại phải soạn **bộ mới**, đọc kết quả rồi thì bộ cũ không còn là bộ giữ kín.

| Chỗ thử | Kết quả | Quyết định |
|---|---|---|
| Phân loại ý định (/30) | luật cũ 4 · **luật dò chỉ mục 23** (0 lời gọi model) · Jev 24 · luật rồi Jev 25 | không dùng Jev: chỉ cứu thêm 1–2 câu, không đạt cổng "giảm ≥ 30% lỗi" |
| Chấm đoạn, bổ sung cho xếp hạng | chặn **8/8 câu không đáp án bị lọt đoạn** (trong 16 câu không đáp án); nhưng **loại oan 6 đoạn đúng**, cả 6 là câu hai ý (hỏi số + hỏi quy trình); câu một ý giữ 27/27 | chỉ chạy bóng, ghi vết, không lọc |
| Chấm đoạn, thay xếp hạng | xếp hạng 31/36 · Jev 27/36 | bỏ |
| Quyền, tenant, phiên bản, số liệu, giao dịch | — | **không bao giờ** giao cho model |

⚠️ Lưu ý con số 23/30: bản luật đã chạy có lỗi, đẩy câu số liệu sang nhánh tài liệu. Ở chỗ này Jev an toàn hơn: 0 câu số liệu lọt, luật lọt 1. Bản luật đã sửa ước khoảng 21/30, **chưa đo**. Nên đọc "luật gần bằng model" là "luật kém model 1–3 câu, chưa đo lại", không phải "bằng".

**Hai phát hiện đắt hơn cả Jev:**

1. **Lỗi không nằm ở model.** Phần lớn câu trượt là do **cấu hình quyền** thiếu, câu hỏi bị chặn dù đã hiểu đúng ý. Sửa cấu hình lợi hơn mọi phương án có model, và không tốn đồng nào.
2. **Tầng sai kế tiếp là tìm tài liệu, không phải phân loại.** Trên bộ phát triển, mấy câu Jev cứu thêm đều kết thúc "không đủ bằng chứng": hiểu đúng ý nhưng không tìm ra đoạn.

💡 Đúng bài QuânIT: 🎙 "có khi một số cái chẳng cần ai". Luật tốn 0 đồng mà chỉ kém model vài câu (xem lưu ý 23/30 ở trên). Muốn thắng phải sửa **dữ liệu / cấu hình**, không phải thêm AI.

---

## 5. Học cái hay, bỏ cái dở

**Học:**

- Câu hỏi **nguyên tử**; ngưỡng và tổ hợp nằm trong code.
- **Ghim mã phiên bản** và ghi tên model vào vết.
- Tách **"liên quan"** khỏi **"trả lời được"**.
- Chọn ngưỡng trên bộ có nhãn; hoà thì chọn mức bảo thủ.
- Lỗi bên ngoài ⇒ về đường tất định và **đếm lỗi**.
- Tách **"quyết làm gì"** khỏi **"sinh chữ gì"**: model quyết định chọn hành động, LLM nhỏ chỉ viết chữ.

**Không học:**

- Coi đúng kiểu là đúng nghĩa.
- Coi confidence 1,0 là chắc chắn.
- Dùng ngưỡng 0,5 / 0,7 mà không đo.
- Để model chọn quyền, nguồn, phiên bản.
- Tin benchmark do hãng tự dựng: tỉ lệ "nhanh hơn 193,6×, rẻ hơn 444,6×" đo trên nhãn là **trung bình đáp án của hai LLM**, không phải đáp án đúng.
- Nhét model khắp luồng vì "nghe hợp kiến trúc".

**Ai cam kết độ chính xác?** (câu 8 của cổng, 🎙 "ai là người chịu trách nhiệm"): bên làm Jev không có SLA công khai, cũng chưa thấy công bố khách production nào có SLA [ĐO-THỬ]. Không ai ký thì không giao việc critical.

**Khi nào nên dùng kiểu Jev:** quyết định ngữ nghĩa mơ hồ thật, chưa có nhãn để train, chấp nhận dữ liệu ra ngoài, có code giữ ngưỡng và đường lùi. Có nhãn rồi thì fine-tune model nhỏ; việc có công cụ chuyên dụng thì dùng công cụ đó.

---

## 6. Lưu ý nguồn

Báo cáo thử Jev có mục phản biện các câu như "Jev là SLM", "80% TF/Keras làm rồi", "chưa ai dùng production". Những câu này **không có trong video A** (phụ đề tự động của video A không có SLM, keras, production) ⇒ **đừng gán cho QuânIT**.
