# 01 — Individual Problem Scan

> Điền theo Phase 1 + Phase 2 trong `01-worksheet.md`. Tự scan trước, dùng AI sau để phản biện. Không copy ví dụ Weekly Report.

## Thông tin cá nhân

- Họ và tên: Dương Minh Hiếu
- Mã học viên: 2A202602488
- Vai trò / bối cảnh: Sinh viên mới tốt nghiệp ngành Data Science, hiện đang làm Thực tập sinh Computer Vision Engineer tại công ty giải pháp Camera AI giám sát (AI Monitoring).
- Công việc hằng tuần (3-5 gạch đầu dòng để soi problem):
  - Phát triển và tối ưu các pipeline Computer Vision phục vụ bài toán AI monitoring (phát hiện xâm nhập vùng cấm, đếm người/xe, nhận diện hành vi bất thường).
  - Áp dụng kỹ thuật multi-threading xử lý đồng thời nhiều luồng RTSP từ IP camera, decode frame và quản lý hàng đợi (buffer queue) để tránh nghẽn luồng / giật lag.
  - Đóng gói luồng video đã vẽ overlay (bounding box, alert text) và stream lên Node Media Server (qua giao thức RTMP/HLS).
  - Đẩy metadata thời gian thực (tọa độ bounding box, object class, track ID, confidence, alert timestamp) qua WebSocket/HTTP API lên Web Dashboard cho frontend hiển thị.
  - Phối hợp với team Frontend và DevOps kiểm thử độ trễ (latency), đồng bộ khung hình - metadata, và lọc cảnh báo ảo (false positives) tại các site thử nghiệm.

---

## Phase 1 — Scan 5+ problems (tối thiểu 5, khuyến khích 8-10)

**Cách điền:** mỗi dòng = việc gì + ai chịu + đo bằng gì. Cột `Dấu hiệu thật` bắt buộc có số: mất bao lâu (bấm giờ mấy lần), mấy lần/tuần, bao nhiêu người gặp, log/ticket/quote nào.

