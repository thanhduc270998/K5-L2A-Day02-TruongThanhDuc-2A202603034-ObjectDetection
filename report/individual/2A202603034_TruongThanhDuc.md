# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Trương Thành Đức  
- MSSV:2A202603034
- Email: 26ai.ductt2@vinuni.edu.vn
- Lớp: L2A
- Link repo bài làm: https://github.com/thanhduc270998/K5-L2A-Day02-TruongThanhDuc-2A202603034-ObjectDetection.git
- Link Colab đã chạy: https://colab.research.google.com/drive/1DjgzAnu9L3Yow2EVG6_PMJ-3OtZLUXa8?usp=sharing

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50 file
- Kết quả checker và lỗi đã xử lý: 
    + python3 tools/check_yolo_labels.py cho ra: "ĐẠT cấu trúc: 50/50 file nhãn hợp lệ". Checker không báo lỗi định dạng nào cần sửa.
    + Checker kiểm theo cấu trúc (5 cột, toạ độ normalized, class ID trong khoảng 0–3).
- Danh sách ít nhất 10 ảnh đã rà bằng mắt:
    + drive_001, drive_002, drive_003, drive_004, drive_005, drive_006, drive_007, drive_008, drive_009, drive_010
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:
    + Đã thêm box bị bỏ sót.
    + Đã xóa Box gán sai.
    + Một số box đánh nhãn sai đã được sửa cho đúng
- Commit khóa nhãn ban đầu:3e604b4

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 450 | |
| FP | 442 | |
| FN | 203 | |
| Precision | 0.5045 | |
| Recall | 0.6891 | |
| F1 | 0.5825 | |
| mean_iou_of_matches | 0.8462 | |
| passed | false | true (vì lab coach chỉnh yêu cầu hoàn thành F1 từ 0.7 xuống còn từ 0.5 là đạt yêu cầu)|

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: 
Runtime: Google Colab, Python 3.13.16.
GPU: NVIDIA Tesla T4, CUDA 13.0.
Ultralytics: 8.4.208.
PyTorch: 2.14.1+cu130.
Mô hình sử dụng: YOLO26n (yolo26n.pt).
- Các tham số thay đổi so với notebook nếu có và lý do:
Sử dụng mô hình YOLO26n pretrained để phù hợp với quy mô dữ liệu nhỏ.
Epochs: 50.
Image size: 640.
Batch size: -1 (tự động lựa chọn theo bộ nhớ GPU).
Seed: 20260909.
Sử dụng GPU để tăng tốc quá trình huấn luyện.

