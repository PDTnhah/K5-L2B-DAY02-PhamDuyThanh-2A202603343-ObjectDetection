# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Phạm Duy Thanh
- MSSV: 2A202603343
- Email: 26ai.thanhpd3@vinuni.edu.vn
- Lớp: L2B
- Link repo bài làm: https://github.com/PDTnhah/K5-L2B-DAY02-PhamDuyThanh-2A202603343-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1pIZsLYHBI6dtsVTO5pHh0AaCETyHwslu#scrollTo=0f04d1c9

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_004` | xe bị che bới rìa ảnh, góc chụp từ trên cao và đang di chuyển nên hơi mờ, không phân biệt rõ car hay van | Gán nhãn `car` | phán đoán dáng xe có vẻ nhỏ hơn 1 con xe van, khá giống xe bán tải hơn |
| `drive_014` | xe `truck` chở đồ nhiều trên thùng, cao hơn đầu xe, không biết nên vẽ khung đến mức nào | chỉ vẽ khung cao bằng đầu xe | tập trung khung vào xe `truck` hơn |
| `drive_020` | Xe trắng nhỏ, đầu xe khá giống với xe `van`, nhưng kích cỡ lại khá nhỏ, khó phân biệt với `car` | gán nhãn `car` | chọn `car` vì kích thước nhỏ hơn van bình thường, có thể chỉ là xe cá nhân |

Phần cần giảng viên làm rõ:
- Ở drive_021 có 1 số xe `car` chị che bởi chỗ chờ xe buýt nhưng vẫn nhìn được ra thì có gán nhãn không mặc dù bị che khá nhiều 
- Ở drive_042, xe chở bao hàng chồng đầy thì có gán khung toàn bộ đống hàng đó không

## 3. Annotation và self-QC

- Số file nhãn train: 50/50 file (`dataset/labels/train/drive_001.txt` … `drive_050.txt`), không có file rỗng
- Kết quả checker và lỗi đã xử lý: `python tools/check_yolo_labels.py` →
  `ĐẠT cấu trúc: 50/50 file nhãn hợp lệ.` Không có lỗi định dạng, ID lớp hay box vượt biên.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_001, drive_002, drive_003, drive_004, drive_005, drive_006, drive_007, drive_008, drive_009, drive_010, drive_011, drive_012, drive_013, drive_014, drive_015, drive_041, drive_050
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:
  - Một số khung nhãn ở drive_001 - 003 đang gán cho một số xe ở khá xa và mờ -> bỏ khung đi
  - `drive_014` khung chưa bao trọn xe đen -> kéo cho đủ bao trọn xe
  - `drive_041`, `drive_050` thiếu nhiều nhãn -> tiếp tục gán vào
- Commit khóa nhãn ban đầu: 7f7571af337bfcdb8ceed5dd638b1d5677f0843b

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 425 | 438 |
| FP | 357 | 350 |
| FN | 228 | 215 |
| Precision | 0.5435 | 0.5558 |
| Recall | 0.6508 | 0.6708 |
| F1 | 0.5923 | 0.6079 |
| mean_iou_of_matches | 0.8614 | 0.8584 |
| passed | false | false |

Rework:
  - Một số khung nhãn ở drive_001 - 003 đang gán cho một số xe ở khá xa và mờ -> bỏ khung đi
  - `drive_014` khung chưa bao trọn xe đen -> kéo cho đủ bao trọn xe
  - `drive_041`, `drive_050` thiếu nhiều nhãn -> tiếp tục gán vào
  
## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Google Colab, GPU Tesla T4 ; Python 3.13.16, Ultralytics 8.4.208, Torch 2.14.1+cu130. Model YOLO26n (fused: 121 layers, 2,390,464 tham số, 5.4 GFLOPs).
- Các tham số thay đổi so với notebook nếu có và lý do: không thay đổi, giữ nguyên toàn bộ tham số mặc định của notebook.
- Đường dẫn best checkpoint trong phiên chạy: `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.4105453698519763 |
| Recall | 0.23026854853628678 |
| mAP50 | 0.27225886498171903 |
| mAP50–95 | 0.20168300309748077 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?

   `bus` thấp nhất: recall 0.000, mAP50 0.008 (2 instance).
   - [drive_051](assets/eval_predictions/visuals/drive_051.jpg): nhãn eval có 1 box ID 2 (`bus` theo `data.yaml`) ở góc trên phải, nhưng nhìn ảnh thì đó là xe tải thùng trắng, không phải xe buýt. Đây cũng là lệch ID lớp. Model không có box nào trên xe này nên bị tính FN. Ngoài ra, sedan đỏ ở tiền cảnh bị đoán `van 0.99`, trong khi nhãn eval là `car`.
   - [drive_058](assets/eval_predictions/visuals/drive_058.jpg): ba xe buýt bên trái được đoán `bus` (0.83/0.47/0.27) nhưng nhãn eval là `van`, nên bị tính thành FP `bus` + FN `van`.

