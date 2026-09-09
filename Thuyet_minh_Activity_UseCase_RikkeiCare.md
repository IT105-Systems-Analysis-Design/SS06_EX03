# THUYẾT MINH — ACTIVITY DIAGRAM & USE CASE DIAGRAM
## Hệ thống đặt lịch khám RikkeiCare

---

## PHẦN A — Activity Diagram

### Kịch bản
Bệnh nhân đặt lịch khám. Lễ tân kiểm tra khung giờ:
- Nếu **Còn trống** → Lễ tân vừa **xác nhận lịch khám** vừa **gửi SMS nhắc lịch cho Bệnh nhân** cùng lúc (song song) → kết thúc.
- Nếu **Hết chỗ** → Lễ tân **báo chọn khung giờ khác** → kết thúc.

### Bảng phân rã Node — Swimlane (đã hoàn thiện)

| Loại Node | Tên Node / Tác vụ | Swimlane phụ trách |
|---|---|---|
| Initial Node | Bắt đầu | — |
| Action | Đặt lịch khám | Bệnh nhân |
| Decision | Kiểm tra khung giờ (Còn trống / Hết chỗ) | Lễ tân |
| **Fork** | **Phân luồng xử lý** — tách luồng "Còn trống" thành 2 nhánh song song (xác nhận lịch và gửi SMS nhắc lịch chạy đồng thời) | Lễ tân |
| Action | Xác nhận lịch khám | **Lễ tân** |
| Action | Gửi SMS nhắc lịch | **Lễ tân** |
| **Join** | **Gộp luồng xử lý** — chờ cả 2 nhánh song song (Xác nhận lịch, Gửi SMS) hoàn tất rồi mới đi tiếp | Lễ tân |
| Action (nhánh phụ) | Báo chọn khung giờ khác (khi Hết chỗ) | Lễ tân |
| Final Node | Kết thúc | — |

> Ghi chú: Sau Join (nhánh Còn trống) và sau Action "Báo chọn khung giờ khác" (nhánh Hết chỗ) đều dẫn tới cùng một Final Node — một Final Node được phép nhận nhiều luồng vào, mỗi luồng tới đều kết thúc tiến trình.

### Mô tả luồng chạy
1. **Initial Node** → Action "Đặt lịch khám" (Bệnh nhân).
2. Chuyển sang làn Lễ tân → **Decision** "Kiểm tra khung giờ".
3. Nhánh **Còn trống** → **Fork** tách song song 2 action: "Xác nhận lịch khám" và "Gửi SMS nhắc lịch" → **Join** gộp lại → **Final Node**.
4. Nhánh **Hết chỗ** → Action "Báo chọn khung giờ khác" → **Final Node**.

*(Xem file đính kèm `Activity_Diagram_RikkeiCare.svg`.)*

---

## PHẦN B — Use Case Diagram

### Kịch bản bổ sung
- Bệnh nhân phải **Đăng nhập** trước khi **Đặt lịch khám** — bắt buộc.
- Bệnh nhân có thể tuỳ chọn **Chọn bác sĩ chỉ định** khi đặt lịch — không bắt buộc.
- Đặt lịch khám có 2 hình thức: **Đặt lịch khám thường** và **Đặt lịch khám ưu tiên** (phụ phí) — cả hai là dạng chuyên biệt của Đặt lịch khám.

### Bảng quan hệ Use Case (đã hoàn thiện)

| Use Case A | Use Case B | Quan hệ | Giải thích logic |
|---|---|---|---|
| Đặt lịch khám | Đăng nhập | «include» | Phải đăng nhập trước khi đặt lịch khám — bước bắt buộc, luôn xảy ra mỗi khi thực hiện Đặt lịch khám. |
| Đặt lịch khám | Chọn bác sĩ chỉ định | «extend» | **Bệnh nhân có thể tuỳ chọn chỉ định một bác sĩ cụ thể ngay khi đặt lịch. Đây là hành vi mở rộng, không bắt buộc — chỉ xảy ra khi bệnh nhân chủ động chọn tại một điểm mở rộng (extension point) của Use Case Đặt lịch khám; nếu không chọn, hệ thống sẽ tự động phân bác sĩ theo lịch trống.** |
| Đặt lịch khám | Đặt lịch khám thường | Generalization | Là một dạng chuyên biệt của Đặt lịch khám (kế thừa hành vi chung, xếp lịch theo thứ tự thông thường, không phụ phí). |
| Đặt lịch khám | **Đặt lịch khám ưu tiên** | Generalization | **Là một dạng chuyên biệt của Đặt lịch khám, cho phép bệnh nhân trả thêm phụ phí để được ưu tiên xếp lịch/khám trước — kế thừa hành vi chung của Đặt lịch khám nhưng có thêm quy tắc riêng về thứ tự ưu tiên và tính phí.** |

### Thành phần Use Case Diagram
- **Actor**: Bệnh nhân.
- **System Boundary**: "Hệ thống RikkeiCare".
- **Use Case** bên trong boundary: Đăng nhập, Đặt lịch khám, Đặt lịch khám thường, Đặt lịch khám ưu tiên, Chọn bác sĩ chỉ định.
- **Association**: Bệnh nhân — Đăng nhập; Bệnh nhân — Đặt lịch khám (actor tham gia trực tiếp 2 use case gốc này; Chọn bác sĩ chỉ định được kích hoạt gián tiếp qua điểm mở rộng của Đặt lịch khám).
- **Include**: Đặt lịch khám → Đăng nhập (mũi tên nét đứt, đầu mũi tên hở, hướng từ Use Case gốc tới Use Case bắt buộc được gộp vào).
- **Extend**: Chọn bác sĩ chỉ định → Đặt lịch khám (mũi tên nét đứt, đầu mũi tên hở, hướng từ Use Case mở rộng tới Use Case gốc).
- **Generalization**: Đặt lịch khám thường → Đặt lịch khám; Đặt lịch khám ưu tiên → Đặt lịch khám (đường nét liền, đầu mũi tên tam giác rỗng, hướng từ lớp con tới lớp cha).

*(Xem file đính kèm `UseCase_Diagram_RikkeiCare.svg`.)*

---

## Kết luận
Bộ đôi Activity Diagram và Use Case Diagram trên đã mô hình hóa đầy đủ:
- Luồng xử lý nghiệp vụ có rẽ nhánh (Decision) và xử lý song song (Fork/Join) theo swimlane Bệnh nhân / Lễ tân.
- Cấu trúc chức năng của hệ thống với đầy đủ Actor, System Boundary, Use Case, và 3 loại quan hệ chuẩn UML: Include (bắt buộc), Extend (tuỳ chọn), Generalization (kế thừa).
