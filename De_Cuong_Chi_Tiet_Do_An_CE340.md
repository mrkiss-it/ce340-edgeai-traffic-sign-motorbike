# ĐỀ CƯƠNG CHI TIẾT ĐỒ ÁN MÔN HỌC CE340
## MÔN: TRÍ TUỆ NHÂN TẠO CHO HỆ THỐNG NHÚNG
### HỌC KỲ 1 – NĂM HỌC 2026 – 2027

---

**ĐẠI HỌC QUỐC GIA TP. HỒ CHÍ MINH**  
**TRƯỜNG ĐẠI HỌC CÔNG NGHỆ THÔNG TIN**  
**KHOA KỸ THUẬT MÁY TÍNH**

---

### THÔNG TIN CHUNG

* **TÊN ĐỀ TÀI (Tiếng Việt):**  
  **Nghiên cứu và triển khai hệ thống nhận diện biển báo giao thông đường bộ Việt Nam cho xe máy trong điều kiện thời tiết phức tạp dựa trên Edge AI & TinyML**
* **TÊN ĐỀ TÀI (Tiếng Anh):**  
  *Research and Implementation of an Edge AI and TinyML-based Traffic Sign Recognition System for Motorbikes in Vietnam under Adverse Weather Conditions*
* **Cán bộ hướng dẫn (CBHD):** ThS. Phan Đình Duy (Email: `duypd@uit.edu.vn`)
* **Thời gian thực hiện:** Từ ngày 21/10/2026 đến ngày 08/12/2026 (Nghiệm thu Đợt 1)
* **Sinh viên thực hiện (Nhóm 5 thành viên):**

| STT | Họ và Tên Sinh Viên | Mã Số Sinh Viên | Lớp Sinh Hoạt | Vai Trò Đảm Trách Trong Đồ Án |
| :---: | :--- | :---: | :---: | :--- |
| 1 | **Trần Hoàng Hôn** | `26410046` | CE2026 | **Trưởng nhóm / Project Manager & QA:** Quản trị dự án, kiểm soát chất lượng bàn giao (DoD), thiết kế kiến trúc hệ thống nhúng, tối ưu hóa giao tiếp ngoại vi và điều phối báo cáo |
| 2 | **Nguyễn Duy Khang** | `26410055` | CE2026 | **Kỹ sư Dữ liệu:** Thu thập, gán nhãn dataset biển báo VN & xây dựng pipeline tăng cường dữ liệu thời tiết phức tạp (mưa, mờ, che khuất) |
| 3 | **Phạm Thành Trung** | `26410141` | CE2026 | **Kỹ sư AI:** Huấn luyện đa mô hình học sâu (YOLOv8n, YOLOv5-tiny, MobileNetV2-SSD) & Benchmark so sánh hiệu năng |
| 4 | **Vũ Văn Duy** | `26410031` | CE2026 | **Kỹ sư Tối ưu hóa (TinyML):** Thực hiện kỹ thuật cắt tỉa (Pruning) & lượng tử hóa số nguyên 8-bit (Quantization INT8) trên TFLite/ONNX |
| 5 | **Hoàng Cao Sơn** | `25730061` | CE2026 | **Kỹ sư Nhúng & Thực nghiệm:** Triển khai nạp mô hình lên phần cứng thật (ESP32-S3), thi công vỏ hộp gắn ghi-đông & đo đạc hiệu năng (FPS, Latency, mA) |

---

## I. NỘI DUNG ĐỀ TÀI

