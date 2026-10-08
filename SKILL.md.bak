---
name: srt-whiteboard-animation
description: "Quy chuẩn & Động cơ Sản xuất Video Hoạt hình Vẽ tay Bảng trắng & Màu nước (SRT Whiteboard & Watercolor Animation Engine v2.0). Hỗ trợ nạp Ảnh Gốc Đầu Vào (Reference Image) và dùng I2I (Image-to-Image / Google Flow I2I) để tạo chuỗi ảnh phân cảnh Màu Nước (Watercolor) nhất quán 100% về nhân vật/chủ thể theo phụ đề SRT. Điều phối luồng vẽ tay liên tục: Nét mực phác thảo (ink outline) → Loang màu nước (watercolor fill) bằng con trỏ tay drawing-hand.png trên nền giấy màu be ấm."
version: 2.0.0
---

# ✍️ SRT WHITEBOARD & WATERCOLOR ANIMATION ENGINE (v2.0.0)
## ĐỘNG CƠ HOẠT HÌNH VẼ TAY BẢNG TRẮNG & MÀU NƯỚC THEO NỘI DUNG / ẢNH ĐẦU VÀO

> **Tổng quan:** Chuyển đổi phụ đề SRT và **Ảnh Gốc Đầu Vào (Reference Input)** thành video hoạt hình vẽ tay bảng trắng kết hợp tranh màu nước nghệ thuật (`Watercolor Illustration`).  
> **Điểm đột phá v2.0:** Tích hợp **I2I (Image-to-Image)** để khóa nhân vật/chủ thể gốc, sinh ra chuỗi ảnh phân cảnh đồng nhất phong cách màu nước loang tự nhiên, sau đó mô phỏng nét vẽ tay liên tục (`Stream Ink Outline → Watercolor Wash Fill`) theo từng câu phụ đề.

---

## 🎨 §1. QUY CHUẨN SINH ẢNH I2I MÀU NƯỚC NHẤT QUÁN (I2I WATERCOLOR STANDARD)

### 1. Cơ chế nhận diện & Khóa chủ thể từ Ảnh Gốc (Reference-Driven Lock):
* **Đầu vào (Input):** 1 hoặc nhiều ảnh gốc của người dùng (Ảnh chân dung nhân vật, sản phẩm, linh vật hoặc phong cảnh mẫu).
* **Động cơ I2I (Google Flow I2I / SD I2I / Midjourney):**
  * Sử dụng ảnh gốc làm ảnh tham chiếu (`Reference Image / Character Lock / Face-Lock`).
  * Giữ nguyên 100% đặc điểm nhận diện: Cấu trúc gương mặt, kiểu tóc, dáng vóc, trang phục, hoặc hình khối sản phẩm.
  * Chuyển đổi phong cách sang **Tranh Màu Nước Nghệ Thuật (Artistic Watercolor Sketch)**.

### 2. Tiêu chuẩn Phong cách Visual Màu Nước (Visual Guidelines):
* **Nền giấy (Canvas Background):** Nền giấy màu be ấm có vân texture (`Warm Creamy Textured Paper`, mã màu `#F5EBD7`), tuyệt đối không dùng nền trắng tinh đơn điệu.
* **Nét vẽ (Outline):** Nét chì phác thảo mềm mại hoặc nét mực mảnh (`delicate pencil sketch & fine ink contours`) làm khung cho các chi tiết chính.
* **Màu sắc (Coloring):** Loang màu nước trong trẻo (`soft translucent watercolor washes, wet-on-dry texture`), màu sắc nhã nhặn, có khoảng loang tự nhiên và vùng chừa sáng (negative white space).
* **Cấm kỵ (Negative Constraints):** Không chèn chữ/text thô vào ảnh nguồn (text do phụ đề/animation đảm nhiệm), không hiệu ứng 3D CGI nhựa bóng, không chi tiết rác gây rối mắt khi vẽ.

---

## ⚙️ §2. THAM SỐ VẼ TAY & HIỆU ỨNG (ANIMATION PARAMETERS)

