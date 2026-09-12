# 03 — Individual Reflection

> Viết bằng lời của bạn (Phase 7 trong `01-worksheet.md`). Có thể dùng AI gợi ý câu hỏi tự soi, không dùng AI viết thay. 8-12 câu, có chuyện cụ thể.

## Thông tin cá nhân

- Họ và tên: Dương Minh Hiếu
- Mã học viên: 2A202602488
- Nhóm: Nhóm 5 (Dương Minh Hiếu, Nguyễn Vũ Quang Anh, Vũ Quốc Bảo, Cao Văn Trường, Nguyễn Thị Chinh)
- Candidate problem nhóm chọn: Khi cư dân gửi yêu cầu dịch vụ không khẩn cấp bằng mô tả tự do, thông tin có thể thiếu và phiếu có thể bị chuyển qua nhiều bộ phận trước khi đến đúng đơn vị xử lý.

---

## 1. Tôi đã tham gia vào phần nào?

Ghi việc cụ thể + kết quả cụ thể. Không ghi chung chung kiểu "tham gia thảo luận".

| Hoạt động | Tôi đã làm gì? (việc cụ thể) | Kết quả / ảnh hưởng tới nhóm |
|---|---|---|
| Scan cá nhân | Quét 10 vấn đề thực tế từ bối cảnh kỹ sư Computer Vision thực tập tại công ty Camera AI (multi-threading RTSP, đồng bộ timestamp WebSocket, false alarm 45%, buffer queue...). | Cung cấp 10 problem có số liệu đo đạc cụ thể; lập 3 Problem Card chi tiết với workflow Mermaid, mang lại góc nhìn kỹ thuật hệ thống cho nhóm. |
| Pitch Problem Card | Pitch Problem Card #1 về "Lọc báo động giả (False Alarm) trong Camera AI giám sát xâm nhập", phân tích điểm nghẽn phán đoán tức thời trên 1 frame gây "alert fatigue" cho an ninh. | Bài được nhóm đánh giá rất cao về tính rõ ràng kỹ thuật, lọt vào shortlist top 3 và xếp thứ hai toàn nhóm trong bảng chấm điểm đồng thuận (28/35 điểm). |
| Challenge bài của bạn khác | Đặt câu hỏi phản biện bài lừa đảo người cao tuổi (Bảo) về tính khả thi tiếp cận dữ liệu cuộc gọi/quyền riêng tư; phản biện bài hồ sơ bệnh án (Quang Anh) về rủi ro dữ liệu y tế nhạy cảm; phản biện bài tài xế Green SM (Chinh) vì bản chất là bài toán vận hành logistics. | Giúp nhóm loại bỏ các đề tài có rủi ro pháp lý/dữ liệu vượt quá khả năng kiểm chứng trong lab 4 tiếng, thu hẹp sự tập trung vào bài toán có quy trình handoff rõ ràng. |
| Gom trùng / cluster | Đọc và phân tích 15 đề xuất của 5 thành viên thành 4 cụm logic (A: Phân luồng & cấu trúc hóa thông tin; B: Giám sát & cảnh báo tự động; C: Phát hiện bất thường sinh học/logistics; D: Thông tin hỗ trợ quyết định). | Giúp nhóm nhận diện pattern chung của các bài toán: nhóm thấy rõ sự khác biệt giữa bài toán nhận diện tín hiệu cảm biến với bài toán điều phối quy trình nghiệp vụ. |
| Chọn candidate problem | Trực tiếp đối chiếu giữa bài Camera AI của mình và bài Phản ánh cư dân của Quang Anh; chủ động lùi bài cá nhân để biểu quyết chọn bài cư dân. | Nhóm đạt đồng thuận 100% chọn bài toán có đầy đủ chuỗi handoff (Cư dân → CSKH → Điều phối → Đội kỹ thuật), tạo điều kiện tốt nhất để phân tích ranh giới Rule vs AI. |
| Validation / research | Cùng nhóm phân tích các quote giả lập phỏng vấn CSKH/Điều phối; chỉ ra thời gian chậm sau tiếp nhận chủ yếu do chờ vật tư chứ không do phân luồng; nghiên cứu pattern từ Zendesk và ServiceNow. | Giúp nhóm định vị đúng điểm nghẽn ở khâu tiếp nhận/chuyển đúng bộ phận, không quy kết sai trách nhiệm cho AI về việc rút ngắn thời gian sửa chữa ngoài hiện trường. |
| Workflow nhóm | Thiết kế các nhánh rẽ và fallback trong Future Workflow: bổ sung chốt chặn Rule-based tách riêng trường hợp khẩn cấp chuyển sang người trực theo SOP; thiết kế luồng fallback khi AI confidence thấp. | Đảm bảo Future Workflow có đầy đủ 5 yếu tố cốt lõi (Rule, AI, Người, Boundary, Fallback), thể hiện rõ ràng và an toàn trên sơ đồ Mermaid. |
| Problem Statement | Tham gia xây dựng từ PS v0 lên PS v1; thắt chặt boundary (chỉ xử lý 2-3 nhóm dịch vụ không khẩn cấp, cấm AI tự dispatch/đóng phiếu); chuẩn hóa 3 metric định lượng và guardrail. | Hoàn thiện Problem Statement v1 chặt chẽ, có công thức tính từ log (T0, tỷ lệ đúng lần đầu tăng ≥20%, thời gian giảm ≥40%, guardrail 0% lọt ca khẩn cấp). |
| Rule / Workflow / Agent | Phân tích ma trận Mơ hồ × Phức tạp; kịch liệt phản biện xu hướng muốn làm "Autonomous Agent" tự gọi API giao việc; lập luận chọn Workflow có CSKH xác nhận. | Kéo nhóm thoát khỏi bẫy "solution-first", thống nhất chọn giải pháp Workflow có Human-in-the-loop vừa vặn với bài toán và đảm bảo an toàn vận hành. |
| Decision | Cùng nhóm rà soát checklist 6 câu hỏi và kiên quyết chọn quyết định "Not Yet" thay vì vội vã "Go", do chưa có 100 log phiếu thật để đo baseline T0 và chưa có owner pilot. | Bản quyết định cuối cùng đạt tính trung thực, dũng cảm dựa trên bằng chứng kỹ thuật, có kế hoạch validation rõ ràng trước khi triển khai thử nghiệm. |