### 1. Tổng quan đề tài & Tính cấp thiết
Tại Việt Nam, xe máy là phương tiện giao thông chủ lực, chiếm hơn **80% tổng lưu lượng phương tiện** lưu thông trên toàn quốc. Tuy nhiên, điều kiện giao thông đô thị tại các thành phố lớn (như TP.HCM, Hà Nội) rất phức tạp: mật độ phương tiện dày đặc, hệ thống biển báo đôi khi bị che khuất bởi tán cây, dây điện, bảng hiệu quảng cáo; bên cạnh đó là điều kiện thời tiết nhiệt đới khắc nghiệt như mưa lớn gây nhòe mờ tầm nhìn, thiếu sáng vào ban đêm và hiện tượng rung lắc mạnh của tay lái xe máy. Những yếu tố này thường xuyên khiến người điều khiển xe máy vô tình vi phạm luật giao thông (đi vào đường cấm, vượt quá tốc độ cho phép, rẽ vào đường một chiều) hoặc gặp nguy cơ tai nạn giao thông nghiêm trọng.

Trong khi các hệ thống hỗ trợ lái xe nâng cao (ADAS) và camera hành trình nhận diện biển báo thông minh (như của Vietmap, 70mai, Mobileye) đã phát triển mạnh mẽ trên ô tô, thì trên thị trường xe máy hiện nay **hoàn toàn là một khoảng trống công nghệ (Market & Research Gap)**. Các dòng xe máy thông minh hiện nay mới chỉ tích hợp màn hình kết nối Bluetooth cơ bản để báo cuộc gọi/tin nhắn, chưa hề có giải pháp thị giác máy tính nào hỗ trợ cảnh báo an toàn giao thông cho người lái.

#### Khảo sát các nghiên cứu liên quan:
* **2 Bài báo nghiên cứu quốc tế:**
  1. *IEEE Xplore (2023) – "Lightweight Traffic Sign Recognition for Resource-Constrained Edge Devices under Adverse Weather Conditions":* Nghiên cứu này chứng minh rằng việc áp dụng kỹ thuật lượng tử hóa sau huấn luyện (Post-Training Quantization INT8) kết hợp kiến trúc mạng nơ-ron rút gọn có thể giảm kích thước mô hình từ 4 đến 6 lần, đạt tốc độ suy luận $>20\text{ FPS}$ trên vi xử lý nhúng mà độ chính xác mAP chỉ suy giảm dưới $2.5\%$.
  2. *Sensors / MDPI (2024) – "Real-Time Embedded Computer Vision for Motorcyclist Advanced Driver Assistance Systems (ADAS)":* Công trình phân tích sâu các ràng buộc cơ học đặc thù của xe máy hai bánh: góc quan sát camera bị rung lắc động, độ trễ hệ thống bắt buộc phải dưới $150\text{ms}$ để người lái kịp phản xạ ở tốc độ $40 - 50\text{ km/h}$, và thiết kế giao diện cảnh báo (HMI) phải trực quan qua âm thanh hoặc màn hình nhỏ gắn ghi-đông để không làm xao nhãng người điều khiển.
* **2 Nghiên cứu và đồ án trong nước:**
  1. *Tạp chí Giao thông Vận tải Việt Nam (2023) – "Nghiên cứu nhận diện biển báo giao thông đường bộ Việt Nam theo quy chuẩn QCVN 41:2019/BGTVT sử dụng mạng nơ-ron tích chập":* Nghiên cứu đã xây dựng bộ phân loại chuẩn cho các biển báo giao thông đường bộ đặc thù Việt Nam, khẳng định các thuật toán phát hiện đối tượng một giai đoạn (one-stage detectors) như YOLO cho hiệu quả vượt trội so với các phương pháp trích xuất đặc trưng truyền thống.
  2. *Khóa luận tốt nghiệp chuyên ngành Kỹ thuật Máy tính (Trường ĐH Công nghệ Thông tin, ĐHQG-HCM) – "Nghiên cứu và triển khai mô hình học sâu nhận diện đối tượng thời gian thực trên vi xử lý nhúng hỗ trợ TensorFlow Lite":* Đề tài đã thử nghiệm quy trình biên dịch mô hình sang định dạng `.tflite` nạp xuống kit nhúng có bộ nhớ hạn chế, cung cấp phương pháp luận thực nghiệm đo đạc độ trễ và mức chiếm dụng RAM/Flash.

