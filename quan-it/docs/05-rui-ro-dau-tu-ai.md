# 05 — Rủi ro khi đầu tư AI cho doanh nghiệp (video C)

> Cả tệp là 📝 / 💡. Transcript video C không có; ý lấy qua bản tóm tắt ChatGPT. Không câu nào ở đây được trích là "QuânIT nói".
> Video B hỏi "AI có hợp việc này không". Video C hỏi "hệ có AI có **sống được lâu** không, khi model, nhà cung cấp, giao thức và người vận hành thay đổi".

---

## 1. Sáu rủi ro video C nói tới

| # | Rủi ro | Ví dụ trong video (qua tóm tắt) |
|---|---|---|
| 1 | Đầu tư theo phong trào | lãnh đạo sẵn sàng chi khoảng 5 tỷ mà chưa biết làm tính năng gì |
| 2 | Công nghệ đổi nhanh | MCP, SDK đổi cách gọi; hệ dựng hôm nay mai có thể gãy |
| 3 | Phụ thuộc bên thứ ba | nhà cung cấp ngừng model, tăng giá, hạn chế truy cập |
| 4 | Model mạnh hơn lại làm hỏng | đổi model "xịn hơn" làm hỏng workflow đang chạy tốt |
| 5 | Không ai tiếp quản | đơn vị triển khai nghiệm thu, nhận tiền, rút; khách không tự vận hành được |
| 6 | Dữ liệu và quy trình chưa chuẩn | bản tóm tắt nối ý này với video B; 💡 cùng tinh thần "rác vào rác ra" của video A |

---

## 2. Đầu tư theo phong trào → đo bằng kết quả nghiệp vụ

💡 Đừng đo thành công bằng số agent, số người dùng chatbot hay số token đã tiêu. Đo bằng việc thật:

```text
Trước AI: 3 giờ/ngày tổng hợp báo cáo
Sau AI:   30 phút/ngày kiểm và hoàn thiện
Điều kiện: chất lượng dữ liệu không giảm; chi phí triển khai + vận hành tính vào
```

**Chi phí trên mỗi việc được chấp nhận:**

```text
= tổng chi phí mọi lần chạy (cả thử lại, người soát, vận hành) / số việc được chấp nhận
```

Ví dụ đã tính lại: 1.000 báo cáo × 12 phút = 200 giờ → sau AI × 4 phút = 66,7 giờ → tiết kiệm 133,3 giờ ≈ 13,33 triệu/tháng ở 100.000 đ/giờ, **trước khi trừ chi phí**.

Lưu ý: giờ công quy ra tiền **≠** tiền mặt tiết kiệm được thật.

📝 Bản tóm tắt hai video B/C có nhắc cách bán hàng bằng nỗi sợ rủi ro (ví dụ chào bán firewall). 💡 Nỗi sợ giúp ra quyết định nhanh, nhưng không thay được phân tích rủi ro minh bạch: kịch bản, xác suất, tác động, chi phí cơ hội.

---

## 3. Công nghệ đổi nhanh → tách ba loại thay đổi

📝 Video ví "MCP 1.0 gọi function, 2.0 gọi class, đổi là sập". Bản tóm tắt ghi rõ ví dụ này **không đúng đặc tả**, nhưng rủi ro thì có thật. Phải tách:

| Loại thay đổi | Là gì |
|---|---|
| Đổi giao thức | bắt tay, khám phá, thoả thuận phiên bản, kênh truyền |
| Đổi SDK | class / hàm của thư viện đổi, giao thức có thể vẫn tương thích |
| Đổi hành vi | API vẫn gọi được nhưng kết quả khác |

💡 Cách đỡ: nghiệp vụ không gọi thẳng MCP.

```text
workflow nghiệp vụ → hợp đồng tool nội bộ (mã tool, input, output, quyền, lỗi)
                   → adapter MCP / HTTP / native → tool bên ngoài
```

Đổi kênh chỉ là đổi adapter. Nhưng adapter mới vẫn phải được kiểm cả **hành vi**, không chỉ kiểu dữ liệu.

---

## 4. Nhà cung cấp ngừng model → đổi model không chỉ là đổi mã model

- Model có thể bị khai tử, bị giới hạn theo tài khoản / khu vực, đổi chính sách hoặc giá. Model vẫn còn mà mình mất quyền dùng cũng là một kiểu ngừng.
- 💡 **Sổ model:** hãng, mã, năng lực, vòng đời, ràng buộc truy cập, việc được duyệt dùng, trạng thái đo, bản thay thế, chi phí / độ trễ.
- « Không hardcode fallback sang model chưa được kiểm thử. »

---

## 5. Model mạnh hơn chưa chắc hợp hơn → ba mức tương thích