2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?

   Model tìm đúng xe nhưng cạnh box chưa khớp ở IoU chặt (0.75–0.95). Cần kiểm: box lỏng hay hụt (lấy cả bóng đổ/mặt đường, hoặc cắt mất đầu/đuôi xe), xe nhỏ ở xa, xe bị che/cắt khung có được vẽ nhất quán không; độ sát của nhãn train (mean IoU so với gold là 0.858).

3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?

   Lỗi: nhầm lẫn giữa `car` ↔ `van` (MPV/SUV 7 chỗ hình hộp với minivan) và giữa `van` ↔ `bus` ↔ `truck` (van cỡ lớn, xe buýt, xe tải thùng kín).

   Dữ kiện hỗ trợ:
   - Góc camera CCTV trên cao: nhìn từ trên xuống, MPV, minivan và xe tải nhỏ ở xa có hình dáng rất giống nhau. Nếu guideline không quy định rõ thì rất dễ gán lệch. Ví dụ: MPV trắng nóc cao ở drive_004 tôi gán `car`, nhưng gold xếp nó cùng lớp với chiếc van bên cạnh. Trên eval, model cũng đoán sedan đỏ ở drive_051 là `van 0.99`.
   - `van` / `bus` / `truck` lệch có quy luật so với data gold

4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?

   Eval phải là ảnh model chưa thấy để đo khả năng tổng quát hóa. Train trên đó là rò rỉ dữ liệu: metric bị thổi phồng và che mất overfit, trong khi 50 ảnh train vốn đã dễ overfit. Trong notebook, `data.yaml` đặt `val: images/eval`, nên sau mỗi epoch model được validate trên 10 ảnh này ([results.csv](assets/yolo26_runs/vehicle_annotation/results.csv)) để chọn `best.pt` (chủ yếu theo mAP50–95), rồi được đánh giá lại trên chính tập này. Vì vậy eval không phải test set độc lập và số liệu có phần lạc quan.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Gán nhãn — phân lớp | `drive_004`, `drive_020`: xe trắng nhỏ, đầu xe giống `van` nhưng kích thước nhỏ, khó tách `car`/`van` | Áp guideline (nóc, capo, kích thước), chọn `car`, ghi vào bảng ca mơ hồ | Còn mơ hồ: so với gold, MPV nóc cao ở `drive_004` được gán như `van` |
| Gán nhãn — vẽ box | `drive_014`, `drive_042`: xe tải chở hàng chất cao hơn cabin; `drive_021`: nhiều `car` bị nhà chờ xe buýt che khá nhiều | `drive_014` chỉ vẽ box cao bằng đầu xe; các xe bị che nhưng còn nhận ra thì vẫn gán | Chưa chắc đúng quy ước, đã ghi vào “Phần cần giảng viên làm rõ” |
| Self-QC | Thừa box ở xe xa/mờ (`drive_001`–`003`); thiếu nhãn ở `drive_041`, `drive_050`; box chưa bao trọn xe đen ở `drive_014` | Bỏ box xe xa mờ, gán thêm xe còn thiếu, kéo lại cạnh box | Checker vẫn đạt 50/50 |


## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md):

| Tiêu chí (điểm) | Kết quả / bằng chứng | Còn thiếu / chưa kiểm được | Tự đánh giá |
|---|---|---|---|
| Hoàn thành và định dạng YOLO (10) | 50/50 file đúng basename, 5 cột, tọa độ chuẩn hóa, class ID 0–3 theo `data.yaml`; `check_yolo_labels.py` đạt 50/50 (commit `6559507`, đã push) | — | Đạt |
| Đầy đủ vật thể (20) | Recall so với gold 0.6708 (FN 215). Nếu bỏ qua lệch ID lớp, recall là 0.827 (FN 113). Đã gán bổ sung ở `drive_041`, `drive_050` | Vẫn thiếu nhiều xe ở `drive_024`, `drive_027`, `drive_041`; chưa rà hết 50 ảnh | Đạt một phần |
| Độ bám sát và hình học box (20) | mean IoU của matches 0.8584 (≥ 0.70, và ≥ 0.80 của mức xuất sắc); đã sửa box chưa bao trọn ở `drive_014` | Box thừa ở xe xa/mờ (`drive_002`, `drive_003`); chưa kiểm riêng lỗi gộp hai xe vào một box | Đạt phần lớn |
| Phân lớp nhất quán (15) | `truck`/`bus`/`van` gán theo đúng tên lớp trong `data.yaml` (kiểm bằng mắt `drive_014`); lệch với gold là lệch ID có hệ thống (mục 5, câu 3) | Ca biên `car`/`van`: 8 box `car` bị gold gán lớp khác (MPV ở `drive_004`); chưa kiểm hết xem có box nào ngoài scope không | Đạt một phần, chờ giảng viên xác nhận thứ tự ID |
| Mini guideline và quyết định mơ hồ (10) | [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md) có định nghĩa 4 lớp, quy tắc box, 3 ca mơ hồ có lý do (`drive_004`, `drive_014`, `drive_020`) | Chưa bổ sung quy tắc MPV/minivan nóc cao; 2 câu hỏi (`drive_021`, `drive_042`) chưa có phản hồi | Đạt |
| Gold-set annotation QC (15) | Có đánh giá với gold và 1 vòng rework (`7f7571a` → `6559507`): F1 0.5923 → 0.6079, mean IoU 0.8584. Nguyên nhân F1 thấp đã phân tích: lệch ID lớp (F1 0.7495 nếu đổi ID trên bản sao) và box thừa ở xe xa | F1 chưa đạt 0.80 theo rubric gốc; theo ngưỡng tạm thời F1 ≥ 0.5 (do gold lệch nhãn) thì đạt | Đạt theo ngưỡng tạm thời |
| Pipeline YOLO và phân tích (10) | Colab chạy hết trên T4; giữ nguyên split, không dùng eval để train; có metrics, ảnh dự đoán 10 ảnh eval trong `assets/` và trả lời 4 câu hỏi | `best.pt` lấy từ epoch 1 nên model gần như chưa học được; chưa train lại sau khi giảng viên xác nhận ID lớp | Đạt |

Điều kiện bắt buộc:
- Đủ 50 file nhãn và chạy qua `check_yolo_labels.py`: **có**.
- Có gold-set evaluation: **có** (`outputs/annotation_evaluation.json`).
- Không dùng eval để train, không sửa gold labels, không truy vết nhãn ngoài repo: **đúng**. Việc thử đổi ID chỉ làm trên bản sao nhãn của tôi, không đụng vào gold.

## 8. Công cụ AI và tài liệu tham khảo

- **Công cụ AI:** hỗ trợ hoàn thiện report.
- **Tài liệu:** chỉ dùng tài liệu, notebook và tool có sẵn trong repo (`check_yolo_labels.py`, `evaluate_annotations.py`); `gold_train_labels/` chỉ dùng để QC, không commit. Không dùng nguồn ngoài repo.
