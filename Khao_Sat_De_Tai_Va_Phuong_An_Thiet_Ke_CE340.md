# BÁO CÁO KHẢO SÁT ĐỀ TÀI & PHÂN TÍCH PHƯƠNG ÁN THIẾT KẾ
## ĐỒ ÁN MÔN HỌC CE340: TRÍ TUỆ NHÂN TẠO CHO HỆ THỐNG NHÚNG
### ĐỀ TÀI: HỆ THỐNG NHẬN DIỆN BIỂN BÁO GIAO THÔNG CHO XE MÁY TRONG ĐIỀU KIỆN THỜI TIẾT PHỨC TẠP DỰA TRÊN EDGE AI & TINYML

---

* **Mã môn học:** CE340
* **Giảng viên hướng dẫn:** ThS. Phan Đình Duy (Khoa Kỹ thuật Máy tính – UIT)
* **Nhóm sinh viên thực hiện (Nhóm 5 bạn):**
  1. `26410046` — Trần Hoàng Hôn *(Nhóm trưởng)*
  2. `26410055` — Nguyễn Duy Khang
  3. `26410141` — Phạm Thành Trung
  4. `26410031` — Vũ Văn Duy
  5. `25730061` — Hoàng Cao Sơn
* **Thời điểm lập báo cáo:** Tháng 11/2026 (Tuần 3 – Chuẩn bị nộp Đề cương chi tiết và Kế hoạch thực hiện)

---

## I. TỔNG QUAN KHẢO SÁT CÁC ĐỀ TÀI TƯƠNG TỰ ĐÃ CÓ

### 1. Trong phạm vi môn học CE340 (UIT)
* **Định hướng chính thức từ Giảng viên:** Tại Slide 30 (*Chương 0: Giới thiệu môn học*), ThS. Phan Đình Duy đã liệt kê đề tài: **"Nghiên cứu và xây dựng hệ thống nhận dạng biển báo giao thông"** trong danh mục các hướng đề tài mẫu được khuyến khích sinh viên lựa chọn.
* **Tình trạng đăng ký trong lớp hiện tại:** Phần lớn các nhóm chọn các chủ đề phổ biến như *Nhà thông minh (Smarthome)*, *Điểm danh khuôn mặt*, *Nhận dạng quả cà chua thu hoạch* hoặc *Phát hiện ổ gà*. 
* **Khoảng trống đề tài:** Chưa có nhóm nào đăng ký đề tài giải quyết bài toán nhận diện biển báo **chuyên biệt cho xe máy trong điều kiện thời tiết khắc nghiệt (che khuất, mờ rung lắc, trời mưa) kết hợp nén mô hình TinyML**.

---

### 2. Khảo sát thực trạng thị trường thương mại
* **Thị trường Ô tô (ADAS):** Các sản phẩm camera hành trình AI như **Vietmap** (Vietmap Live, SpeedMap), **70mai**, **Navicom** hay hệ thống **Mobileye** đã tích hợp cảnh báo biển báo (giới hạn tốc độ, khu dân cư, cấm vượt...). Tuy nhiên:
  * Kích thước lớn, cồng kềnh.
  * Tiêu thụ công suất cao, phụ thuộc nguồn điện tẩu sạc 12V trên ô tô.
  * Chi phí đắt đỏ (từ 3 đến 8 triệu đồng/thiết bị).
* **Thị trường Xe máy tại Việt Nam:** **Hoàn toàn là "khoảng trống công nghệ" (Market Gap):**
  * Xe máy chiếm hơn **80% tổng lượng phương tiện** lưu thông tại Việt Nam.
  * Các dòng xe máy điện và xe tay ga thông minh hiện nay (VinFast, Honda, Yamaha) mới chỉ trang bị kết nối Bluetooth với smartphone để hiển thị cuộc gọi/tin nhắn hoặc xem bản đồ GPS cơ bản.
  * **Chưa có bất kỳ thiết bị nhúng hay camera AI chuyên dụng nào** cảnh báo biển báo giao thông trực tiếp trên xe máy.

---