| # | Lăng kính (Lặp lại / Tốn thời gian / AI có thể tốt hơn / Pain từ người khác) | Problem quan sát được | Ai chịu ảnh hưởng? | Dấu hiệu thật (số + bằng chứng) |
|---|---|---|---|---|
| 1 | AI có thể tốt hơn | Hệ thống AI monitoring báo động giả (False Positives) liên tục do lá cây đung đưa, bóng người đổ dài hoặc đèn pha ô tô quét vào vùng cấm (ROI) | Nhân viên giám sát an ninh (bảo vệ), CV Intern | 1 ca trực 8 tiếng có khoảng 120 cảnh báo thì ~55 cảnh báo là giả (tỷ lệ báo ảo ~45%). Bảo vệ tốn 30-40 phút/ca chỉ để bấm tắt thông báo popup |
| 2 | Tốn thời gian | Lệch đồng bộ (Time-sync mismatch) giữa video stream trên Node Media Server và metadata Bounding Box gửi qua WebSocket lên Web | Frontend Dev, CV Intern | Bounding box hiển thị đi trước hoặc sau đối tượng trong video từ 300ms - 800ms. Mỗi tuần tốn 3-4 tiếng debug log timestamp giữa 2 tiến trình multi-thread |
| 3 | Lặp lại | Đo đạc benchmark FPS, GPU memory usage và latency thủ công mỗi khi deploy model mới hoặc cắm thêm 4-8 luồng RTSP camera | CV Intern, DevOps | Mất 45-60 phút/lần đo đạc và ghi chép tay vào file Excel/Sheet; lặp lại 2-3 lần/tuần mỗi đợt release |
| 4 | Pain từ người khác | Frontend Dev thường xuyên phàn nàn cấu trúc JSON payload metadata WebSocket bị thay đổi thiếu báo trước hoặc bị drop frame metadata khi GPU quá tải | Frontend Dev, CV Intern | 3-4 lần/sprint FE phải nhắn Slack yêu cầu format lại schema; trung bình 5% frame metadata bị drop khi chạy đồng thời trên 8 camera |
| 5 | Tốn thời gian | Tìm kiếm và cắt trích xuất các đoạn video (video snippets) bị miss detection hoặc false alarm từ hàng trăm GB video lưu trữ để gom dữ liệu re-train | CV Intern, Data Annotation team | Mất 3-5 tiếng/tuần tua thủ công các video lưu trữ trên NAS/Server để tìm 15-20 đoạn clip góc cạnh (edge cases) bổ sung vào dataset |
| 6 | Lặp lại | Cấu hình tham số camera (RTSP URL, FPS, resolution, skip-frame rate, queue buffer size, tọa độ đa giác ROI) thủ công cho từng camera qua file YAML/JSON | Kỹ sư triển khai On-site, CV Intern | Mỗi site có 15-20 camera; mất 1.5 - 2 tiếng/site để nhập tay tọa độ và test ping từng luồng RTSP |
| 7 | AI có thể tốt hơn | Không tự động phân loại nguyên nhân khi một luồng camera bị mất kết nối hoặc đứng hình (do đứt cáp mạng, camera treo, tràn RAM buffer hay Node Media Server crash) | Kỹ sư vận hành hệ thống, CV Intern | Mỗi ngày trung bình 2-3 sự cố ngắt luồng; mất 15-20 phút mở log terminal kiểm tra từng container/tiến trình mới biết nguyên nhân |
| 8 | Pain từ người khác | Người trực an ninh ban đêm bị "alert fatigue" (chai lì cảnh báo) vì chuông báo động kêu dồn dập vào lúc trời mưa to hoặc có gió bão | Nhân viên an ninh trực ca đêm | Ghi nhận phản ánh từ khách hàng: 2 lần bảo vệ tắt luôn âm lượng cảnh báo và bỏ sót 1 vụ công nhân trèo rào vào ban đêm |
| 9 | Tốn thời gian | Cân chỉnh thủ công các tham số multi-threading (kích thước Queue, số lượng Worker Threads, tần suất sleep thread) khi chuyển model chạy giữa các thiết bị phần cứng khác nhau (Server RTX 3060 vs Jetson Orin) | CV Intern | Mất từ 4-6 tiếng chạy thử nghiệm, profiling CPU/GPU để tìm ra điểm cân bằng không bị rớt frame (dropped frames) trên phần cứng mới |
| 10 | Lặp lại | Tổng hợp báo cáo tuần về hiệu năng của hệ thống camera giám sát (tỷ lệ uptime luồng, FPS trung bình các cam, số lượng event đã bắt được trong tuần) | CV Intern, Team Lead | Mất 60-90 phút vào chiều thứ Sáu hàng tuần để query log từ database và xuất biểu đồ gửi Team Lead |

> Gợi ý tự soi: tuần trước mất nhiều thời gian nhất vào việc gì? Việc gì hay trì hoãn? Người khác hay hỏi lại câu gì? Workflow nào ai cũng biết là chậm?

**AI đã dùng ở Phase 1 (nếu có):**
- Prompt đã hỏi: "Liệt kê các điểm nghẽn kỹ thuật và bài toán thực tế thường gặp trong pipeline Camera AI monitoring từ khâu multi-threading RTSP, stream lên Node Media Server đến đẩy metadata WebSocket lên Web frontend."
- Ý dùng được: Vấn đề lệch timestamp giữa video stream và metadata WebSocket; hiện tượng alert fatigue do false positive từ ngoại cảnh; nghẽn buffer queue khi decode đa luồng RTSP.
- Ý bỏ vì không phải pain thật: "Tự động sinh toàn bộ code pipeline bằng AI Agent" (không thực tế vì hệ thống phụ thuộc nhiều vào C++/Python bindings, driver GPU và cấu hình mạng thực tế); "Nhận diện hàng nghìn loại đối tượng camera tổng hợp" (quá viển vông, bài toán của công ty chỉ tập trung vào an ninh giám sát người/xe/hành vi).