#### Xác định vấn đề cần giải quyết:
1. Xây dựng và tăng cường tập dữ liệu biển báo giao thông Việt Nam tập trung vào các trường hợp khó (Edge Cases): bị che khuất một phần (Occlusion), mờ chuyển động do xe chạy (Motion Blur), trời mưa đọng nước trên ống kính (Rainy) và ánh sáng yếu ban đêm.
2. Thiết kế và huấn luyện nhiều kiến trúc mô hình học sâu gọn nhẹ (YOLOv8n, YOLOv5-tiny, MobileNetV2-SSD) để benchmark so sánh hiệu năng.
3. Ứng dụng kỹ thuật nén mô hình cốt lõi của môn học: **Cắt tỉa (Pruning)** và **Lượng tử hóa số nguyên 8-bit (Quantization INT8)** để nén mô hình nạp vừa bộ nhớ của kit nhúng thật (ESP32-S3 hoặc Raspberry Pi).
4. Xây dựng thiết bị phần cứng hoàn chỉnh gắn trên ghi-đông xe máy, tích hợp màn hình OLED hiển thị icon biển báo và còi Buzzer phát tín hiệu cảnh báo với độ trễ phản hồi $< 150\text{ms}$.

#### Sơ đồ tổng quan giải pháp:
```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│             SƠ ĐỒ TỔNG QUAN HỆ THỐNG NHẬN DIỆN BIỂN BÁO CHO XE MÁY (CE340)             │
└────────────────────────────────────────────────────────────────────────────────────────┘

 [ 1. Thu Nhận Hình Ảnh ]           [ 2. Khối Tiền Xử Lý ]         [ 3. Khối Suy Luận Edge AI ]
┌─────────────────────────┐       ┌────────────────────────┐      ┌───────────────────────────┐
│ Camera Góc Rộng Đầu Xe  │       │ • Cân bằng tương phản  │      │ Bo Mạch Nhúng Xử Lý       │
│ (OV2640 / CSI Camera)   │──────>│   thời tiết (CLAHE)    │─────>│ (ESP32-S3 / Raspberry Pi)  │
│ Thu video 320x320 pixel │       │ • Cắt vùng quan tâm    │      │ • Model Nén INT8          │
│ Tốc độ: 15 - 25 FPS     │       │   (ROI biển báo bên lề)│      │ • Phân loại QCVN 41:2019  │
└─────────────────────────┘       └────────────────────────┘      └─────────────┬─────────────┘
                                                                                │
                                                                   [Tín hiệu nhận diện đúng]
                                                                                │
                                                                                ▼
                                                                  [ 4. Giao Diện Cảnh Báo ]
                                                                  ┌───────────────────────────┐
                                                                  │ Gắn Trên Ghi-đông Xe Máy  │
                                                                  │ • Màn hình OLED mini      │
                                                                  │   (Hiện icon biển báo)    │
                                                                  │ • Còi Buzzer 5V & LED đỏ  │
                                                                  │   (Cảnh báo biển cấm)     │
                                                                  └───────────────────────────┘
```

---