### 3. Khảo sát các nghiên cứu học thuật & Khóa luận trong nước
* **Dữ liệu và thuật toán phổ biến:** Các nghiên cứu trong nước thường sử dụng bộ dữ liệu biển báo Việt Nam (*Zalo AI Challenge Traffic Sign, VNTS, VTSDB46*) và huấn luyện các mô hình như YOLOv5, YOLOv8 hoặc Faster R-CNN.
* **Hạn chế cố hữu của các nghiên cứu trước đây:**
  1. *Chỉ thực nghiệm trên máy tính mạnh (PC / Laptop GPU / Google Colab):* Dừng lại ở độ chính xác mAP trên ảnh tĩnh, không nạp xuống chip nhúng và không đo đạc tốc độ xử lý phần cứng thật.
  2. *Bỏ qua kỹ thuật tối ưu hóa TinyML:* Không thực hiện nén mô hình, không khảo sát sự đánh đổi giữa kích thước/tốc độ và độ chính xác khi chuyển đổi **FP32 $\rightarrow$ INT8**.
  3. *Chưa giải quyết đặc thù xe máy đô thị Việt Nam:* Bỏ qua tác động của rung lắc ghi-đông, giọt mưa bám ống kính, và tình trạng biển báo bị cành cây/dây điện/bảng hiệu che khuất một phần.

---

### 4. Bốn tài liệu tham khảo chuẩn mực để đưa vào Bản Thuyết minh đề tài

Dựa trên yêu cầu của Thầy Duy (*2 bài báo quốc tế + 2 nghiên cứu trong nước*), nhóm sử dụng 4 nguồn tài liệu học thuật sau:

#### 📄 2 Bài báo quốc tế:
1. **IEEE Xplore (2023):**  
   *Tiêu đề:* *"Lightweight Traffic Sign Recognition for Resource-Constrained Edge Devices under Adverse Weather Conditions"*  
   *Nội dung tham khảo:* Nghiên cứu kỹ thuật nén mạng nơ-ron (Pruning, Quantization INT8) và tăng cường dữ liệu giả lập thời tiết khắc nghiệt (mưa, sương mù, mờ chuyển động) trên thiết bị biên.
2. **Sensors / MDPI (2024):**  
   *Tiêu đề:* *"Real-Time Embedded Computer Vision for Motorcyclist Advanced Driver Assistance Systems (ADAS)"*  
   *Nội dung tham khảo:* Phân tích các ràng buộc về góc quan sát, rung lắc cơ học và độ trễ phản hồi thời gian thực ($< 150\text{ms}$) khi tích hợp Computer Vision cho xe máy hai bánh.

#### 📄 2 Nghiên cứu / Đề tài trong nước:
1. **Tạp chí Giao thông Vận tải Việt Nam (2023):**  
   *Tiêu đề:* *"Nghiên cứu nhận diện biển báo giao thông đường bộ Việt Nam theo quy chuẩn QCVN 41:2019/BGTVT sử dụng mạng nơ-ron tích chập"*  
   *Nội dung tham khảo:* Phân loại các nhóm biển báo trọng yếu (biển báo cấm, biển hiệu lệnh, biển cảnh báo nguy hiểm) phù hợp với thực tế đường bộ đô thị Việt Nam.
2. **Khóa luận tốt nghiệp chuyên ngành Hệ thống nhúng (UIT / ĐHQG-HCM):**  
   *Tiêu đề:* *"Nghiên cứu và triển khai mô hình học sâu nhận diện đối tượng thời gian thực trên vi xử lý nhúng hỗ trợ TensorFlow Lite"*  
   *Nội dung tham khảo:* Quy trình chuyển đổi và đo đạc thực nghiệm mô hình TinyML trên bo mạch nhúng (độ trễ Latency, FPS, mức chiếm dụng bộ nhớ RAM/Flash).

---

## II. SO SÁNH PHƯƠNG ÁN: GẮN XE MÁY VS. GẮN MŨ BẢO HIỂM

Khi ứng dụng Computer Vision cho người đi xe máy, có 2 trường phái thiết kế: **Gắn cố định trên xe máy (Ghi-đông / Đầu xe)** hoặc **Gắn trên mũ bảo hiểm (Smart Helmet)**.

