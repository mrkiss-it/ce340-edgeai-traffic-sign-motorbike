# 05_firmware_esp32s3 — Phần cứng, Ngoại vi & Firmware TFLite Micro

* **Phụ trách chính:** Vũ Văn Duy (`[D1]`, `[D2]`), Trần Hoàng Hôn (`[H2]` ngoại vi `OLED`, `Buzzer`, `LED`)
* **Sản phẩm Tuần 4:**
  * `bom_and_hardware_specs.md` (chốt đúng mã bo `ESP32-S3-CAM N16R8`, Pinout né `GPIO 33–37` PSRAM, BOM & hộp ghi-đông)
  * `wiring_diagram.md` & `peripherals_test/` (`OLED I2C`, `Buzzer 5V`, `LED`)
  * `mock_inference_test/` (compile-test `OV2640` + `tflite-micro` nạp thử stub `INT8` & đo `Flash`/`PSRAM Tensor Arena`)