### 2. Mục tiêu của đề tài
* **Mục tiêu tổng quát:** Xây dựng thành công một thiết bị nhúng thông minh nhỏ gọn gắn trên xe máy, có khả năng tự động xử lý hình ảnh và nhận diện chính xác biển báo giao thông Việt Nam theo thời gian thực (Real-time Offline Edge Computing) ngay tại thiết bị biên, giúp nâng cao an toàn cho người điều khiển xe máy mà không cần kết nối Internet.
* **Mục tiêu cụ thể:**
  1. *Về phần cứng:* Thiết kế và thi công hoàn chỉnh module phần cứng bao gồm bo mạch xử lý nhúng, camera góc rộng, màn hình hiển thị OLED I2C và còi cảnh báo Buzzer. Mạch được đóng trong hộp vỏ bảo vệ, chống rung lắc, cấp nguồn an toàn từ cổng USB 5V xe máy hoặc pin sạc dự phòng.
  2. *Về mô hình AI và tối ưu hóa:* Huấn luyện tối thiểu 2–3 kiến trúc mô hình học sâu phát hiện đối tượng; áp dụng thành công kỹ thuật **Pruning** và **Quantization INT8** giúp giảm dung lượng mô hình $\ge 70\%$, đảm bảo nạp vừa bộ nhớ Flash/RAM của vi điều khiển.
  3. *Về hiệu năng:* Mô hình sau nén đạt độ chính xác $\ge 85\%$ (mAP@0.5) trên tập kiểm thử có các điều kiện che khuất, mờ rung lắc và thời tiết mưa; tốc độ xử lý đạt $\ge 10 - 15\text{ FPS}$ với độ trễ phản hồi toàn hệ thống $< 150\text{ms}$.
  4. *Về giao diện người dùng (HMI):* Hiển thị trực quan icon biển báo lên màn hình OLED mini gắn ghi-đông và phát âm thanh cảnh báo tức thì khi gặp biển báo cấm hoặc nguy hiểm.

---

### 3. Phương pháp thực hiện & Tổ chức dự án song song (Zero-Waiting Pipeline)
Dự án được quản lý theo phương pháp linh hoạt (Agile WBS), phân rã công việc độc lập thông qua kỹ thuật Contract-First và Dữ liệu mẫu (Mock Data). Trưởng nhóm đảm nhiệm vai trò PM/QA và tối ưu hóa hệ thống, cung cấp sẵn các bộ dữ liệu/mô hình mẫu để các thành viên (Dữ liệu, AI, Tối ưu hóa, Nhúng) triển khai song song 100% thời gian ngay từ Tuần 4 mà không phải chờ đợi lẫn nhau.

* **Phương pháp nghiên cứu tài liệu:**
  * Tra cứu Quy chuẩn kỹ thuật quốc gia về báo hiệu đường bộ **QCVN 41:2019/BGTVT** để trích xuất quy cách, màu sắc, hình dạng các nhóm biển báo trọng yếu tại Việt Nam.
  * Nghiên cứu datasheet vi điều khiển **ESP32-S3** (kiến trúc Xtensa LX7 hỗ trợ tập lệnh tăng tốc AI Vector), module camera OV2640, chuẩn giao tiếp I2C/SPI.
  * Nghiên cứu tài liệu kỹ thuật về các phương pháp nén mô hình TinyML: Lượng tử hóa sau huấn luyện (Post-Training Quantization), Cắt tỉa trọng số (Weight Pruning) trên TensorFlow Lite, TensorFlow Lite for Microcontrollers (TFLM) và ONNX Runtime.
* **Phương pháp thực nghiệm song song:**
  * *Thu thập & Gán nhãn dữ liệu:* Sử dụng tập dữ liệu biển báo Việt Nam kết hợp chụp thực tế trên các tuyến đường TP.HCM; dùng công cụ **Roboflow / LabelImg** để vẽ bounding box gán nhãn chuẩn định dạng YOLO.
  * *Tăng cường dữ liệu (Data Augmentation):* Xây dựng pipeline bằng thư viện `Albumentations` để sinh các biến thể ảnh phức tạp: giả lập hiệu ứng hạt mưa rơi (*Random Rain*), làm mờ chuyển động khi xe chạy (*Motion Blur*), tạo mảng che khuất ngẫu nhiên (*Cutout / Random Erasing*), và thay đổi độ sáng (*Brightness / Contrast Shift*).
  * *Huấn luyện đa mô hình & Tối ưu hóa:* Huấn luyện các mô hình Baseline (YOLOv8n, YOLOv5-tiny, MobileNetV2-SSD) trên máy chủ GPU; áp dụng kỹ thuật Pruning và Quantization INT8 để trích xuất file `.tflite`.
  * *Triển khai & Đo lường:* Nạp mô hình nhúng xuống phần cứng thật, lập trình firmware đọc camera, chạy suy luận và kích hoạt ngoại vi.