- Đường dẫn best checkpoint trong phiên chạy:
/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.4105 |
| Recall | 0.2303 |
| mAP50 | 0.2723 |
| mAP50–95 | 0.2017 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
- Lớp bus có Recall thấp nhất. Ảnh minh hoạt: drive_052.jpg, drive_053.jpg, drive_056.jpg, drive_058.jpg, drive_060.jpg.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
- Kiểm tra Bounding Box có bị lệch, quá rộng, quá hẹp hoặc chưa bao sát vật thể không.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
- Các lớp truck, bus, van có thể bị nhầm lẫn do hình dáng tương đồng.
- Ảnh cần kiểm tra: drive_053.jpg, drive_059.jpg, drive_060.jpg
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
- Không dùng 10 ảnh Eval để cập nhật trọng số nhằm tránh rò rỉ dữ liệu đánh giá.
- Trong notebook, 10 ảnh Eval được dùng làm validation để theo dõi hiệu suất và chọn checkpoint tốt nhất

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Xử lý phương tiện bị che khuất | Một số xe nằm chồng lên nhau, điển hình tại drive_009, gây khó khăn trong việc xác định phạm vi bounding box cần gán. | Thống nhất quy tắc sử dụng bounding box ước lượng toàn bộ phương tiện thay vì chỉ bao quanh phần nhìn thấy. Quy tắc này được bổ sung vào guideline để đảm bảo tính nhất quán. | Tại drive_009, chỉ có 8/21 box khớp với nhãn gold dù số lượng box dự đoán gần tương đương (28 box). Cần kiểm tra trực quan để xác định nguyên nhân do vị trí box hay do khác biệt trong quy ước gán nhãn. |
| Kiểm tra dữ liệu sau khi xuất từ CVAT | Phát hiện thứ tự các class giữa obj.names và data.yaml không đồng nhất. CVAT sử dụng car, bus, van, truck, trong khi cấu hình YOLO sử dụng car, truck, bus, van. | Đối chiếu file labels.jpg và confusion matrix để nhận diện sự không thống nhất về class ID. | Checker vẫn thông báo ĐẠT vì chỉ kiểm tra cấu trúc dữ liệu, không xác minh ý nghĩa thực tế của từng class ID. Cần ánh xạ lại ID nhãn theo đúng thứ tự class trước khi huấn luyện. |
| Rà soát xe ở khoảng cách xa | Các phương tiện ở xa có kích thước rất nhỏ, chỉ khoảng 8–20 px, khiến việc xác định chính xác tọa độ bounding box gặp khó khăn. Sai lệch vài pixel cũng ảnh hưởng đáng kể đến IoU. | Sử dụng chức năng phóng to ảnh trên CVAT để xác định vị trí phương tiện rõ hơn. Bổ sung quy định trong guideline: vẫn gán nhãn đối với xe có kích thước từ khoảng 8 px nếu có thể nhận diện được. | Mean IoU chung đạt 0.8462. Tuy nhiên, các box của xe nhỏ vẫn có độ chính xác chưa cao, có thể ảnh hưởng đến kết quả đánh giá tại các ngưỡng IoU nghiêm ngặt như mAP50–95.|

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.
| Tiêu chí (điểm) | Bằng chứng hiện có | Đánh giá |
|---|---|---|
| Hoàn thành và định dạng YOLO (10) | `check_yolo_labels.py`: "ĐẠT cấu trúc: 50/50 file nhãn hợp lệ". | 50 file cùng basename, 5 cột, normalized, bốn class ID đúng |
| Đầy đủ vật thể (20) | Recall gold 0.2303; gán cả xe bị che và xe bị cắt rìa. | Còn 203 FN. Một số ảnh khớp ít như `drive_030` (13/20), `drive_027` (17/31), chưa rà nguyên nhân. |
| Độ bám sát và hình học box (20) | mean IoU of matches 0.8462 (≥ 0.70, gần mức "xuất sắc" 0.80). | Đạt theo số liệu. Có box xe nhỏ ở xa lệch 2–4 px. |
| Mini guideline và quyết định mơ hồ (10) | `GUIDELINE_MINI.md` có định nghĩa lớp, quy tắc box và 3 ca mơ hồ có lý do. | Đủ yêu cầu. |
| Gold-set annotation QC (15) | `annotation_evaluation.json`: F1 0.5825, mean IoU 0.8462. | **Vừa đủ**:  0.50 < F1 < 0.80 và chưa có vòng rework. |
| Pipeline YOLO và phân tích (10) | Colab chạy xong 50 epoch; không dùng eval để train; có metrics, ảnh eval và phân tích lỗi (mục 5). | Có kết quả. Cần train lại sau khi sửa class ID để số liệu theo lớp có ý nghĩa. |

**Điều kiện bắt buộc:**
- Có đủ 50 file nhãn.
- Có gold-set evaluation.
- Không dùng eval để train.
- Không thay đổi gold labels.
- Đã chạy qua `check_yolo_labels.py` (50/50 hợp lệ)

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.

- Công cụ AI: ChatGPT (OpenAI), được sử dụng trong các công việc sau:

- Hỗ trợ xây dựng tài liệu:
    + Hỗ trợ soạn thảo và hoàn thiện nội dung báo cáo quá trình gán nhãn dữ liệu và huấn luyện mô hình YOLO.
    + Hỗ trợ tổng hợp kết quả kiểm tra dữ liệu train/eval, đánh giá chất lượng nhãn và các vấn đề phát sinh trong quá trình thực hiện.
    + Hỗ trợ trình bày các lỗi liên quan đến bounding box, class ID và đề xuất hướng khắc phục.
- Phân tích kết quả thực nghiệm:
    + Hỗ trợ đọc và phân tích các kết quả đánh giá mô hình YOLO.
    + Phân tích confusion matrix, các chỉ số Precision, Recall và mAP theo từng lớp.
    + Hỗ trợ nhận diện vấn đề không đồng nhất thứ tự class giữa nhãn xuất từ CVAT và file cấu hình data.yaml.
    + Đề xuất quy trình remap class ID, kiểm tra lại nhãn và huấn luyện mô hình sau khi chuẩn hóa dữ liệu.
- Công cụ và tài liệu kỹ thuật tham khảo:
    + Ultralytics YOLO: Tài liệu hướng dẫn sử dụng, cấu hình và huấn luyện mô hình phát hiện đối tượng: Ultralytics Documentation.
    + CVAT: Công cụ gán nhãn ảnh và quản lý bounding box: CVAT Documentation.
    + GitHub: Quản lý mã nguồn, dữ liệu nhãn và các phiên bản thực hiện dự án.