**Self-check Phase 1:**
- [x] Đủ 5+ dòng, mỗi dòng có actor + số đo cụ thể
- [x] Dùng ít nhất 3/4 lăng kính
- [x] Không có dòng chung chung kiểu "mất nhiều thời gian"

---

## Phase 2 — Top 3 Problem Cards

### 2.1. Chọn top 3

Giữ bài nào: actor cụ thể, workflow vẽ được 3-7 bước, bottleneck ở 1 bước, impact đo được. Loại bài quá rộng.

| Rank | Problem (copy từ bảng scan) | Vì sao chọn (2-3 ý) | Điều còn chưa chắc |
|---|---|---|---|
| 1 | Hệ thống AI monitoring báo động giả (False Positives) liên tục do ngoại cảnh (lá cây, bóng đổ, đèn rọi) vào vùng cấm | Pain point nhức nhối nhất của người dùng cuối (bảo vệ); workflow xử lý rõ ràng; impact đo đạc được trực tiếp bằng số lượng alert giả giảm xuống | Chưa chắc việc thêm logic kiểm chứng theo chuỗi thời gian có làm tăng độ trễ (latency) của cảnh báo lên nhiều không |
| 2 | Lệch đồng bộ (Time-sync mismatch) giữa video stream trên Node Media Server và metadata Bounding Box gửi qua WebSocket lên Web | Tần suất xuất hiện cao trong quá trình phát triển; ảnh hưởng trực tiếp đến chất lượng nghiệm thu giao diện Web; đo đạc được bằng mili-giây (ms) | Khó xác định hoàn toàn nguyên nhân do mạng phía client (jitter) hay do tiến trình multi-thread phía backend |
| 3 | Tìm kiếm và trích xuất các đoạn video clip góc cạnh (edge cases) bị miss/false alert từ hàng trăm GB video lưu trữ để re-train model | Tốn nhiều thời gian cá nhân của CV Intern (3-5 tiếng/tuần); quy trình hiện tại hoàn toàn thủ công; giá trị cải thiện model rõ ràng | Dữ liệu video dung lượng rất lớn, giải pháp tự động có thể cần nhiều tài nguyên lưu trữ và tính toán |

### 2.2. Problem Cards chi tiết (lặp lại cho cả 3 cards)

---

#### Problem Card #1 — Lọc cảnh báo ảo (False Alarm Reduction) trong hệ thống Camera AI giám sát xâm nhập

