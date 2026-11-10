# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Trương Thành Đức  
- MSSV:2A202603034
- Email: truongthanhduc270998@gmail.com
- Lớp: L2A
- Link repo bài làm: https://github.com/thanhduc270998/K5-L2A-Day02-TruongThanhDuc-2A202603034-ObjectDetection.git
- Link Colab đã chạy: https://colab.research.google.com/drive/11M7W176d2kPIF8sQIr3yms7EFOjDvWbv?usp=sharing

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train:
- Kết quả checker và lỗi đã xử lý:
- Danh sách ít nhất 10 ảnh đã rà bằng mắt:
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:
- Commit khóa nhãn ban đầu:

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
Lớp bus có Recall thấp nhất. Ảnh minh hoạt: drive_052.jpg, drive_053.jpg, drive_056.jpg, drive_058.jpg, drive_060.jpg.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
Kiểm tra Bounding Box có bị lệch, quá rộng, quá hẹp hoặc chưa bao sát vật thể không.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
Các lớp truck, bus, van có thể bị nhầm lẫn do hình dáng tương đồng.
Ảnh cần kiểm tra: drive_053.jpg, drive_059.jpg, drive_060.jpg
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
Không dùng 10 ảnh Eval để cập nhật trọng số nhằm tránh rò rỉ dữ liệu đánh giá.

Trong notebook, 10 ảnh Eval được dùng làm validation để theo dõi hiệu suất và chọn checkpoint tốt nhất

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| | | | |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
