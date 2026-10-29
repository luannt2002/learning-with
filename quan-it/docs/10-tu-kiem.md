# 10 — Tự kiểm (ôn)

> Đọc câu, tự trả lời, rồi mới mở đáp án. Số trong ngoặc là tệp để đọc lại.

## Video A: luận điểm và vấn đề lõi

<details><summary>1. "AI chỉ là một tech stack" nghĩa là gì? (01)</summary>
AI là một công nghệ có mạnh có yếu, như blockchain "chỉ là một cái database". Nó chưa thành chuẩn như stack web, vẫn đang thử–sai. Đừng thần thánh hoá, cũng đừng chê bỏ.</details>

<details><summary>2. Vì sao "hỏi doanh thu" là ví dụ mạnh nhất? (02, 04)</summary>
Giao tiếp thì mỗi lần một khác là tốt, còn số liệu thì một câu hỏi phải ra một đáp án. Số phải từ tool / truy vấn, model chỉ diễn đạt.</details>

<details><summary>3. "4k lít dầu" dạy những bẫy nào? (04)</summary>
Nhiều cách viết một số; nhầm đơn vị (4k → 4 L, lệch 1.000 lần); ước lượng khác số chính xác; tin trùng / tin sửa; hai nguồn lệch. Mức cần là 100% vì dính tiền.</details>

<details><summary>4. Phần nào của hệ AI "chẳng liên quan gì tới AI"? (03)</summary>
Phân loại và đọc dữ liệu, router / classifier, tool truy vấn, context builder, kiểm đầu ra. QuânIT gọi là "architecture và lập trình thuần túy".</details>

<details><summary>5. Khi nào nên khuyên khách KHÔNG làm AI? (02, 04)</summary>
Khi dữ liệu và quy trình nội bộ chưa chuẩn. Chuẩn hoá phần mềm trước, AI sau.</details>

<details><summary>6. Benchmark sai ở đâu? (02)</summary>
Giống bài thi ở trường, không phải bài toán thật; có bên cố tình overfit. Phải đo trên bộ ca của chính việc mình, giữ kín.</details>

<details><summary>7. RAG khác fine-tune thế nào? (03)</summary>
Fine-tune đổi trọng số model bằng dữ liệu của khách. RAG giữ nguyên model, lo gom đúng context (A B C D E) rồi mới hỏi; hệ lo việc lấy từng loại thông tin.</details>

<details><summary>8. Vì sao YOLO "số một" vẫn có thể trượt? (03)</summary>
Mỗi doanh nghiệp một camera, góc quay, ánh sáng. Model tốt ≠ hệ tốt; phải biết train lại trên dữ liệu của khách.</details>

## Video B, C (qua tóm tắt)

<details><summary>9. Tổng đài đặt vé: AI làm phần nào, phần nào không? (04)</summary>
AI nghe và hỏi lại. Giữ ghế, chống trùng, ghi, đọc lại xác nhận là phần mềm tất định. Timeout ≠ chưa ghi.</details>

<details><summary>10. "99% chính xác" sai ở đâu? (04)</summary>
Phải hỏi 99% của cái gì. 5 bước mỗi bước 99% thì cả luồng chỉ còn ≈ 95,1%.</details>

<details><summary>11. Đổi sang model "mạnh hơn" thì phải làm gì? (05)</summary>
Chạy lại bộ ca của từng việc trước khi bật. Tương thích API không có nghĩa là tương thích hành vi.</details>

<details><summary>12. Đơn vị triển khai rút đi thì hệ cần có gì? (05)</summary>
Vết từng bước: đi qua đâu, model nào, tool nào lỗi, lỗi do ai, thử lại có an toàn không, đường lùi đã chạy chưa.</details>

## Nguyên tắc thiết kế

<details><summary>13. Bảy bất biến là gì? (06)</summary>
Model không là nguồn sự thật · quyền kiểm độc lập · thiếu dữ liệu ≠ sai · timeout ≠ thất bại · đúng schema ≠ đúng · đổi model phải đo lại · vết phản ánh sự thật.</details>

<details><summary>14. Vì sao "normalized ≠ verified"? (06)</summary>
Đổi được về schema chỉ nghĩa là đọc được, chưa nghĩa là đúng. Phải qua luật xác minh; không đủ căn cứ thì để "chưa kết luận".</details>

<details><summary>15. Phê duyệt gắn băm chặn lỗi gì? (06)</summary>
Nội dung lệnh bị đổi (số tiền, đối tượng) sau khi đã duyệt mà phê duyệt vẫn còn hiệu lực.</details>

<details><summary>16. Vì sao không gộp mọi thứ thành một "điểm tin cậy"? (06, 07)</summary>
Model tự tin không có nghĩa là đúng. Quyết định dựa trên bằng chứng đủ và code kiểm qua.</details>