```text
Problem 1 câu:
Hệ thống AI monitoring phát hiện tức thời theo từng khung hình (frame-by-frame) sinh ra 40-50% cảnh báo ảo do bóng đổ, lá cây hoặc ánh sáng rọi vào vùng cấm, khiến nhân viên an ninh bị quá tải và bỏ qua cảnh báo thật.

Actor:
Nhân viên an ninh trực ca giám sát màn hình & Kỹ sư Computer Vision phụ trách độ chính xác.

Thời điểm / bối cảnh:
Trong suốt ca trực giám sát 24/7 tại các mục tiêu (nhà máy, kho bãi, tòa nhà), đặc biệt nghiêm trọng vào ban đêm hoặc khi thời tiết có mưa gió, rung lắc camera.

Current workflow 3-7 bước:
1. Luồng multi-thread đọc RTSP từ camera IP, giải mã frame và đưa vào Queue.
2. Worker thread chạy model YOLO/Detection dự đoán vị trí bounding box của người/phương tiện.
3. Kiểm tra điều kiện hình học: nếu bounding box chạm hoặc cắt vào vùng đa giác bảo vệ (ROI) thì lập tức tạo Event.
4. Gửi event qua WebSocket lên Web Dashboard, phát âm thanh báo động và mở pop-up video.
5. Nhân viên an ninh nghe chuông, chuyển hướng nhìn sang màn hình, quan sát video stream để xác thực.
6. Nhân viên bấm nút "Xác nhận báo giả" để tắt chuông hoặc kích hoạt quy trình ứng phó nếu có đột nhập thật.

Bottleneck:
Bước 3 & 4: Thuật toán phán đoán tức thời chỉ dựa trên 1 khung hình đơn lẻ (single-frame intersection), không có nhận thức về thời gian lưu lại (dwell time) hay hành vi liên tục, dẫn đến hàng trăm báo động rác mỗi ngày.

Impact:
Trung bình ~55 cảnh báo giả/ca 8 tiếng (trên 10 camera). Nhân viên mất 30-40 phút/ca chỉ để nhìn màn hình tắt cảnh báo giả. Hậu quả nguy hiểm: bảo vệ bị "chai lì phản xạ", từng xảy ra trường hợp tắt loa cảnh báo làm lọt sự cố thật.

Success metric:
- Giảm tỷ lệ báo động giả từ 45% xuống dưới 10% (từ ~55 vụ/ca xuống <12 vụ/ca).
- Giữ vững tỷ lệ phát hiện đột nhập thật (Recall >= 98%).
- Giảm tổng thời gian nhân viên phải can thiệp thủ công cho báo động rác từ 35 phút xuống <8 phút/ca.

Non-AI alternative:
Dùng Rule-based logic lọc thời gian và kích thước:
- Đặt điều kiện đối tượng phải xuất hiện trong ROI liên tục >= 15 frames (tương đương 0.5 - 1 giây).
- Đặt ngưỡng diện tích bounding box tối thiểu/tối đa để lọc lá cây hoặc nhiễu pixel.
- Tự động co hẹp đường biên đa giác ROI cách lề 5-10%.

AI hypothesis:
Sử dụng mô hình phân tích chuỗi chuyển động ngắn (Temporal Tracker / Lightweight Spatial-Temporal model) hoặc VLM mini để xác nhận: "Có đúng là đối tượng người thật đang di chuyển vượt rào/xâm nhập hay chỉ là bóng đổ/nhiễu ngoại cảnh".

Quick gut:
[ ] No AI / process fix
[ ] Rule
[x] Workflow
[ ] Agent
[ ] Chưa biết
```

**Draft workflow Card #1** (ASCII / Mermaid / ảnh đính kèm):

```text
CURRENT STATE — ~45 giây/chu kỳ cảnh báo

[1. Multi-thread nhận frame: 30ms] 
→ [2. Model YOLO detect: 40ms] 
→ [3. Bounding box chạm ROI: 5ms]  <-- bottleneck (nhạy quá mức, 1 frame là bắn alert)
→ [4. WebSocket đẩy chuông & popup lên Web: 200ms] 
→ [5. Bảo vệ nhìn màn hình xác minh mắt thường: 15-30s] 
→ [6. Bảo vệ click bấm tắt báo giả: 5-10s]

FUTURE STATE — ~2 giây (tự động hóa) + 5 giây xác thực khi có biến thật

[1. Multi-thread nhận frame: 30ms] 
→ [2. Model YOLO detect & Track ID: 40ms] 
→ [3. Rule kiểm tra: Object tồn tại trong ROI >= 15 frames: 500ms]
→ [4. AI/Heuristic xác nhận quỹ đạo chuyển động thực (loại bóng đổ/lá cây): 100ms]
→ [5. Phân loại mức độ nguy cơ: Cao / Trung bình / Báo giả tự lọc: 50ms]
→ [6. Đẩy alert nguy cơ cao lên Web, nhân viên chỉ duyệt hành động ứng phó: 5s]  <-- human boundary

Fallback: nếu AI/Rule phân vân (độ tin cậy nằm trong vùng nghi vấn 50-70%), đẩy vào danh sách "Sự kiện cần lưu ý" không reo chuông báo động khẩn cấp, để nhân viên rà soát lại khi rảnh.
```

File đính kèm (nếu vẽ riêng): `01-individual-problem-scan-workflow-card-1.png`