* **Phương pháp kiểm thử và QA:**
  * Kiểm soát chất lượng theo tiêu chí Definition of Done (DoD).
  * Đo lường định lượng các chỉ số học máy: `Precision`, `Recall`, `F1-Score`, `mAP@0.5` trên tập dữ liệu kiểm thử chuẩn và tập dữ liệu thử thách thời tiết khắc nghiệt.
  * Đo lường hiệu năng phần cứng thực tế: Thời gian suy luận (`Inference Latency` tính bằng ms), Tốc độ khung hình (`FPS`), Mức chiếm dụng bộ nhớ tĩnh và động (`RAM/Flash Usage`), Dòng điện tiêu thụ (`mA`) và nhiệt độ hoạt động của chip.

---

### 4. Các nội dung chính của đề tài

* **Nội dung 1: Nghiên cứu tổng quan đề tài, khảo sát và khởi tạo dự án**
  * *Thực hiện cái gì:* Khảo sát các tài liệu nghiên cứu quốc tế và trong nước về TinyML cho nhận diện biển báo; khảo sát quy chuẩn QCVN 41:2019/BGTVT; xác định danh mục các lớp biển báo mục tiêu; lập sơ đồ kiến trúc khối phần cứng và phần mềm của hệ thống. PM phân rã công việc WBS và chuẩn bị mock assets.
  * *Thực hiện như thế nào:* Tra cứu tài liệu học thuật trên IEEE, ScienceDirect; đối chiếu thực tế xe máy tại Việt Nam; thảo luận nhóm và chốt các thông số kỹ thuật (độ trễ $<150\text{ms}$, FPS $\ge 10-15$).
  * *Dự kiến kết quả:* Hoàn thành Bản Đề cương chi tiết và Bảng kế hoạch thực hiện đồ án 8 tuần được CBHD phê duyệt.
* **Nội dung 2: Thu thập, gán nhãn và xây dựng pipeline tăng cường dữ liệu thời tiết phức tạp**
  * *Thực hiện cái gì:* Xây dựng tập dữ liệu biển báo giao thông đường bộ Việt Nam gồm 10–15 lớp biển báo trọng yếu (thuộc nhóm cấm, hiệu lệnh, tốc độ); tạo các tập dữ liệu thử thách với điều kiện bị che khuất, mờ rung lắc và thời tiết mưa.
  * *Thực hiện như thế nào:* Tổng hợp dữ liệu từ Zalo AI Challenge kết hợp quay video thực tế trên đường phố TP.HCM; chuẩn hóa và gán nhãn bằng Roboflow; viết script Python áp dụng `Albumentations` để tăng cường dữ liệu lên khoảng 3.000 – 5.000 ảnh.
  * *Dự kiến kết quả:* Bộ dữ liệu được chia theo tỷ lệ Train/Val/Test (70/20/10) đạt chuẩn định dạng YOLO và sẵn sàng huấn luyện.
* **Nội dung 3: Thiết kế, huấn luyện đa mô hình học sâu và benchmark so sánh hiệu năng**
  * *Thực hiện cái gì:* Cấu hình và huấn luyện các kiến trúc mô hình học sâu nhẹ: YOLOv8n, YOLOv5-tiny, MobileNetV2-SSD trên GPU Google Colab / máy trạm.
  * *Thực hiện như thế nào:* Huấn luyện mô hình qua 100–150 epochs; điều chỉnh siêu tham số (*hyperparameters*); đánh giá và so sánh độ chính xác (`mAP@0.5`, `Recall`, `Confusion Matrix`) giữa các mô hình.
  * *Dự kiến kết quả:* Bảng so sánh thực nghiệm hiệu năng giữa 3 kiến trúc mô hình; lựa chọn mô hình có sự cân bằng tối ưu giữa độ chính xác và tốc độ để chuyển sang bước nén.
