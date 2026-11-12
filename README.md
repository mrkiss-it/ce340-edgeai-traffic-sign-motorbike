# HỒ SƠ ĐỒ ÁN MÔN HỌC CE340 - TRÍ TUỆ NHÂN TẠO CHO HỆ THỐNG NHÚNG
## ĐỀ TÀI: HỆ THỐNG NHẬN DIỆN BIỂN BÁO GIAO THÔNG ĐƯỜNG BỘ VIỆT NAM CHO XE MÁY TRONG ĐIỀU KIỆN THỜI TIẾT PHỨC TẠP DỰA TRÊN EDGE AI & TINYML

---

* **Đơn vị đào tạo:** Khoa Kỹ thuật Máy tính – Trường Đại học Công nghệ Thông tin (ĐHQG-HCM)
* **Giảng viên hướng dẫn:** ThS. Phan Đình Duy (Email: `duypd@uit.edu.vn`)
* **Học kỳ:** HK1 – Năm học 2026 – 2027
* **Mục tiêu hoàn thành:** Báo cáo và nghiệm thu đồ án **Đợt 1 ở Tuần 8 (09/12/2026)**

---

### DANH SÁCH NHÓM 5 THÀNH VIÊN & PHÂN CÔNG CHUYÊN TRÁCH

| STT | Họ và Tên | MSSV | Vai Trò Chuyên Trách | Đầu Việc Chính Trong Dự Án |
| :---: | :--- | :---: | :--- | :--- |
| 1 | **Trần Hoàng Hôn** | `26410046` | **Trưởng nhóm / PM & QA** | Quản trị dự án (Agile WBS), kiểm soát chất lượng bàn giao (DoD), thiết kế kiến trúc nhúng, tối ưu hóa giao tiếp ngoại vi (OLED, Buzzer) và điều phối báo cáo |
| 2 | **Nguyễn Duy Khang** | `26410055` | **Kỹ sư Dữ liệu** | Thu thập và gán nhãn dataset biển báo Việt Nam (QCVN 41:2019); xây dựng pipeline tăng cường dữ liệu thời tiết phức tạp (mưa, mờ, che khuất) |
| 3 | **Phạm Thành Trung** | `26410141` | **Kỹ sư AI** | Huấn luyện đa mô hình học sâu (YOLOv8n, YOLOv5-tiny, MobileNetV2-SSD); đo lường và benchmark so sánh hiệu năng (`Precision`, `Recall`, `mAP@0.5`) |
| 4 | **Vũ Văn Duy** | `26410031` | **Kỹ sư Nhúng & Thực nghiệm** | Triển khai nạp mô hình nén INT8 lên phần cứng thật (ESP32-S3), thi công hộp mạch bảo vệ gắn ghi-đông xe máy và đo đạc thông số thực tế (FPS, Latency, dòng điện mA) |
| 5 | **Hoàng Cao Sơn** | `25730061` | **Kỹ sư Tối ưu hóa (TinyML)** | Thực hiện kỹ thuật cắt tỉa (Pruning) và lượng tử hóa sau huấn luyện (Post-Training Quantization INT8) trên TensorFlow Lite/ONNX |

---

### CẤU TRÚC CÁC FILE TRONG THƯ MỤC ĐỒ ÁN

```
D:\Trí tuệ nhân tạo\HK3\Trí tuệ nhân tạo cho hệ thống nhúng\Đồ án\
├── 📄 De_Cuong_Chi_Tiet_Do_An_CE340.docx           <-- [FILE CHÍNH THỨC NỘP THẦY] Đề cương chi tiết (Word)
├── 📊 Ke_Hoach_Thuc_Hien_Do_An_CE340_Nhom_Hon.xlsx   <-- [FILE CHÍNH THỨC NỘP THẦY] Bảng tiến độ 8 tuần (Excel)
├── 📝 De_Cuong_Chi_Tiet_Do_An_CE340.md             <-- Bản thảo Markdown đề cương chi tiết đầy đủ nội dung
├── 📑 Khao_Sat_De_Tai_Va_Phuong_An_Thiet_Ke_CE340.md <-- Báo cáo phân tích tính mới & so sánh gắn xe vs mũ bảo hiểm
├── 📘 1. Mẫu Đề cương chi tiết CITD CE340.docx     <-- Mẫu đề cương gốc từ Giảng viên
├── 📗 2. Mẫu Kế hoạch thực hiện đồ án, chuyên đề.xlsx <-- Mẫu kế hoạch gốc từ Giảng viên
├── 📙 3. Mẫu Báo cáo tuần x.docx                   <-- Mẫu nhật ký báo cáo tiến độ tuần (dùng từ Tuần 5-7)
├── 📕 4. Mẫu Báo cáo đồ án.docx                    <-- Mẫu báo cáo tổng kết đồ án cuối kỳ 50 trang
├── 📊 5. Mẫu Slide báo cáo.pptx                    <-- Mẫu slide thuyết trình bảo vệ nghiệm thu trước lớp
└── 📖 README.md                                    <-- Tài liệu này
```

---

### TIẾN ĐỘ THỰC HIỆN & DEADLINE THEO TUẦN

| Tuần | Mốc thời gian | Nhiệm vụ trọng tâm | Sản phẩm nghiệm thu |
| :---: | :---: | :--- | :--- |
| **Tuần 3** | 04/11 – 10/11 | Chốt đề tài & Nộp Đề cương chi tiết + Kế hoạch tiến độ | **0%** *(Khởi tạo đề cương & kế hoạch nộp Giảng viên)* |
| **Tuần 4** | 11/11 – 17/11 | Thu thập dataset; script tăng cường ảnh; chuẩn bị kit ESP32-S3 | **0%** *(Dự kiến)* |
| **Tuần 5** | 18/11 – 24/11 | Train mô hình Baseline; test camera ESP32-S3; **Nộp Báo cáo Tuần 1** | **0%** *(Dự kiến)* |
| **Tuần 6** | 25/11 – 01/12 | Tối ưu hóa TinyML: Pruning & Quantization INT8; **Nộp Báo cáo Tuần 2** | **0%** *(Dự kiến)* |
| **Tuần 7** | 02/12 – 08/12 | Nạp model lên ESP32-S3; ráp hộp ghi-đông; đo FPS/độ trễ; **Nộp Báo cáo Tuần 3** | **0%** *(Dự kiến)* |
| **Tuần 8** | 09/12 – 15/12 | **BÁO CÁO VÀ BẢO VỆ NGHIỆM THU ĐỒ ÁN ĐỢT 1 TRƯỚC LỚP** | **0%** *(Dự kiến)* |
| **Tuần 9** | 16/12 – 22/12 | Hiệu chỉnh sau bảo vệ (nếu có) & Đóng gói source code GitHub | **0%** *(Dự kiến)* |
| **Tuần 10**| 23/12/2026 | Ôn tập tổng kết và thi tập trung cuối kỳ môn CE340 (90 phút) | **0%** *(Dự kiến)* |