**Dấu tay rõ nhất của tôi trong artifact cuối (1-2 câu):**

```text
Dấu tay rõ nhất của tôi là việc thiết lập các boundary an toàn và cơ chế fallback trong Future Workflow và PS v1: tách riêng nhánh khẩn cấp để xử lý theo SOP người thật, thiết kế fallback khi AI trích xuất độ tin cậy thấp, và kiên quyết hạ cấp giải pháp từ AI Agent tự hành xuống Workflow có CSKH phê duyệt bắt buộc.
```

---

## 2. Bảng dùng AI (mỗi dòng 1 phase có dùng AI — 2 cột cuối bắt buộc)

| Phase | Tôi dùng AI để làm gì? | AI hữu ích ở đâu? | AI sai / hời hợt ở đâu? | Tôi sửa gì bằng nhận định của mình? |
|---|---|---|---|---|
| Scan | Hỏi các điểm nghẽn kỹ thuật thường gặp trong pipeline Camera AI monitoring (multi-threading RTSP, đồng bộ WebSocket metadata, buffer queue). | Gợi ý đúng thuật ngữ chuyên môn và hiện tượng "alert fatigue" của nhân viên an ninh do tỷ lệ báo động giả ngoại cảnh cao. | Gợi ý các ý tưởng viển vông, phi thực tế như "AI Agent tự động sinh code pipeline C++" hoặc "nhận diện hàng nghìn loại đối tượng camera". | Lọc bỏ hoàn toàn các ý tưởng mơ hồ; tự quét 10 problem từ trải nghiệm thực tập thực tế với số liệu đo lường cụ thể (45% false alert, lệch 300-800ms). |
| Problem Card | Nhờ AI chuyển đổi mô tả các bước xử lý của Problem Card #1 và #2 thành cú pháp sơ đồ Mermaid flowchart. | Tạo nhanh khung cú pháp sơ đồ Mermaid chuẩn, giúp trực quan hóa chu kỳ cảnh báo tức thời. | AI tự ý gộp bước kiểm tra điều kiện hình học ROI và bước phán đoán model, làm mờ đi điểm nghẽn cốt lõi (single-frame bottleneck). | Tách bạch rõ 2 bước phát hiện và kiểm tra vùng ROI; bổ sung ghi chú chỉ ra nguyên nhân gốc rễ là thuật toán thiếu nhận thức theo chuỗi thời gian (temporal context). |
| Workflow | Nhờ AI rà soát xem sơ đồ Future Workflow bản nhóm có bị thiếu nhánh rẽ ngoại lệ hoặc rơi vào vòng lặp kín không thoát được không. | Nhắc nhở nhóm cần bổ sung nhánh xử lý khi dữ liệu cư dân đính kèm bị lỗi (ảnh mờ, hỏng file, thiếu thông tin bắt buộc). | AI gợi ý cho phép hệ thống tự động giao việc trực tiếp cho kỹ thuật viên nếu điểm tự tin của mô hình > 90% để "tối đa hóa tốc độ". | Bác bỏ gợi ý tự động dispatch của AI; kiên quyết giữ vai trò CSKH duyệt 100% phiếu trong giai đoạn pilot để ngăn chặn rủi ro phân công sai đội thợ. |
| Research | Tìm kiếm các giải pháp thương mại và tài liệu kỹ thuật về bài toán tự động phân loại yêu cầu dịch vụ khách hàng/cư dân. | Gợi ý trích dẫn chính xác các case study thực tế từ AppFolio Smart Maintenance, ServiceNow Record Categorization và Zendesk Triage. | AI tự bịa số liệu "giúp giảm 70% thời gian xử lý toàn hệ thống" mà không dẫn link nguồn kiểm chứng được. | Tự truy cập và đọc link tài liệu chính thức của từng hãng; loại bỏ mọi số liệu không nguồn; chỉ rút ra bài học cốt lõi về mô hình "AI đề xuất - Người duyệt". |
| Problem Statement | Nhập vai giám khảo khó tính để phản biện và tìm lỗ hổng trong bản thảo Problem Statement v0. | Phát hiện ra cụm từ "xử lý phản ánh chậm" quá mơ hồ, dễ gây nhầm lẫn giữa khâu phân luồng với khâu sửa chữa vật lý tại hiện trường. | Gợi ý các metric định tính chung chung như "nâng cao trải nghiệm sống của cư dân", "tối ưu hóa quy trình quản lý tòa nhà". | Cụ thể hóa thành 3 metric đo từ log (T0, Chuyển đúng lần đầu, Thời gian đến đúng bộ phận, Cần hỏi bổ sung) và bổ sung guardrail 0% lọt ca khẩn cấp. |
| Rule / Workflow / Agent | Yêu cầu AI so sánh ưu nhược điểm giữa mô hình Autonomous Agent tự hành và mô hình Workflow có kiểm soát cho bài toán của nhóm. | Phân tích rõ các rủi ro bảo mật thông tin và sự cố phân quyền khi cấp quyền cho Agent tự gọi API các hệ thống nội bộ. | AI có xu hướng "tâng bốc" công nghệ mới, khẳng định Agent là xu thế bắt buộc phải làm để tối ưu chi phí vận hành. | Bác bỏ định hướng của AI; lập luận dựa trên tính chất luồng nghiệp vụ đã cố định để chứng minh Workflow là lựa chọn vừa vặn, tiết kiệm và an toàn nhất. |
| Decision | Tham vấn checklist đánh giá quyết định Go / Not Yet / No-Go cho dự án thử nghiệm AI trong doanh nghiệp. | Cung cấp khung câu hỏi chặt chẽ về dữ liệu baseline và sự sẵn sàng của bộ phận tiếp nhận vận hành (business owner). | AI khuyên nhóm nên chọn "Go với phạm vi nhỏ" để vừa làm vừa sửa, mang tính lạc quan thái quá. | Cùng nhóm kiên định chọn "Not Yet"; lập luận rằng khi chưa có trong tay 100 log phiếu thật để đo baseline T0 thì chưa đủ cơ sở kỹ thuật để Go. |