* **Nội dung 4: Tối ưu hóa mô hình AI (TinyML Optimization) và triển khai trên thiết bị nhúng**
  * *Thực hiện cái gì:* Áp dụng các kỹ thuật nén mô hình: Cắt tỉa trọng số dư thừa (Pruning) và Lượng tử hóa sau huấn luyện từ số thực dấu phẩy động FP32 sang số nguyên 8-bit (Post-Training Quantization INT8).
  * *Thực hiện như thế nào:* Sử dụng công cụ TensorFlow Lite Converter hoặc ONNX Runtime; xây dựng tập dữ liệu hiệu chuẩn đại diện (*Representative Calibration Dataset*); đo lường sự suy giảm độ chính xác so với tỷ lệ nén dung lượng; chuyển đổi mô hình thành mảng C-array để nạp vào firmware nhúng.
  * *Dự kiến kết quả:* File mô hình `.tflite` (hoặc `.h`) định dạng INT8 có dung lượng nén giảm $\ge 70\%$, sẵn sàng thực thi trên vi điều khiển.
* **Nội dung 5: Tích hợp phần cứng trên xe máy, đo đạc thực nghiệm và viết báo cáo tổng kết**
  * *Thực hiện cái gì:* Lắp ráp và hoàn thiện mạch phần cứng hoàn chỉnh gồm camera, bo mạch vi điều khiển (ESP32-S3), màn hình OLED I2C và còi Buzzer; thiết kế vỏ hộp bảo vệ gắn ghi-đông; tiến hành chạy thử nghiệm thực tế và đo đạc các chỉ số phần cứng; viết báo cáo đồ án và chuẩn bị slide thuyết trình.
  * *Thực hiện như thế nào:* Lập trình firmware C++/Arduino đọc luồng ảnh camera, tiền xử lý và chạy suy luận; hiển thị icon lên OLED; đo dòng tiêu thụ bằng ampe kế; kiểm thử nhận diện thực tế trên đường; tổng hợp số liệu viết báo cáo theo chuẩn UIT.
  * *Dự kiến kết quả:* Sản phẩm demo phần cứng hoàn chỉnh hoạt động ổn định; video clip demo thực nghiệm; cuốn Báo cáo đồ án đúng quy chuẩn (khoảng 40–50 trang) và slide báo cáo nghiệm thu trước lớp.

---

### 5. Giới hạn của đề tài
* **Phạm vi phân loại biển báo:** Do thời gian làm đồ án trong 6 tuần thực nghiệm, đề tài giới hạn tập trung vào **10–15 loại biển báo trọng điểm** có tác động trực tiếp nhất đến an toàn người đi xe máy tại đô thị Việt Nam:
  * *Nhóm biển cấm (P):* Cấm đi ngược chiều (P.102), Cấm rẽ trái (P.103a), Cấm rẽ phải (P.103b), Cấm quay đầu (P.124a), Cấm xe mô tô (P.111a).
  * *Nhóm biển hiệu lệnh (R):* Hướng đi thẳng phải theo (R.301a), Hướng phải đi theo vòng xuyến (R.303).
  * *Nhóm biển giới hạn tốc độ (P.127):* Tốc độ tối đa cho phép 40 km/h, 50 km/h, 60 km/h.
  * *Nhóm biển cảnh báo nguy hiểm (W):* Giao nhau có tín hiệu đèn (W.209), Đi chậm (W.245).
* **Giới hạn về điều kiện môi trường:** Kiểm thử tập trung vào các trường hợp biển báo bị che khuất một phần ($< 40\%$ diện tích biển), mờ chuyển động khi xe di chuyển ở tốc độ thông thường ($30 - 50\text{ km/h}$), và điều kiện mưa rào nhẹ / mưa nhân tạo đọng nước trên ống kính.
* **Giới hạn về phần cứng:** Thiết bị hoạt động hoàn toàn **Offline (độc lập)** trên vi điều khiển / máy tính nhúng, không sử dụng kết nối 4G/Cloud để xử lý nhằm đảm bảo tính bảo mật và triệt tiêu độ trễ mạng.

---

