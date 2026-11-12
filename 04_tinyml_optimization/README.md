# 04_tinyml_optimization — Tối ưu hóa TinyML (Pruning & Quantization INT8)

* **Phụ trách chính:** Hoàng Cao Sơn (`[S1]`, `[S2]`)
* **Sản phẩm Tuần 4:**
  * `tinyml_toolchain_notes.md` (tách rõ luồng `tfmot` cho TensorFlow/Keras và `torch.nn.utils.prune -> ONNX -> onnx2tf` cho PyTorch + bảng kiểm ops `tflite-micro`)
  * `quantize_int8_mock.py` (Full Integer Quantization `INT8` bằng `representative_dataset`)
  * `mock_model_int8.h` (mảng C-array xuất cho firmware `ESP32-S3`)