> Nếu phase nào không dùng AI, ghi `Không dùng` và vì sao tự làm.

---

## 3. Reflection câu hỏi mở

Chọn 3-4 câu trong 6 câu dưới để viết thành đoạn 8-12 câu (không trả lời bullet 1 dòng):
- Tôi học được gì khi nghe top 3 problems của các bạn khác?
- Nhóm có lúc nào bị solution-first, đòi làm Agent cho ngầu không?
- Tôi có thay đổi ý kiến sau khi bị challenge không, vì sao đổi?
- Tôi đóng góp gì thật sự vào artifact cuối, phần nào có dấu tay của tôi?
- Điều khó nhất khi viết Problem Statement là gì, metric hay boundary?
- Nếu làm lại, tôi sẽ challenge nhóm mạnh hơn ở điểm nào?

**Reflection:**

```text
Khi lắng nghe top 3 problems của các thành viên trong nhóm, tôi nhận ra một bài toán thuần kỹ thuật chưa hẳn đã là một bài toán AI giá trị nếu thiếu đi bối cảnh vận hành và chuỗi bàn giao (handoff) giữa con người với nhau. Ban đầu, tôi rất tự tin bảo vệ đề tài "Lọc cảnh báo ảo cho Camera AI" của mình vì có sẵn số liệu thực tập và công thức đo đạc quen thuộc trong ngành Computer Vision. Tuy nhiên, khi nhóm challenge về tính đóng kín của bài toán camera chỉ nằm trong nội bộ một phòng kỹ thuật, tôi đã chủ động thay đổi ý kiến để đồng thuận chọn đề tài xử lý phản ánh cư dân vì nó phản ánh trọn vẹn điểm nghẽn giao tiếp giữa Cư dân, CSKH và Đội kỹ thuật. Ở Phase 6, nhóm từng có lúc rơi vào bẫy solution-first khi có thành viên hào hứng đề xuất dựng một AI Agent toàn năng có thể tự đọc mô tả rồi tự động phân công thợ sửa chữa cho "ngầu". Tôi đã kiên quyết dùng tư duy kỹ thuật để phản biện, chỉ ra rằng việc để Agent tự hành khi chưa có dữ liệu kiểm chứng sẽ gây hỗn loạn điều phối và tiềm ẩn rủi ro an toàn nghiêm trọng nếu phân loại sai sự cố cháy nổ hay rò rỉ khí gas. Điều trăn trở và khó khăn nhất với tôi khi viết Problem Statement không phải là đặt mục tiêu metric, mà là việc thiết lập ranh giới (boundary) và cơ chế dự phòng (fallback) để tách bạch hoàn toàn luồng khẩn cấp sang quy trình người thật xử lý theo SOP. Dấu tay rõ nét nhất của tôi trong báo cáo nhóm chính là việc kiên trì giữ vững nguyên tắc "Human-in-the-loop", biến AI thành trợ thủ trích xuất và đề xuất có độ tin cậy thay vì một cỗ máy ra quyết định thay con người. Quyết định chọn "Not Yet" ở bước cuối cùng là trải nghiệm đáng giá nhất với tôi, bởi nó khẳng định rằng làm AI chuyên nghiệp là phải tôn trọng bằng chứng từ dữ liệu thật và giá trị của các giải pháp Rule-based đơn giản trước khi nghĩ đến những mô hình phức tạp.
```

---

## 4. Tự kiểm cuối bài (check trước khi nộp repo)

- [x] [12đ] Cá nhân có 5+ problems + top 3 Problem Cards
- [x] [12đ] Tôi đã pitch rõ + challenge nhóm đúng trọng tâm (ghi ở bảng mục 1)
- [x] Nhóm có nhật ký hội tụ từ candidates về 1 bài
- [x] [15đ] Nhóm có workflow trước/sau
- [x] [20đ] Nhóm có PS v0/v1 với metric + boundary rõ
- [x] [15đ] Nhóm có so sánh No AI / Rule / Workflow / Agent
- [x] [10đ] Nhóm có Go / Not Yet / No-Go + lý do rõ
- [x] [10đ] Reflection này có vai trò thật + AI giúp/sai ở đâu + điều học được + nếu làm lại đổi gì
- [x] [6đ] Tôi tự giải thích được mạch problem → workflow → metric → boundary → độ phù hợp AI