### 6. Đối tượng nghiên cứu
* **Đối tượng thiết bị phần cứng:** Vi điều khiển thế hệ mới hỗ trợ tăng tốc AI **ESP32-S3** (hoặc máy tính nhúng Raspberry Pi 4), cảm biến hình ảnh góc rộng OV2640, màn hình hiển thị OLED I2C 0.96 inch / 1.3 inch, còi cảnh báo Buzzer 5V.
* **Đối tượng phần mềm và thuật toán:**
  * Các kiến trúc mạng nơ-ron tích chập phát hiện đối tượng thu gọn: YOLOv8n, YOLOv5-tiny, MobileNetV2-SSD.
  * Kỹ thuật tối ưu hóa mô hình học sâu biên: Post-Training Quantization (INT8), Magnitude-based Weight Pruning.
  * Thư viện và framework: TensorFlow, TensorFlow Lite for Microcontrollers (TFLM), OpenCV, Albumentations, ESP-IDF / Arduino C++.

---

### 7. Kết quả mong đợi
* **Về sản phẩm phần cứng:**
  * Một module thiết bị nhúng thông minh hoàn chỉnh, mạch điện được thi công an toàn, chắc chắn, đóng hộp bảo vệ nhỏ gọn có thể gắn cố định lên ghi-đông xe máy.
  * Hệ thống hoạt động ổn định với nguồn cấp 5V, không bị quá nhiệt khi chạy liên tục.
* **Về sản phẩm phần mềm:**
  * Bộ dữ liệu biển báo giao thông Việt Nam được tăng cường các kịch bản thời tiết phức tạp.
  * Pipeline huấn luyện và mã nguồn chuyển đổi mô hình nén INT8 hoàn chỉnh.
  * Chương trình firmware chạy suy luận thời gian thực trên bo nhúng đạt:
    * Độ chính xác nhận diện: $\ge 85\%$ trong điều kiện tiêu chuẩn và $\ge 75\%$ trong điều kiện khắc nghiệt.
    * Tốc độ xử lý: $\ge 10 - 15\text{ FPS}$.
    * Độ trễ toàn hệ thống: $< 150\text{ms}$.
* **Về tài liệu báo cáo:**
  * Một cuốn Báo cáo đồ án môn học đúng quy chuẩn nghiên cứu khoa học của Trường ĐH Công nghệ Thông tin (khoảng 40–50 trang).
  * Slide báo cáo thuyết trình chuyên nghiệp kèm toàn bộ mã nguồn trên GitHub và video clip thực nghiệm demo trên đường phố.

---

## II. KẾ HOẠCH THỰC HIỆN CHI TIẾT (BẢNG TIẾN ĐỘ)