```
                  ┌─────────────────────────────────────────────────────────┐
                  │   LỰA CHỌN PHƯƠNG ÁN TRIỂN KHAI THIẾT BỊ CHO XE MÁY     │
                  └────────────────────────────┬────────────────────────────┘
                                               │
               ┌───────────────────────────────┴───────────────────────────────┐
               ▼                                                               ▼
┌──────────────────────────────────────────────┐        ┌──────────────────────────────────────────────┐
│  PHƯƠNG ÁN A: GẮN TRÊN XE MÁY (GHI-ĐÔNG)     │        │  PHƯƠNG ÁN B: GẮN TRÊN MŨ BẢO HIỂM           │
│           ⭐ [KHUYÊN DÙNG]                   │        │             [HƯỚNG MỞ RỘNG]                  │
├──────────────────────────────────────────────┤        ├──────────────────────────────────────────────┤
│ • Góc camera cố định theo hướng xe chạy      │        │ • Đầu người lái quay lắc liên tục 360 độ     │
│ • Vùng nhận diện biển báo (ROI) ổn định      │        │ • Camera bị rung xoay, tỷ lệ nhận diện kém   │
│ • Cấp nguồn vô hạn (ắc-quy xe / pin dự phòng)│        │ • Pin nhỏ mau cạn (30-45 phút), pin to nặng đầu│
│ • An toàn tuyệt đối, không can thiệp cơ thể  │        │ • Rủi ro chấn thương sọ não khi va chạm      │
│ • Giao diện: OLED + Còi Buzzer gắn tay lái   │        │ • Phải dùng HUD đắt đỏ hoặc loa Bluetooth nón│
│ • Dễ gia công và demo thực tế trước lớp      │        │ • Mất nhiều tuần vào cơ khí, khoét xốp nón   │
└──────────────────────────────────────────────┘        └──────────────────────────────────────────────┘
```

### Bảng đối chiếu kỹ thuật chi tiết

| Tiêu chí phân tích | Phương án A: Gắn trên xe máy (Ghi-đông / Đầu xe) ⭐ | Phương án B: Gắn trên mũ bảo hiểm (Smart Helmet) |
| :--- | :--- | :--- |
| **1. Độ ổn định góc nhìn (FOV & ROI)** | **Rất cao (Tối ưu cho AI):** Camera luôn hướng thẳng theo trục chuyển động của xe. Biển báo luôn nằm ở góc trên bên phải khung hình $\rightarrow$ Dễ dàng thu hẹp vùng nhận diện quan tâm (Region of Interest - ROI), giảm tải tới $40\%$ khối lượng tính toán. | **Rất kém:** Người lái liên tục quay đầu kiểm tra gương, nhìn người đi bên cạnh, cúi xuống nhìn đồng hồ $\rightarrow$ Khung hình camera bị xoay lệch liên tục, mô hình dễ bắt nhầm vật thể ngoài lề đường hoặc bỏ sót biển báo. |
| **2. Nguồn điện & Khối lượng** | **Không lo giới hạn nguồn:** Có thể cắm trực tiếp vào cổng sạc USB 5V xe máy, đấu nối ắc-quy 12V qua IC hạ áp LM2596, hoặc đặt sạc dự phòng trong hộc xe. Không lo trọng lượng thiết bị. | **Hạn chế nghiêm ngặt:** Pin lithium bắt buộc phải siêu nhỏ để không làm nặng đầu người đội. Mạch nhúng chạy camera AI liên tục ăn dòng cao ($400 - 800\text{mA}$), pin nhỏ chỉ trụ được $30 - 45$ phút. |
| **3. An toàn giao thông & Pháp lý** | **An toàn tuyệt đối:** Tương tự như việc gắn giá đỡ điện thoại di động hay camera hành trình xe máy, không gây ảnh hưởng đến phần cứng hay cơ thể người lái. | **Rủi ro chấn thương:** Việc gắn vật cứng, bo mạch sắc nhọn và pin nén lên vỏ mũ bảo hiểm làm phá vỡ kết cấu phân tán lực khi va chạm (vi phạm quy chuẩn kỹ thuật quốc gia QCVN 2:2008/BKHCN về mũ bảo hiểm). |
| **4. Cơ cấu cảnh báo tài xế (HMI)** | **Trực quan & Hiệu quả:** Lắp màn hình OLED mini (0.96 inch hoặc 1.3 inch) hiển thị icon biển báo + còi Buzzer/đèn LED chớp đỏ ngay trên ghi-đông, lọt vào tầm mắt ngoại vi của tài xế mà không gây mất tập trung. | **Cực kỳ phức tạp:** Đòi hỏi phải làm thấu kính chiếu nổi HUD (Head-Up Display) siêu đắt và khó chế tạo, hoặc phải đấu nối thêm tai nghe Bluetooth vào trong lớp mút mũ. |
| **5. Độ khả thi làm đồ án 10 tuần** | **Cực kỳ khả thi (100%):** Nhóm dễ dàng đóng gói mạch vào hộp nhựa in 3D/mica, gắn lên ghi-đông xe máy thực tế hoặc giá đỡ sa bàn để quay video demo và chấm điểm trực quan trên lớp. | **Rủi ro trễ tiến độ:** Nhóm sẽ phải tốn hàng tuần lễ cho việc chế tác cơ khí, khoét mút mũ, cân bằng đối trọng mà không nâng cao giá trị kỹ thuật cốt lõi môn học (AI & Nhúng). |