## Jev và nguồn

<details><summary>17. Jev có deterministic không? (07)</summary>
Đầu ra đóng: có. Cùng câu ra cùng đáp án: không. Tất định thật nằm ở code: ngưỡng, 3 vùng, đường lùi.</details>

<details><summary>18. Câu nào trong cổng 9 câu không có lời gốc? (08)</summary>
Câu 9 "đồ có sẵn giải được chưa": suy diễn.</details>

<details><summary>19. "deterministic shell quanh probabilistic core" là của ai? (09)</summary>
ChatGPT. Ý vẫn hay, nhưng không được trích là "QuânIT nói".</details>

<details><summary>20. Câu nào của QuânIT nên nhớ nhất? (README)</summary>
"ai chỉ là một công nghệ thôi" · "cái AI model nó chỉ nằm ở giữa thôi" · "có khi một số cái chẳng cần ai".</details>

## Backend Go / microservice (P1, P2)

<details><summary>21. Vì sao đặt Kafka trước Elasticsearch? (12)</summary>
Làm bộ đệm khi log tăng vọt, để ES không sập. [P1 23:00]</details>

<details><summary>22. Trên Kibana tìm lỗi thế nào cho nhanh? (12)</summary>
Add filter theo <code>service.name</code> + <code>level: error</code>, thu hẹp khung thời gian. [P1 43:15]</details>

<details><summary>23. 4 partition mà chạy 6 consumer cùng group thì sao? (11)</summary>
2 consumer ngồi không. Số partition là trần cứng của mức song song. [P1 74:55]</details>

<details><summary>24. Làm sao để mọi sự kiện của cùng một sản phẩm được xử lý đúng thứ tự? (11)</summary>
Gửi kèm message key = Product ID. Và tầng DB vẫn phải kiểm điều kiện khi trừ tồn kho. [P1 77:00]</details>

<details><summary>25. Vì sao JSON nặng hơn Protobuf? (13)</summary>
Bản tin JSON nào cũng mang tên field. Protobuf chỉ gửi số thứ tự field + wire type + giá trị. [P2 22:00]</details>

<details><summary>26. Tính tag cho field số 3 kiểu string? (13)</summary>
<code>(3 << 3) | 2 = 26 = 0x1a</code>. [P2 26:00]</details>

<details><summary>27. Scale gRPC lên 100 pod mà 1 pod gánh hết, vì sao? (13)</summary>
LB tầng L4 + kết nối HTTP/2 sống lâu. Cần LB L7 hoặc Service Mesh; tự kiểm bằng load test + log tên pod. [P2 46:00, 56:00]</details>

<details><summary>28. gRPC có tự an toàn không? (13)</summary>
Không. Luật encode công khai; không TLS thì Wireshark vẫn đọc được string. Mạng nội bộ cũng phải TLS. [P2 58:30, 77:30]</details>

<details><summary>29. Startup có nên tách microservice, mỗi service một DB ngay không? (11, 13)</summary>
Không. Monolith / Modular Monolith, dùng chung DB tới khi có product-market fit và nghiệp vụ ổn định. [P2 16:00, 98:05]</details>

<details><summary>30. Vì sao API chỉ publish, còn consumer chạy thành tiến trình riêng? (11, 12)</summary>
API theo mô hình hỏi–đáp; consumer chạy nền liên tục. Gộp chung thì không scale riêng được từng phần. [P1 35:05]</details>

## Nghề engineer thời AI (14)

<details><summary>31. AI đang gỡ nút thắt nào, nút thắt dời sang đâu? (14)</summary>
Gỡ khâu Coding. Nút thắt dời sang Review (AI sinh hàng trăm nghìn dòng/ngày, người duyệt tay không nổi), rồi tới Test (AI mới test được phần đơn giản). [27:12, 29:55, 34:30]</details>

<details><summary>32. Vì sao "one-shot" (ném AI vào cả chuỗi) là sai? (14)</summary>
Làm sản phẩm là vòng lặp: Review / Test sai thì quay lại System design / Planning. Không phải một đường thẳng để AI chạy một lần. [21:00–22:00]</details>

<details><summary>33. Ví dụ "notification = true nhưng danh sách nhận rỗng" dạy gì? (14)</summary>
Lời người nói không chặt; AI làm đúng chữ mà sai ý, và không "nói xạo". Phải kiểm kết quả thật (có ai nhận được không), không tin lời AI tự khai. [44:00]</details>

<details><summary>34. "What is good?" trả lời thế nào? (14)</summary>
Không có "tốt nhất" tuyệt đối. Tìm "phù hợp nhất" với ngữ cảnh dự án và chi phí; cần người có thực chiến cân. [48:00–50:30]</details>
