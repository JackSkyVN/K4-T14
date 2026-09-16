# Sổ quyết định



## Danh sách

| Mã | Quyết định | Ngày | Xuất phát từ | Trạng thái |
|---|---|---|---|---|
| [QĐ-001](#qđ-001) | Cách vẽ Polyline cho Lane Marking: vẽ 1 đường đi giữa tim vạch, không vẽ sát biên | 16/09/2026 | Guideline §4.2 | Hiệu lực |
| [QĐ-002](#qđ-002) | Ngưỡng gán nhãn vật thể bị che khuất (Occluded): chỉ gán khi nhìn thấy trên 40% | 16/09/2026 | Guideline §3.1 / P-002 | Hiệu lực |
| [QĐ-003](#qđ-003) | Gán nhãn Đèn giao thông (`traffic light`): chỉ vẽ Bounding Box cho cụm hộp đèn | 16/09/2026 | Guideline §3 / [P-001](problem-backlog.md#p-001) | Hiệu lực |
| [QĐ-004](#qđ-004) | Quy tắc vẽ Polygon cho `area/drivable`: vẽ toàn bộ phần đường xe có thể chạy, bao gồm cả vạch kẻ đường | 16/09/2026 | Guideline §4.1 | Hiệu lực |
| [QĐ-005](#qđ-005) | Quy tắc vẽ Polyline cho `lane/road curb`: vẽ bám theo biên trên của gờ đường | 16/09/2026 | Guideline §4.2 | Hiệu lực |

**Trạng thái:** Hiệu lực · Bị thay bởi QĐ-xxx · Huỷ (ghi lý do)

---

## QĐ-001

**Cách vẽ Polyline cho Lane Marking: vẽ 1 đường đi giữa tim vạch, không vẽ sát biên**

- **Ngày:** 16/09/2026
- **Người tham gia:** @hominhhau (chốt), @JackSkyVN, @hynu15, @NDViANh, @emsiCUD
- **Xuất phát từ:** Guideline §4.2 (Polyline – Lane Marking)
- **Bối cảnh:** Vạch kẻ đường (lane marking) có độ rộng bề mặt. Cần thống nhất quy tắc vẽ Polyline đi theo biên hay đi theo tim vạch để đảm bảo tính nhất quán dữ liệu cho nhóm K4-T14.
- **Các phương án đã cân nhắc:**
  1. *Vẽ sát biên hai bên vạch* — tốn thời gian, không đồng nhất giữa các loại vạch và dễ gây nhầm lẫn với Polygon. Loại.
  2. *Vẽ 1 đường đi chính giữa tim vạch* — đúng định nghĩa Polyline §4.2, thao tác nhanh và đảm bảo tính nhất quán cao nhất cho model/evaluation. **Chọn.**
- **Quyết định:** Khi gán nhãn Polyline cho các lớp `lane/*`, tất cả thành viên nhóm K4-T14 bắt buộc vẽ 1 đường Polyline duy nhất chạy chính giữa tim vạch, tuyệt đối không vẽ viền sát biên.
- **Việc phải làm theo:**
  - [x] Thông báo quy định tới toàn bộ thành viên nhóm K4-T14 (@hominhhau)
- **Trạng thái:** Hiệu lực

## QĐ-002

**Ngưỡng gán nhãn vật thể bị che khuất (Occluded): chỉ gán khi nhìn thấy trên 40%**

- **Ngày:** 16/09/2026
- **Người tham gia:** @hominhhau (chốt), @JackSkyVN, @hynu15, @NDViANh, @emsiCUD
- **Xuất phát từ:** Guideline §3.1 & P-002 (Vật thể bị che khuất)
- **Bối cảnh:** Guideline đề cập bật attribute `occluded = true` khi vật thể bị che khuất một phần, nhưng chưa quy định rõ ngưỡng diện tích hiển thị tối thiểu để gán nhãn.
- **Các phương án đã cân nhắc:**
  1. *Gán nhãn mọi vật thể bị che dù chỉ nhìn thấy tỉ lệ nhỏ (< 40%)* — dễ suy đoán class thiếu căn cứ, làm giảm độ chính xác của dữ liệu. Loại.
  2. *Chỉ gán nhãn khi diện tích nhìn thấy được > 40%* — đảm bảo đủ bằng chứng thị giác để xác định chắc chắn class, không đoán mò. **Chọn.**
- **Quyết định:** Đối với các vật thể bị che khuất (`occluded`), chỉ thực hiện gán nhãn (và bật attribute `occluded = true`) nếu phần vật thể nhìn thấy được chiếm **trên 40%** diện tích. Trường hợp nhìn thấy từ **40% trở xuống**, coi như không xác định và **không gán nhãn**.
- **Việc phải làm theo:**
  - [x] Cập nhật quy tắc tới toàn bộ thành viên nhóm K4-T14 (@hominhhau)
- **Trạng thái:** Hiệu lực

## QĐ-003

**Gán nhãn Đèn giao thông (`traffic light`): chỉ vẽ Bounding Box cho cụm hộp đèn**

- **Ngày:** 16/09/2026
- **Người tham gia:** @hominhhau (chốt), @JackSkyVN, @hynu15, @NDViANh, @emsiCUD
- **Xuất phát từ:** Guideline §3 & [P-001](problem-backlog.md#p-001) (Phạm vi vẽ Bounding Box cho Đèn giao thông)
- **Bối cảnh:** Đối với lớp `traffic light`, một số annotator vẽ bounding box bao gồm cả cột đèn hoặc thanh treo, gây thừa vùng nền và làm không đồng nhất nhãn.
- **Các phương án đã cân nhắc:**
  1. *Vẽ bao gồm cả cột đèn/giá treo* — chứa nhiều vùng nền không cần thiết, kích thước nhãn bị méo lệch so với thực tế của đèn. Loại.
  2. *Chỉ vẽ Bounding Box bao vừa sát cụm hộp đèn (phần biển/hộp chứa các đèn)* — đảm bảo bám sát đối tượng, đúng định nghĩa Bounding Box và thống nhất cao. **Chọn.**
- **Quyết định:** Khi gán nhãn cho lớp `traffic light`, tất cả annotator chỉ vẽ Bounding Box bao quanh phần cụm hộp đèn (hộp/biển chứa bóng đèn), tuyệt đối **không vẽ bao gồm cột đèn hay giá treo**.
- **Việc phải làm theo:**
  - [x] Quán triệt quy định tới toàn bộ Annotator và Reviewer nhóm K4-T14 (@hominhhau)
- **Trạng thái:** Hiệu lực

## QĐ-004

**Quy tắc vẽ Polygon cho `area/drivable`: vẽ toàn bộ phần đường xe có thể chạy, bao gồm cả vạch kẻ đường**

- **Ngày:** 16/09/2026
- **Người tham gia:** @hominhhau (chốt), @JackSkyVN, @hynu15, @NDViANh, @emsiCUD
- **Xuất phát từ:** Guideline §4.1 (Polygon – Drivable Area)
- **Bối cảnh:** Cần thống nhất quy tắc khoanh vùng Polygon cho `area/drivable` khi trên mặt đường có các vạch sơn kẻ đường (như vạch trắng, vạch vàng).
- **Các phương án đã cân nhắc:**
  1. *Vẽ né hoặc chia cắt Polygon khi gặp các vạch sơn kẻ đường (vạch trắng, vạch vàng)* — làm vỡ Polygon thành các vùng vụn, không phản ánh đúng toàn bộ vùng đường xe có thể lưu thông. Loại.
  2. *Vẽ Polygon trùm lên toàn bộ diện tích phần đường xe có thể di chuyển, bao gồm cả các vạch kẻ đường vàng/trắng* — đúng bản chất vùng đường lưu thông §4.1, tạo Polygon liên tục, chính xác. **Chọn.**
- **Quyết định:** Khi vẽ Polygon cho nhãn `area/drivable`, annotator bắt buộc vẽ trùm toàn bộ phần diện tích mặt đường xe có thể chạy được. **Bao gồm cả vùng có các vạch sơn kẻ đường (vàng, trắng)**, không cắt nhỏ hay né các vạch kẻ đường này.
- **Việc phải làm theo:**
  - [x] Thông báo tới toàn bộ Annotator và Reviewer nhóm K4-T14 (@hominhhau)
- **Trạng thái:** Hiệu lực

## QĐ-005

**Quy tắc vẽ Polyline cho `lane/road curb`: vẽ bám theo biên trên của gờ đường**

- **Ngày:** 16/09/2026
- **Người tham gia:** @hominhhau (chốt), @JackSkyVN, @hynu15, @NDViANh, @emsiCUD
- **Xuất phát từ:** Guideline §4.2 (Polyline – Lane Marking)
- **Bối cảnh:** Gờ đường/vỉa hè (`lane/road curb`) có độ cao và bề rộng nhất định. Cần thống nhất vị trí vẽ Polyline bám theo biên trên hay biên dưới của road curb.
- **Các phương án đã cân nhắc:**
  1. *Vẽ bám theo biên dưới (nơi tiếp giáp lòng đường)* — dễ bị bóng râm hoặc vết bẩn che khuất, thiếu tính nhất quán. Loại.
  2. *Vẽ bám theo biên trên (mép mép gờ trên tiếp giáp vỉa hè)* — quan sát rõ ràng trên ảnh, dễ nhận biết và đồng nhất dữ liệu. **Chọn.**
- **Quyết định:** Khi vẽ Polyline cho lớp `lane/road curb`, tất cả annotator bắt buộc vẽ đường Polyline bám chính xác theo **biên trên** (mép trên đỉnh) của road curb.
- **Việc phải làm theo:**
  - [x] Phổ biến quy tắc cho toàn bộ Annotator và Reviewer nhóm K4-T14 (@hominhhau)
- **Trạng thái:** Hiệu lực

---

## Mẫu để copy

```markdown
## QĐ-NNN

**Quyết định trong một dòng**

- **Ngày:** dd/mm/yyyy
- **Người tham gia:** @ (chốt), @, @
- **Xuất phát từ:** [P-NNN](problem-backlog.md#p-nnn) | Họp tuần NN | …
- **Bối cảnh:** vì sao phải quyết định
- **Các phương án đã cân nhắc:**
  1. *Phương án* — ưu / nhược. Loại hoặc **Chọn.**
  2. *Phương án* — ưu / nhược. Loại hoặc **Chọn.**
- **Quyết định:** đủ rõ để người không dự họp vẫn làm đúng
- **Việc phải làm theo:**
  - [ ] việc (@người phụ trách)
- **Trạng thái:** Hiệu lực
```

Nhớ thêm một dòng vào bảng **Danh sách** ở đầu file, và đóng mục P-xxx tương ứng trong backlog.