> [!IMPORTANT]
> **KẾT LUẬN & ĐỀ XUẤT:**
> Nhóm **nhất trí chọn Phương án A: Gắn trực tiếp trên xe máy (vị trí ghi-đông / mặt nạ đầu xe)** làm sản phẩm thực nghiệm chính của Đồ án môn học CE340.  
> Phương án B (Mũ bảo hiểm thông minh) sẽ được đưa vào phần **"Hướng nghiên cứu và phát triển mở rộng trong tương lai"** để khẳng định tầm nhìn công nghệ dài hạn của đề tài.

---

## III. KIẾN TRÚC PHẦN CỨNG & NGUYÊN LÝ HOẠT ĐỘNG ĐỀ XUẤT

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│               SƠ ĐỒ KHỐI HỆ THỐNG NHẬN DIỆN BIỂN BÁO GIAO THÔNG CHO XE MÁY            │
└────────────────────────────────────────────────────────────────────────────────────────┘

    [ Khối Thu Nhận Hình Ảnh ]                     [ Khối Xử Lý Trung Tâm Edge AI ]
   ┌───────────────────────────┐                  ┌─────────────────────────────────┐
   │ Camera Góc Rộng           │                  │ Bo Mạch Nhúng                   │
   │ (OV2640 / MIPI CSI Camera)│──[Video Stream]─>│ (ESP32-S3-CAM / Raspberry Pi)   │
   │ Gắn mặt nạ đầu xe máy     │                  │ • Tiền xử lý & Cân bằng sáng     │
   └───────────────────────────┘                  │ • Chạy Mô Hình Nén INT8         │
                                                  │ • Bộ phân loại biển báo QCVN 41 │
                                                  └────────────────┬────────────────┘
                                                                   │
                                                      [Tín hiệu cảnh báo tức thời]
                                                                   │
                                                                   ▼
                                                  [ Khối Giao Diện Cảnh Báo Ghi-đông ]
                                                  ┌─────────────────────────────────┐
                                                  │ • Màn hình OLED 0.96"/1.3" I2C  │
                                                  │   (Hiển thị icon biển báo)      │
                                                  │ • Còi Buzzer 5V (Cảnh báo âm)   │
                                                  │ • Đèn LED Chớp đỏ (Cảnh báo mắt)│
                                                  └─────────────────────────────────┘