---

#### Problem Card #2 — Lệch đồng bộ (Time-sync mismatch) giữa video stream (Node Media Server) và Bounding Box (WebSocket) trên Web

```text
Problem 1 câu:
Tọa độ Bounding Box gửi qua WebSocket đến trình duyệt trước/sau khung hình video stream từ 300ms - 800ms khiến khung nhận diện chạy lệch khỏi đối tượng trong video, làm giảm độ tin cậy của sản phẩm khi nghiệm thu.

Actor:
Frontend Developer, Kỹ sư Computer Vision, Khách hàng xem trực tiếp giao diện Web.

Thời điểm / bối cảnh:
Xảy ra thường xuyên trong quá trình live-monitoring trên trình duyệt Web, đặc biệt khi mạng nội bộ bị biến động (network jitter) hoặc server encode video bị chậm tải.

Current workflow 3-7 bước:
1. Multi-threading pipeline đọc frame RTSP, gán timestamp hệ thống $T_0$.
2. Luồng 1: Encode video thành h264 và stream RTMP lên Node Media Server.
3. Luồng 2: Model suy luận ra Bounding Box, đóng gói JSON kèm timestamp và gửi qua WebSocket.
4. Node Media Server chuyển tiếp luồng video (FLV/WebRTC) tới trình duyệt web player.
5. Web player render video; cùng lúc đó WebSocket client nhận JSON và vẽ bounding box lên Canvas overlay.
6. Kết quả: Video bị trễ do buffer của player (500ms-1s), trong khi WebSocket đến gần như tức thì, dẫn đến bbox "chạy trước người" 500ms.

Bottleneck:
Bước 4 & 5: Thiếu cơ chế đồng bộ (synchronization buffer) tại frontend dựa trên presentation timestamp (PTS) giữa video tag và canvas overlay.

Impact:
Mỗi tuần tốn 3-4 tiếng họp và debug giữa CV team và Frontend team. Khách hàng đánh giá giải pháp AI "thiếu chính xác, giật cục", ảnh hưởng trực tiếp đến buổi demo nghiệm thu dự án.

Success metric:
- Độ lệch thời gian giữa video frame và bounding box giảm từ 300-800ms xuống <80ms (mắt thường không phân biệt được lệch).
- Tỷ lệ khung hình khớp metadata đạt >= 95% trong điều kiện mạng nội bộ ổn định.

Non-AI alternative:
Giải pháp kỹ thuật thuần túy (Engineering / Rule-based):
- Nhúng trực tiếp timestamp vào SEI (Supplemental Enhancement Information) của luồng video H264 hoặc đóng gói metadata vào từng frame trước khi stream.
- Tạo hàng đợi (queue buffer) ở Frontend: chỉ vẽ Bounding Box khi video player đạt đúng frame timestamp tương ứng.

AI hypothesis:
Không cần áp dụng mô hình AI cho bài toán đồng bộ luồng này; giải pháp phù hợp nhất là chuẩn hóa quy trình kỹ thuật hạ tầng mạng và player.

Quick gut:
[ ] No AI / process fix
[x] Rule
[ ] Workflow
[ ] Agent
[ ] Chưa biết
```

**Draft workflow Card #2:**

```text
CURRENT STATE — Độ lệch 300-800ms

[1. Capture frame (T0)] 
→ Nhánh A: [Encode & Stream lên Node Media Server: trễ buffer ~600ms] → [Web Player Video: T0 + 600ms]  <-- bottleneck
→ Nhánh B: [Inference & Bắn WebSocket JSON: trễ ~80ms] → [Canvas vẽ ngay: T0 + 80ms]
==> Kết quả: Canvas vẽ Bounding box chạy trước Video 520ms.

FUTURE STATE — Độ lệch <80ms (Đồng bộ qua Time-buffer Queue)

[1. Capture frame & nhúng Frame_ID / Timestamp T0]
→ Nhánh A: Stream video lên Web Player
→ Nhánh B: Bắn WebSocket JSON chứa Frame_ID / T0 vào Hàng đợi (Client Ring Buffer)
→ [Frontend Client Hook: Đọc video current PTS] → [Lấy đúng Bounding Box có Timestamp khớp để vẽ Canvas]  <-- human boundary / deterministic rule

Fallback: Nếu network lag làm mất gói WebSocket của frame đó, tự động nội suy vị trí bbox từ frame liền trước hoặc ẩn bbox sau 200ms để tránh treo khung rác trên màn hình.
```