| Tuần | Thời Gian Thực Hiện | Nội Dung Công Việc Cụ Thể | Thành Viên Phụ Trách Chính | Sản Phẩm Bàn Giao | Mức Độ Hoàn Thành |
| :---: | :---: | :--- | :--- | :--- | :---: |
| **Tuần 3** | 04/11 – 10/11/2026 | Viết Thuyết minh đề tài & Lập kế hoạch phân công công việc chi tiết; chuẩn bị môi trường. | **Trần Hoàng Hôn** (PM) & Nhóm | • File Đề cương chi tiết (Word)<br>• File Kế hoạch thực hiện (Excel) | **0%** *(Khởi tạo đề cương)* |
| **Tuần 4** | 11/11 – 17/11/2026 | • Thu thập và gán nhãn tập dữ liệu biển báo Việt Nam.<br>• Viết script tăng cường ảnh thời tiết phức tạp (mưa, mờ, che khuất).<br>• Khảo sát linh kiện phần cứng (ESP32-S3-CAM, OLED, Buzzer).<br>• Dựng mock pipeline huấn luyện và firmware test. | • **Nguyễn Duy Khang**<br>• **Vũ Văn Duy**<br>• **Hoàng Cao Sơn**<br>• **Trần Hoàng Hôn** | • Dataset biển báo đã gán nhãn YOLO<br>• Script Data Augmentation<br>• Danh mục linh kiện chuẩn bị<br>• Mock training pipeline | **0%** *(Dự kiến)* |
| **Tuần 5** | 18/11 – 24/11/2026 | • Huấn luyện các mô hình Baseline (YOLOv8n, YOLOv5-tiny, MobileNet).<br>• Lập trình firmware đọc camera và test ngoại vi trên ESP32-S3.<br>• **Nộp Báo cáo tiến độ Tuần 1 (Báo cáo tuần 5).** | • **Phạm Thành Trung**<br>• **Trần Hoàng Hôn**<br>• **Hoàng Cao Sơn** | • Trọng số mô hình Baseline (.pt)<br>• Bảng đánh giá mAP ban đầu<br>• Mã nguồn firmware test camera OLED | **0%** *(Dự kiến)* |
| **Tuần 6** | 25/11 – 01/12/2026 | • Thực hiện tối ưu hóa TinyML: Pruning & Quantization INT8.<br>• Đánh giá trade-off suy giảm độ chính xác so với kích thước nén.<br>• **Nộp Báo cáo tiến độ Tuần 2 (Báo cáo tuần 6).** | • **Vũ Văn Duy**<br>• **Phạm Thành Trung**<br>• **Trần Hoàng Hôn** (QA) | • File mô hình nén `.tflite` (INT8)<br>• Bảng đối chiếu kích thước & sai số | **0%** *(Dự kiến)* |
| **Tuần 7** | 02/12 – 08/12/2026 | • Triển khai nạp mô hình INT8 lên ESP32-S3-CAM.<br>• Thi công hộp bảo vệ mạch gắn ghi-đông xe máy.<br>• Thực nghiệm đo đạc: Latency (ms), FPS, RAM/Flash, Dòng tiêu thụ (mA).<br>• **Nộp Báo cáo tiến độ Tuần 3 (Báo cáo tuần 7).** | • **Hoàng Cao Sơn**<br>• **Trần Hoàng Hôn**<br>• Cả nhóm | • Thiết bị phần cứng hoàn chỉnh<br>• Số liệu đo đạc thực nghiệm phần cứng<br>• Video clip demo thực tế | **0%** *(Dự kiến)* |
| **Tuần 8** | 09/12 – 15/12/2026 | • **BÁO CÁO VÀ NGHIỆM THU ĐỒ ÁN ĐỢT 1 TRƯỚC LỚP.**<br>• Nộp trọn bộ sản phẩm: Báo cáo Word/PDF + Slide + Source Code. | **Cả nhóm 5 thành viên** *(Trần Hoàng Hôn điều phối)* | • Báo cáo đồ án hoàn chỉnh 50 trang<br>• Slide thuyết trình nghiệm thu<br>• Bản demo hoạt động trước Thầy | **0%** *(Dự kiến)* |
| **Tuần 9** | 16/12 – 22/12/2026 | Hiệu chỉnh tài liệu theo góp ý của CBHD (nếu có) & Đóng gói sản phẩm GitHub. | **Trần Hoàng Hôn** & Nhóm | Bộ tài liệu nghiệm thu chính thức | **0%** *(Dự kiến)* |
| **Tuần 10**| 23/12/2026 | Ôn tập tổng kết và chuẩn bị thi tập trung cuối kỳ môn CE340. | Cả nhóm | Hoàn thành môn học | **0%** *(Dự kiến)* |

---

### XÁC NHẬN CỦA CÁC BÊN

**CÁN BỘ HƯỚNG DẪN**  
*(Ký và ghi rõ họ tên)*  
<br><br><br>  
**ThS. Phan Đình Duy**

*TP. Hồ Chí Minh, ngày 10 tháng 11 năm 2026*  
**ĐẠI DIỆN NHÓM SINH VIÊN THỰC HIỆN**  
*(Ký và ghi rõ họ tên)*  
<br><br><br>  
**Trần Hoàng Hôn** *(Trưởng nhóm)*