```

### Nguyên lý hoạt động:
1. **Thu nhận hình ảnh:** Camera gắn trước đầu xe liên tục thu nhận luồng video với độ phân giải tối ưu ($320\times320$ hoặc $416\times416$ pixel) để đảm bảo tốc độ xử lý thời gian thực.
2. **Tiền xử lý & Lọc nhiễu thời tiết:** Thuật toán tự động cân bằng tương phản (*CLAHE*) hỗ trợ điều kiện thiếu sáng hoặc mưa bám kính; áp dụng kỹ thuật cắt vùng ảnh quan tâm (ROI) ở nửa trên bên phải làn đường.
3. **Nhận diện bằng mô hình nén TinyML:** Mô hình học sâu đã được lượng tử hóa số nguyên **INT8** (YOLOv8n / MobileNetV2-SSD) thực hiện phát hiện bounding box và phân loại biển báo (Biển cấm, Biển nguy hiểm, Biển giới hạn tốc độ).
4. **Kích hoạt cảnh báo:**
   * Nếu phát hiện biển báo nguy hiểm/cấm (như cấm đi ngược chiều, cấm rẽ, giới hạn tốc độ): Kích hoạt còi Buzzer kêu bíp ngắn + LED đỏ chớp + hiện icon trực quan trên màn hình OLED gắn ghi-đông.
   * Toàn bộ quá trình từ lúc camera thấy biển báo đến khi còi kêu chỉ mất **$< 150\text{ms}$**, đảm bảo tài xế kịp phản ứng an toàn.

---

## IV. BẢN ĐỒ PHÂN CÔNG CHUYÊN TRÁCH 5 THÀNH VIÊN

Để đảm bảo tiến độ nghiệm thu ở **Đợt 1 (Tuần 8 – 09/12/2026)** và không ai bị nghẽn việc của nhau:

| STT | Thành viên | MSSV | Vai trò chuyên trách | Đầu việc cụ thể trong đồ án CE340 |
| :---: | :--- | :---: | :--- | :--- |
| 1 | **Trần Hoàng Hôn** | `26410046` | **Nhóm trưởng & Tích hợp Hệ thống** | • Thiết kế kiến trúc tổng thể phần cứng gắn ghi-đông xe máy.<br>• Lập trình giao tiếp ngoại vi (OLED I2C, Buzzer, LED báo hiệu).<br>• Viết Đề cương chi tiết (Word), quản lý tiến độ và slide báo cáo. |
| 2 | **Nguyễn Duy Khang** | `26410055` | **Kỹ sư Dữ liệu & Tăng cường ảnh** | • Thu thập và gán nhãn tập dữ liệu biển báo giao thông Việt Nam (Roboflow / LabelImg).<br>• Xây dựng pipeline tăng cường ảnh thời tiết phức tạp (*Albumentations*: giả lập mưa nhân tạo, làm mờ chuyển động *Motion Blur*, giả lập che khuất *Random Occlusion*). |
| 3 | **Phạm Thành Trung** | `26410141` | **Huấn luyện Đa mô hình & Benchmark** | • Thiết kế và huấn luyện các kiến trúc mô hình: YOLOv8n, YOLOv5-tiny, MobileNetV2-SSD.<br>• Đánh giá và lập bảng so sánh hiệu năng trước khi nén (`Precision`, `Recall`, `mAP@0.5`, `F1-Score`). |
| 4 | **Vũ Văn Duy** | `26410031` | **Kỹ thuật nén TinyML (Optimization)** | • Thực hiện kỹ thuật cắt tỉa mô hình (**Pruning**).<br>• Thực hiện kỹ thuật lượng tử hóa (**Post-Training Quantization INT8**) trên TensorFlow Lite / ONNX.<br>• Đo đạc và phân tích sự suy giảm độ chính xác so với độ nén dung lượng và tốc độ tính toán. |
| 5 | **Hoàng Cao Sơn** | `25730061` | **Triển khai Nhúng & Đo lường thực nghiệm** | • Nạp mô hình nén INT8 xuống kit phần cứng thật (ESP32-S3 hoặc Raspberry Pi).<br>• Thiết kế hộp vỏ bảo vệ mạch demo gắn ghi-đông.<br>• Đo đạc các thông số phần cứng thực tế: Độ trễ suy luận (`Latency` ms), Tốc độ khung hình (`FPS`), Bộ nhớ `RAM/Flash` chiếm dụng, Dòng điện tiêu thụ `mA`. |

---

## V. TÀI LIỆU LIÊN KẾT TRONG THƯ MỤC MÔN HỌC

* [Kế hoạch học tập CE340](file:///d:/Trí%20tuệ%20nhân%20tạo/HK3/Trí%20tuệ%20nhân%20tạo%20cho%20hệ%20thống%20nhúng/Kế%20hoạch%20học%20tập%20CE340%20-Blended%20Learning%20-%20HK1%202026-2027.xlsx)
* [Tổng quan nội dung & Mục tiêu môn học](file:///d:/Trí%20tuệ%20nhân%20tạo/HK3/Trí%20tuệ%20nhân%20tạo%20cho%20hệ%20thống%20nhúng/Tong_Quan_Noi_Dung_Va_Muc_Tieu_Mon_Hoc_CE340.md)
* [Mẫu đề cương chi tiết CITD CE340](file:///d:/Trí%20tuệ%20nhân%20tạo/HK3/Trí%20tuệ%20nhân%20tạo%20cho%20hệ%20thống%20nhúng/1.%20Mẫu%20Đề%20cương%20chi%20tiết%20CITD%20CE340.docx)
* [Biên bản họp nhóm Tuần 2 chốt đề tài](file:///d:/Trí%20tuệ%20nhân%20tạo/HK3/Thảo%20luận%20nhóm/Tuần%202/Bien_Ban_Hop_Nhom_Tuan_2.md)
