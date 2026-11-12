# Mock Assets — Bộ dữ liệu & mô hình mẫu kích hoạt Zero-Waiting Pipeline (`[H1]`)

Thư mục này chứa tài nguyên mẫu gọn nhẹ để 4 thành viên còn lại chạy kiểm thử pipeline ngay trong Tuần 4 mà không cần chờ nhau:

1. **`images/mock_01.jpg .. mock_05.jpg` & `labels/mock_01.txt .. mock_05.txt`:**
   * `5` ảnh tổng hợp `320x320` kèm nhãn chuẩn YOLO khớp `12` lớp trong [`classes.yaml`](../classes.yaml) (sử dụng cùng [`data_mock.yaml`](./data_mock.yaml)).
   * Dùng cho **Khang (`[K2]`)** test script `Albumentations` và **Trung (`[T2]`)** chạy smoke-test train `2–5 epochs`.
2. **`mock_model_int8.tflite` & `mock_model_int8.h`:**
   * Mô hình CNN phân loại tối giản `Full Integer Quantization (INT8)` với đầu vào `[1, 160, 160, 3]` (`int8`) và đầu ra `[1, 12]` (`int8`) — đại diện `12` lớp biển báo.
   * Dùng cho **Sơn (`[S2]`)** đối chiếu chuẩn xuất `.tflite`/`.h` và **Duy (`[D2]`)** kiểm tra biên dịch + cấp phát `kTensorArenaSize` trên `tflite-micro` (`ESP32-S3-CAM N16R8`).