| Mức | Nghĩa |
|---|---|
| **API** | gọi và nhận cùng dạng |
| **Năng lực** | cùng có ngữ cảnh, ảnh, tool, đầu ra có cấu trúc… |
| **Hành vi** | trên việc thật vẫn đạt cùng tiêu chí nghiệm thu |

« API compatible không đồng nghĩa behavior compatible. »

💡 Đổi sang model "giỏi hơn" có thể: viết dài hơn mà giữ schema kém hơn; tự "sửa" trường dữ liệu nó thấy lạ; chọn tool khác trong ca mơ hồ; chậm hơn; tốn token hơn mà kết quả nghiệp vụ không hơn.

💡 **Đổi model như một lần phát hành:** ứng viên → kiểm năng lực → chạy bộ ca offline → test an toàn → đo chi phí / độ trễ → chạy bóng / canary → duyệt → có kế hoạch quay lui.

📝 Video có nhắc tư duy SOA "cắm–chạy": không hợp thì thay nhà cung cấp khác. 💡 Đúng về mục tiêu, nhưng chỉ đạt được ở mức API. Mức hành vi chỉ bộ ca trên việc thật mới bắt được.

---

## 6. Không ai tiếp quản → hệ phải tự giải thích được

💡 Khi một luồng hỏng, người kỹ thuật phía khách phải trả lời được mà không cần gọi vendor:

- Yêu cầu đã đi qua những bước nào?
- Model nào, phiên bản nào?
- Tool nào trả lỗi?
- Lỗi do nhà cung cấp, do dữ liệu hay do luật nghiệp vụ?
- Thử lại có an toàn không?
- Có đường lùi không, và đường lùi đã chạy đúng chưa?

Hệ không trả lời được mấy câu này thì demo đẹp nhưng không vận hành nổi.

---

## 7. Dữ liệu chưa chuẩn → làm từng phần, không chờ chuẩn hết

💡 Bản tóm tắt (ChatGPT) phản biện chính video C: nó không cho rằng phải chuẩn hoá cả doanh nghiệp xong mới dùng AI.

> Dữ liệu chưa chuẩn có thể được AI hỗ trợ làm sạch, nhưng dữ liệu chưa xác minh không được tự động trở thành sự thật nghiệp vụ.

Hai giọng khác nhau, đừng gộp:

- Video A nói thẳng: 🎙 "bắt anh chuẩn hóa cái phần mềm nội bộ của anh trước đi", chuẩn cả phần mềm nội bộ, quy trình và số liệu rồi mới gắn AI.
- ChatGPT đề xuất: làm từng phần, cho AI hỗ trợ làm sạch, miễn kết quả phải qua xác minh.

Đây là **ý của ChatGPT**, không phải QuânIT tự nới lời mình.

---

## 8. Local hay cloud

- 📝 Video nhận xét model open-weight lớn quá nặng cho phần cứng thường.
- 💡 Không chọn cứng một phía. Model nhỏ cho phân loại / trích xuất / dữ liệu nhạy cảm; model lớn qua API cho suy luận khó; code thường cho phần tất định. Chọn theo dữ liệu, tuân thủ, năng lực, chi phí, độ trễ.

---

## 9. Bảng rủi ro đầy đủ (💡 bản tóm tắt mở rộng)

| Rủi ro | Nguyên nhân | Kiểm soát |
|---|---|---|
| Đầu tư theo phong trào | không có mốc so, không ROI | đặt bài toán, chỉ số nghiệp vụ |
| Dữ liệu sai / thiếu | quy trình thật lệch SOP | chất lượng dữ liệu, ghi nguồn, người soát |
| Model đổi hành vi | prompt / tool hiểu khác | chạy lại bộ ca |
| Nhà cung cấp ngừng model | khai tử, chính sách, quyền truy cập | sổ vòng đời model |
| MCP / SDK gãy | đổi giao diện / kênh | hợp đồng nội bộ + adapter |
| Bị khoá vào một hãng | lõi phụ thuộc SDK / hãng | tầng tách hãng |
| Không có đội vận hành | vendor rút | người chịu trách nhiệm, runbook, quan sát |
| Local quá nặng | GPU / RAM / độ trễ | chọn đường theo năng lực và chi phí |
| Chi phí phình | thử lại, agent lặp, ngữ cảnh dài | ngân sách, trần, quy chi phí cho từng việc |
| Hành động gây hậu quả | thử lại mù, thiếu khoá chống trùng | an toàn giao dịch |
| AI vượt quyền | tool quá mạnh, tài liệu có câu lệnh cài cắm | kiểm quyền độc lập với model |
| Chất lượng tụt âm thầm | không đo, không vết | giám sát + cổng phát hành |

💡 **Câu chốt của bản tóm tắt** (ChatGPT, không phải QuânIT): "thay thế được sự thông minh bên ngoài, nhưng không đánh mất sự đúng đắn bên trong".
