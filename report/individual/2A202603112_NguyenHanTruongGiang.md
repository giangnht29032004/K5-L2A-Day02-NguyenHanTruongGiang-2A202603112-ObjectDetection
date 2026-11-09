# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Nguyễn Hán Trường Giang
- MSSV: 2A202603112
- Email: 26ai.giangnht@vinuni.edu.vn
- Lớp: L2A
- Link repo bài làm: https://github.com/giangnht29032004/K5-L2A-Day02-NguyenHanTruongGiang-2A202603112-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1NyqnBEqTDD90ruxYXA-oob64GxKaLcnF#scrollTo=okV4R-SDkjOD

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý: Không có
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: `drive_001.jpg`, `drive_002.jpg`, `drive_003.jpg`, `drive_004.jpg`, `drive_005.jpg`, `drive_006.jpg`, `drive_007.jpg`, `drive_008.jpg`, `drive_009.jpg`, `drive_010.jpg`
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Không
- Commit khóa nhãn ban đầu:

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP |447 | |
| FP |493 | |
| FN |206 | |
| Precision |0.4755 | |
| Recall |0.6845 | |
| F1 |0.5612 | |
| mean_iou_of_matches |0.843 | |
| passed |failed | |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Ultralytics 8.4.208 🚀 Python-3.13.16 torch-2.14.1+cu130 CUDA:0 (Tesla T4, 14913MiB)
- Các tham số thay đổi so với notebook nếu có và lý do: Không
- Đường dẫn best checkpoint trong phiên chạy: /content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt"

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision |0.3003902265842099 |
| Recall |0.28315602836879433 |
| mAP50 |0.27109236388622476 |
| mAP50–95 |0.20031257579398093 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
Class van có recall thấp nhất, bằng 0% theo confusion matrix. Trong 12 đối tượng van thuộc tập validation, có 11 đối tượng bị nhận diện nhầm thành bus và 1 đối tượng bị bỏ sót. Ảnh drive_056.jpg trong assets/eval_predictions/visuals/ có nhiều phương tiện dạng van/minivan, minh họa sự khó khăn khi phân biệt các phương tiện có hình dáng tương tự. Confusion matrix là dữ kiện định lượng cho thấy vấn đề nhầm lẫn giữa hai lớp.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
Cần kiểm tra độ chính xác hình học của bounding box, bao gồm vị trí, kích thước và mức độ bao sát vật thể. mAP50 sử dụng ngưỡng IoU 0.50, trong khi mAP50–95 đánh giá ở nhiều ngưỡng IoU từ 0.50 đến 0.95. Nếu box nhận diện đúng vật thể nhưng chưa bao sát, bị lệch hoặc chứa quá nhiều nền, kết quả ở các ngưỡng IoU cao sẽ giảm. Vì vậy cần kiểm tra các cạnh box, đặc biệt ở những phương tiện bị che khuất hoặc chồng lấn.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
Lỗi đáng chú ý là nhầm lẫn giữa van và bus. Confusion matrix cho thấy 11/12 đối tượng van bị dự đoán thành bus, tương đương khoảng 91.7%. Điều này có thể liên quan đến việc tiêu chí phân biệt van/minivan và minibus chưa đủ rõ ràng trong guideline. Cần làm rõ rằng bus bao gồm xe buýt và minibus chở khách, còn van bao gồm xe van/minivan chở hàng hoặc chở người. Tuy nhiên, confusion matrix chưa đủ để kết luận guideline hoặc nhãn train sai; cần kiểm tra ảnh và nhãn thực tế trước khi quyết định rework.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
Không sử dụng 10 ảnh eval để train nhằm tránh data leakage và đảm bảo chúng không tham gia cập nhật trọng số của mô hình. Trong notebook, 10 ảnh này được dùng làm validation set để theo dõi hiệu suất, tính các chỉ số Precision, Recall, mAP50, mAP50–95 và lựa chọn checkpoint best.pt. Vì vậy, eval không phải test set độc lập do đã được sử dụng trong quá trình lựa chọn mô hình. Muốn đánh giá khách quan hơn cần có một tập test riêng chưa được sử dụng trong training và validation.
## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| | | | |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
Sử dụng ChatGPT để trả lời bốn câu hỏi cuối notebook ở mục 5.
