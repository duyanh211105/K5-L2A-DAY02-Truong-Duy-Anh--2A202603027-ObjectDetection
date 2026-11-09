# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Trương Duy Anh
- MSSV: 2A202603027
- Email: duyanh211105@gmail.com
- Lớp:L2A
- Link repo bài làm: https://github.com/duyanh211105/K5-L2A-DAY02-Truong-Duy-Anh--2A202603027-ObjectDetection
- Link Colab đã chạy:  https://colab.research.google.com/github/duyanh211105/K5-L2A-DAY02-Truong-Duy-Anh--2A202603027-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](https://github.com/duyanh211105/K5-L2A-DAY02-Truong-Duy-Anh--2A202603027-ObjectDetection/blob/main/GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.


## 3. Annotation và self-QC

- Số file nhãn train: 50 file trong `dataset/labels/train/`, mỗi file cùng basename với một ảnh train (`drive_001.txt` … `drive_050.txt`).
- Kết quả checker và lỗi đã xử lý: `python tools/check_yolo_labels.py` báo "ĐẠT cấu trúc: 50/50 file nhãn hợp lệ". Lỗi đã xử lý trước khi chạy checker: CVAT xuất thứ tự lớp là bus, car, van, truck (ID 0–3), lệch với schema của đề (car=0, truck=1, bus=2, van=3); đã dùng script đổi ID theo ánh xạ {0→2, 1→0, 2→3, 3→1} cho cả 50 file rồi mới chạy checker. Ngoài ra, lần export đầu ra file nhãn rỗng do chưa lưu trong CVAT; đã lưu  rồi export lại.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt:  ví dụ: drive_001, drive_005, drive_009, drive_013, drive_018, drive_022, drive_027, drive_033, drive_041, drive_048
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Box ban đầu được tạo bằng model YOLO11s (COCO) rồi soát tay toàn bộ 50 ảnh. Các lỗi đã gặp và sửa:  "drive_0xx: thêm 3 xe nhỏ ở xa bị model bỏ sót; drive_0xx: đổi xe minivan từ car sang van; drive_0xx: đổi xe bán tải từ truck về car; drive_0xx: xóa box trùng và kéo sát box bị lỏng".
- Commit khóa nhãn ban đầu: `1fcd613` ("Khoa nhan ban dau").
## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 418 | [ĐIỀN] |
| FP | 428 | [ĐIỀN] |
| FN | 235 | [ĐIỀN] |
| Precision | 0.4941 | [ĐIỀN] |
| Recall | 0.6401 | [ĐIỀN] |
| F1 | 0.5577 | [ĐIỀN] |
| mean_iou_of_matches | 0.8500 | [ĐIỀN] |
| passed | false | [ĐIỀN] |


Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Google Colab, GPU Tesla T4 (15 GB), Ultralytics 8.4.208, Torch 2.14.1+cu130, Python 3.13.16.
- Các tham số thay đổi so với notebook nếu có và lý do: Không thay đổi; giữ nguyên `yolo26n.pt`, 50 epoch, imgsz 640, batch tự động, seed 20260909.
- Đường dẫn best checkpoint trong phiên chạy: `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.409 |
| Recall | 0.234 |
| mAP50 | 0.271 |
| mAP50–95 | 0.200 |

Chi tiết theo lớp:

| Lớp | Số xe trong eval | Precision | Recall | mAP50 | mAP50–95 |
|---|---:|---:|---:|---:|---:|
| car | 94 | 0.836 | 0.652 | 0.740 | 0.524 |
| truck | 5 | 0.663 | 0.200 | 0.240 | 0.206 |
| bus | 2 | 0.000 | 0.000 | 0.014 | 0.005 |
| van | 12 | 0.136 | 0.083 | 0.089 | 0.064 |

Đặt cả 2 folder ` assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?

Lớp bus có recall thấp nhất (0.000, eval chỉ có 2 xe bus), tiếp theo là van (0.083, bắt được khoảng 1/12 xe) và truck (0.200, 1/5 xe). Ngược lại car đạt recall 0.652 với 94 xe. Nguyên nhân chính là mất cân bằng lớp: tập train chỉ 50 ảnh và phần lớn box là car, nên model có rất ít ví dụ để học bus, van, truck. Ví dụ ở drive_052, chiếc xe buýt hai tầng chiếm gần cả khung hình nhưng model chỉ đưa ra bus với độ tin cậy 0.49, thấp so với các xe con quanh đó. Ngoài ra, ở các ảnh đông xe như drive_058, nhiều xe con nhỏ ở xa không có box dự đoán, làm recall của car cũng chưa cao.

2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?