| Hạng mục | Quy chuẩn mặc định |
|---|---|
| **Nền Canvas** | Nền giấy be vân hạt ấm `#F5EBD7`, lấy mẫu màu từ 4 góc ảnh nguồn. |
| **Quy trình vẽ** | Stream liên tục 2 giai đoạn: **Nét mực / Chì phác thảo (`ink`)** $\rightarrow$ **Loang màu nước (`color fill`)** theo tỷ lệ thời lượng `ink : color = 2 : 1`. |
| **Đường dẫn nét vẽ** | `--ink-path grid` (mặc định cho bố cục tổng thể) hoặc `--ink-path skeleton` (bám sát đường viền tranh màu nước). |
| **Kiểu quét màu** | `--color-fill contour-wipe` (quét loang màu theo đường bao) hoặc `--color-fill brush` (quét cọ mềm). |
| **Con trỏ tay vẽ** | Bàn tay cầm bút sạch [`assets/drawing-hand.png`](file:///C:/Users/user/.gemini/config/skills/srt-whiteboard-animation/assets/drawing-hand.png) (đã khử 100% watermark/chữ thừa). |
| **Phân vùng che chắn** | Sử dụng `protectedRegions` để bảo vệ các chi tiết đè lên nhau, đảm bảo vẽ lớp nền trước, chủ thể sau, không lộ trước nét vẽ. |
| **Thời lượng cảnh** | `sceneDurationMs` đồng bộ chính xác theo từng đoạn thời gian trong file `.srt` (20 – 35 giây/cảnh). |

---

## 🚀 §3. QUY TRÌNH THỰC THI 6 BƯỚC (END-TO-END PIPELINE)

```
 [ ẢNH GỐC ĐẦU VÀO + FILE PHỤ ĐỀ SRT ]
                  │
                  ▼
 [ BƯỚC 1: PHÂN TÍCH SRT & LÊN PHÂN CẢNH ] ──► Xác định nội dung & thời lượng từng scene
                  │
                  ▼
 [ BƯỚC 2: I2I GEN ẢNH MÀU NƯỚC NHẤT QUÁN ] ──► Dùng I2I biến đổi ảnh gốc thành bộ cảnh Watercolor
                  │
                  ▼
 [ BƯỚC 3: PHÂN VÙNG VẼ (ANNOTATION JSON) ] ──► Gắn nhãn toạ độ: Nền → Nhân vật → Chi tiết hành động
                  │
                  ▼
 [ BƯỚC 4: XEM TRƯỚC TRÊN PREVIEW STUDIO ] ──► Mở `assets/preview.html` kiểm tra timeline nét vẽ
                  │
                  ▼
 [ BƯỚC 5: RENDER HOẠT HÌNH VẼ TAY MP4 ] ──► Render nét vẽ bút dạ/chì + loang màu nước
                  │
                  ▼
 [ BƯỚC 6: GHÉP AUDIO & XUẤT MASTER FINAL ] ──► Hòa trộn giọng đọc TTS + Foley tiếng vẽ + MP4 hoàn chỉnh
```

### Chi tiết từng bước:
1. **Bước 1 - Đọc phụ đề SRT & Chia phân cảnh:** Dùng `scripts/parse_srt.py` bóc tách từng mốc câu nói (mỗi phân cảnh 25–35s).
2. **Bước 2 - Tạo ảnh Watercolor bằng I2I:** Nạp ảnh gốc của người dùng vào công cụ I2I (như `google-flow-i2i-director`) để sinh các ảnh phân cảnh màu nước đồng nhất về nhân vật và môi trường.
3. **Bước 3 - Lập bản đồ Annotation:** Đọc ảnh và tạo file `<tên_cảnh>.annotation.json`, xác định thứ tự nét vẽ: *Bối cảnh $\rightarrow$ Nhân vật chính $\rightarrow$ Hành động $\rightarrow$ Kết quả*.
4. **Bước 4 - Tinh chỉnh trên Preview GUI:** Mở trình duyệt với `assets/preview.html` để kiểm tra trực quan các khung vẽ, thời gian xuất hiện ăn khớp với phụ đề.
5. **Bước 5 - Render video từng cảnh:** Chạy script `render_stream_whiteboard.py` để kết xuất video có bàn tay vẽ từng nét bút mực và loang màu nước.
6. **Bước 6 - Hợp nhất & Xuất bản:** Ghép các cảnh bằng `merge_scenes.py`, lồng file âm thanh giọng đọc và xuất video MP4 1080p sắc nét.

---

## 📁 §4. QUY ƯỚC CẤU TRÚC THƯ MỤC

```text
assets/whiteboard/<tên_dự_án>/
  ├── reference-original.png               # Ảnh gốc đầu vào của người dùng
  ├── scene-01-watercolor.png              # Ảnh màu nước I2I đã sinh
  ├── scene-01-watercolor.annotation.json  # File toạ độ & thứ tự vẽ (đồng tên với ảnh)
  ├── scene-01-whiteboard.mp4              # Video vẽ tay từng phân cảnh
  └── final-short-video.mp4                # Video hoàn thiện ghép Audio & Subtitle
```

---

## 🛠️ §5. LỆNH VẬN HÀNH BẰNG CLI

1. **Phân tích phụ đề SRT:**
   ```bash
   python scripts/parse_srt.py <phu_de.srt> --target-sec 30
   ```
2. **Xem trước bản đồ phân vùng vẽ:**
   ```bash
   python scripts/render_annotation_preview.py <anh_watercolor.png> <annotation.json> <preview.png>
   ```
3. **Mở giao diện tinh chỉnh Preview Studio:**
   * Mở trực tiếp file [`assets/preview.html`](file:///C:/Users/user/.gemini/config/skills/srt-whiteboard-animation/assets/preview.html) trên trình duyệt Chrome/Edge.
4. **Render video hoạt hình vẽ tay phân cảnh:**
   ```bash
   python scripts/render_stream_whiteboard.py <anh_watercolor.png> <annotation.json> <output.mp4> assets/drawing-hand.png --ink-path skeleton --color-fill contour-wipe
   ```
5. **Hợp nhất nhiều phân cảnh:**
   ```bash
   python scripts/merge_scenes.py --inputs scene-01.mp4 scene-02.mp4 --output final.mp4
   ```

---

## ✅ §6. TIÊU CHUẨN KIỂM ĐỊNH CHẤT LƯỢNG (QA AUDIT)

* [ ] Khung hình bắt đầu bằng nền giấy be `#F5EBD7` sạch sẽ, không bị lộ trước các nét vẽ.
* [ ] Tính nhất quán của nhân vật/sản phẩm qua các cảnh đạt độ tương đồng $\ge 90\%$ so với ảnh gốc nhờ I2I.
* [ ] Nét bút `drawing-hand.png` bám sát đường vẽ, chuyển động mượt mà tự nhiên không bị giật khung hình.
* [ ] Thứ tự xuất hiện các hình vẽ ăn khớp 100% theo nội dung câu từ của phụ đề SRT.
* [ ] Kết thúc mỗi cảnh có ít nhất 0.5s dừng lại để người xem chiêm ngưỡng toàn bộ tác phẩm màu nước hoàn chỉnh.