File đính kèm: `01-individual-problem-scan-workflow-card-2.png`

---

#### Problem Card #3 — Trích xuất & gán nhãn tự động các đoạn video sự kiện lỗi (Edge Case Mining) để bổ sung dataset re-train model

```text
Problem 1 câu:
CV Intern phải tua và xem lại thủ công hàng trăm gigabyte video ghi hình mỗi tuần để tìm các tình huống model phát hiện sai hoặc bỏ sót, làm chậm tiến độ cải thiện chất lượng mô hình.

Actor:
Thực tập sinh Computer Vision, Kỹ sư Data/MLOps.

Thời điểm / bối cảnh:
Thực hiện định kỳ mỗi tuần hoặc sau mỗi đợt thử nghiệm tại hiện trường (site trial) để thu thập dữ liệu khó chuẩn bị cho đợt huấn luyện lại (re-train).

Current workflow 3-7 bước:
1. Tải các file video lưu trữ (MP4, độ dài 1-2 tiếng mỗi file) từ máy chủ camera về máy trạm.
2. Mở phần mềm video player, tua nhanh với tốc độ x4 hoặc x8 để dò các đoạn có đối tượng di chuyển.
3. Nhận diện mắt thường các đoạn mô hình bị miss (người đi vào mà không có box) hoặc false (lá cây bị nhận diện là người).
4. Dùng công cụ cắt video (FFmpeg / video editor) trích xuất đoạn clip 10-15 giây chứa lỗi.
5. Đưa các đoạn clip vào phần mềm gán nhãn (CVAT / Label Studio) để gán nhãn lại từng frame.
6. Export dataset mới và chuẩn bị pipeline huấn luyện.

Bottleneck:
Bước 2 & 3: Việc người phải ngồi tua hàng trăm giờ video để tìm vài chục giây lỗi cực kỳ tốn công sức, dễ bỏ sót do mỏi mắt và mất tập trung.

Impact:
Tốn từ 3 đến 5 tiếng/tuần của CV Intern. Tốc độ bổ sung dữ liệu mới bị nghẽn, dẫn đến chu kỳ cập nhật model bị kéo dài (2-3 tuần mới re-train được một lần).

Success metric:
- Giảm thời gian tìm và cắt video lỗi từ 4 tiếng/tuần xuống dưới 45 phút/tuần.
- Tự động phát hiện và gợi ý ít nhất 80% các trường hợp confidence thấp hoặc xung đột track ID để người kiểm duyệt.

Non-AI alternative:
Dùng script lọc theo logic confidence score và track length:
- Trích xuất tự động các đoạn video có bounding box nhưng confidence nằm trong vùng nghi vấn (0.25 - 0.45).
- Lọc các track ID chỉ xuất hiện thoáng qua dưới 5 frame rồi biến mất.

AI hypothesis:
Sử dụng mô hình phát hiện bất thường (Anomaly Detection) hoặc Zero-shot Detector mạnh hơn (chạy offline không cần realtime) để quét video và gắn cờ những đoạn có sự chênh lệch lớn giữa mô hình edge và mô hình offline server.

Quick gut:
[ ] No AI / process fix
[ ] Rule
[x] Workflow
[ ] Agent
[ ] Chưa biết
```

**Draft workflow Card #3:**