mAP50 chỉ yêu cầu box dự đoán trùng box chuẩn từ 50% trở lên, còn mAP50–95 lấy trung bình ở các ngưỡng khắt khe hơn tới 95%. Nếu mAP50 cao mà mAP50–95 thấp nghĩa là model tìm đúng xe nhưng box chưa sát: cần kiểm tra các cạnh box có rộng hơn thân xe, lấn ra mặt đường, hay hụt mất phần xe bị che hoặc bị cắt ở rìa ảnh không. Với lớp car của em, mAP50 là 0.740 và mAP50–95 là 0.524, chênh lệch ở mức vừa phải: box nhìn chung đúng vị trí nhưng chưa thật sát, đặc biệt với xe nhỏ ở xa nơi chỉ lệch vài pixel đã làm IoU giảm mạnh. Điều này cũng liên quan trực tiếp đến độ sát của box khi gán nhãn train.

3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?

Lỗi rõ nhất là nhầm car thành van. Ở drive_051, chiếc sedan màu đỏ bị đoán là van với độ tin cậy 0.99; ở drive_054, một xe con màu đỏ khác cũng bị đoán van 0.97. Model tự tin đến mức này với một xe con rõ ràng gợi ý rằng trong nhãn train có những xe con đang bị gán van, tức ranh giới car/van trong guideline chưa được áp dụng nhất quán khi gán nhãn. Dữ kiện hỗ trợ: precision của van chỉ 0.136, nghĩa là phần lớn dự đoán van là sai. . Hướng xử lý là rà lại các box van trong 50 ảnh train, đối chiếu với định nghĩa trong GUIDELINE_MINI.md (thân hộp cao, đầu xe ngắn), sửa các xe con bị gán nhầm rồi train lại.

4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?

Mục đích của tập eval là đo model hoạt động thế nào trên ảnh nó chưa từng học. Nếu đưa 10 ảnh này vào train, model sẽ “học thuộc” chúng và chỉ số đánh giá bị cao ảo, không còn phản ánh khả năng thật trên ảnh mới. Trong notebook, data.yaml đặt 10 ảnh eval làm tập val: sau mỗi epoch, model được đánh giá trên tập này và epoch tốt nhất được lưu thành best.pt. Vì eval đã tham gia vào việc chọn checkpoint, kết quả cuối cùng hơi lạc quan hơn thực tế và tập eval không phải test set hoàn toàn độc lập; muốn đánh giá khách quan cần một tập test riêng chưa dùng ở bất kỳ bước nào.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Khởi động CVAT | Không có container CVAT nào chạy dù image vẫn còn | Chạy lại `docker compose up -d` trong thư mục cvat | 19 container lên bình thường |
| Tạo task CVAT | "Network error. The CVAT server is not reachable" | Kiểm tra log `cvat_server`, chỉ upload đúng 50 ảnh trong `images/train` | Tạo task thành công |
| Gán nhãn | Export lần đầu ra file nhãn rỗng | Lưu nhãn bằng Ctrl+S trước khi export | Export lại đủ nhãn |
| Nạp nhãn pre-label | Task mới báo "No image found" | Xóa task lỗi, dùng Upload annotations vào task có ảnh | Nạp thành công |
| Export cuối | Thứ tự lớp CVAT lệch schema | Script đổi ID dựa trên `obj.names` | 50 file, checker đạt 50/50 |
| Chạy script đổi ID | FileNotFoundError khi chạy trên Colab với đường dẫn ổ C | Upload zip export lên Colab, xử lý rồi tải về | Thành công |
| Notebook Colab | Clone repo lỗi (exit 128) do sai tên repo trong `REPO_URL` | Sửa đúng tên repo, xóa thư mục clone lỗi, chạy lại | Train 50 epoch trên GPU T4 thành công |
| Đưa kết quả vào repo | Lỡ kéo file lẻ của Colab vào thư mục gốc và assets | Xóa file lẻ, giải nén đúng `day2_colab_outputs.zip` vào `assets/` | Đúng cấu trúc, không commit `.pt` |
| Gold QC | F1 0.5577 < 0.80 (mean IoU 0.85 đạt) | Script chẩn đoán tách lỗi sai lớp / box thừa / xe sót; vẽ box so sánh lên ảnh | [ĐIỀN kết quả rework] |

## 7. Tự đánh giá theo rubric

- Hoàn thành và định dạng YOLO: đạt, checker 50/50, ID đúng schema.
- Độ bám sát box: mean IoU của các box khớp là 0.85, trên ngưỡng xuất sắc (0.80).
- Đầy đủ vật thể: lần chấm đầu còn 124 xe bị sót. [ĐIỀN sau rework]
- Phân lớp nhất quán: chưa tốt ở lần đầu, có nhóm lỗi lặp lại giữa bus/van/truck và 317 box thừa. [ĐIỀN sau rework]
- Mini guideline: [ĐIỀN: có định nghĩa lớp, quy tắc box, 3 ca mơ hồ; đã cập nhật quy ước sau gold QC nếu có]
- Gold-set QC: lần đầu chưa đạt (F1 0.5577); đã phân tích nguyên nhân và rework. [ĐIỀN kết quả cuối]
- Pipeline YOLO: Colab chạy được, giữ đúng split (eval không dùng train), có metrics, ảnh dự đoán và phân tích lỗi.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng : claude , dùng ở bước nào và phần đã tự kiểm tra : bước 5 ; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train. 