```text
CURRENT STATE — 240 phút/tuần

[1. Tải video thô từ server: 30'] 
→ [2. Ngồi tua x4/x8 tìm đoạn lỗi bằng mắt: 150']  <-- bottleneck
→ [3. Cắt clip bằng FFmpeg thủ công: 30'] 
→ [4. Import vào CVAT: 15'] 
→ [5. Bắt đầu gán nhãn: 15']

FUTURE STATE — 35 phút/tuần

[1. Script tự động quét log metadata & lọc clip nghi vấn (confidence 0.25 - 0.45): 5']
→ [2. Offline Model đối soát tự động đánh dấu đoạn miss/false detection: 10' (chạy nền)]
→ [3. Tự động cắt clip 10s và đẩy sẵn draft annotations vào CVAT: 5']
→ [4. CV Engineer mở CVAT duyệt/sửa nhãn nhanh (Human-in-the-loop): 15']  <-- human boundary

Fallback: Nếu script lọc bỏ sót, định kỳ lấy mẫu ngẫu nhiên (random sampling) 5% thời lượng video để người kiểm tra xác suất.
```

File đính kèm: `01-individual-problem-scan-workflow-card-3.png`

---

### 2.3. Card muốn pitch nhất (chuẩn bị 2 phút)

**Card tôi muốn pitch nhất:**

```text
Problem Card #1 — Lọc cảnh báo ảo (False Alarm Reduction) trong hệ thống Camera AI giám sát xâm nhập.
```

**Vì sao (2-3 câu: workflow gì, số đo gì, impact gì):**

```text
Đây là "nỗi đau" lớn nhất và thực tế nhất của cả người dùng cuối (bảo vệ trực ban) lẫn đội ngũ phát triển tại công ty em. Workflow từ camera stream đến dashboard rất rõ ràng, điểm nghẽn nằm ở việc phán đoán frame đơn lẻ gây ra 40-50% cảnh báo giả. Việc giải quyết bài toán này có số đo tác động trực tiếp: giảm số lần chuông reo oan từ ~55 lần/ca xuống dưới 12 lần/ca, giúp người trực không bị chai lì phản xạ khi có sự cố thật.
```

**Câu hỏi tôi muốn nhóm challenge (1-2 câu hỏi đúng chỗ yếu):**

```text
1. "Nếu nhóm mình thêm bước xác thực theo chuỗi thời gian (chờ 15-20 frames) hoặc thêm model kiểm chứng, liệu độ trễ cảnh báo (latency) có bị tăng từ <500ms lên 1-2 giây không, và độ trễ đó có chấp nhận được trong kịch bản an ninh không?"
2. "Bài toán này giải bằng Rule-based thuần (tracking đếm frame + ngưỡng diện tích) đã đủ tốt chưa, hay bắt buộc phải có thêm một tầng AI Workflow để phân tích ngữ cảnh?"
```

**AI phản biện Card (nếu có):**
- Điểm yếu AI chỉ ra: AI ban đầu đề xuất dùng một mô hình Vision-Language (VLM) lớn để "nhìn và giải thích lý do xâm nhập". Tuy nhiên trong thực tế triển khai Camera AI giám sát, tài nguyên máy chủ edge rất giới hạn, việc gọi mô hình lớn sẽ làm sập pipeline multi-threading và không thể đáp ứng thời gian thực.
- Tôi sửa gì: Tôi đã điều chỉnh lại phạm vi giải pháp: Không dùng VLM cồng kềnh, mà thiết kế một **Workflow 2 tầng**: Tầng 1 dùng Rule & Tracking đếm frame để lọc ngay 70% nhiễu; Tầng 2 chỉ dùng thuật toán phân tích quỹ đạo vector chuyển động nhẹ (Lightweight Motion/Spatial-Temporal heuristic). Con người vẫn là chốt chặn cuối cùng đưa ra quyết định xử lý thực địa.

### Self-check nộp phần 01
- [x] Có 5+ problems + top 3 Cards đủ field
- [x] Mỗi Card có workflow trước/sau + bottleneck + metric + fallback
- [x] Đã chọn 1 card pitch + câu hỏi challenge
