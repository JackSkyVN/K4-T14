# Problem backlog

Những chỗ gặp trong lúc gán nhãn mà **guideline chưa trả lời được**, cộng các pain point về công cụ.

Ghi ngay khi gặp, kể cả lúc chưa biết xử lý thế nào. Một edge case không được ghi lại thì
mỗi người sẽ tự xử lý theo một kiểu — và đó là nguồn lớn nhất của nhãn không nhất quán.

## Danh sách

| Mã | Tóm tắt | Loại | Mục guideline | Trạng thái | Kết quả |
|---|---|---|---|---|---|
| [P-001](#p-001) | Không rõ định nghĩa của `area/drivable`, `area/alternative` | Guideline mơ hồ | §4.1 | ✅ Đã chốt | [QĐ-004](so-quyet-dinh.md#qđ-004) |
| [P-002](#p-002) | Đèn giao thông (`traffic light`): vẽ cả cột đèn hay chỉ cụm hộp đèn | Guideline mơ hồ | §3 | ✅ Đã chốt | [QĐ-003](so-quyet-dinh.md#qđ-003) |
| [P-003](#p-003) | Vật bị phản chiếu (trên kính, gương chiếu hậu) có gán nhãn hay không | Guideline mơ hồ | §3 | ↗️ Hỏi Mentor | — |

**Loại**

| Loại | Nghĩa là |
|---|---|
| Guideline chưa nói tới | Tình huống không có trong guideline |
| Guideline mơ hồ | Đọc guideline ra được hai cách hiểu trở lên |
| Guideline mâu thuẫn | Hai mục trong guideline nói ngược nhau |
| Pain point công cụ | Guideline rõ, nhưng làm trên CVAT chậm hoặc dễ sai |

**Trạng thái:** 🔴 Mở · 🗣️ Đang bàn · ↗️ Hỏi BTC / Mentor · ✅ Đã chốt (trỏ sang QĐ) · 🛠️ Làm tool (trỏ sang `source-tool/`) · ⚪ Bỏ (ghi lý do)

---

## P-001

**Không rõ định nghĩa của `area/drivable`, `area/alternative`**

- **Loại:** Guideline mơ hồ
- **Mục guideline:** §4.1 — Polygon – Drivable Area
- **Người phát hiện:** @emsiCUD · 16/09/2026
- **Link CVAT:**
  - [https://cvat.note.transformerlabs.ai/tasks/132/jobs/1378](https://cvat.note.transformerlabs.ai/tasks/132/jobs/1378?frame=17)
- **Mô tả:** Không rõ ranh giới phân định và cách phân biệt nhãn giữa `area/drivable` (vùng đường xe có thể di chuyển trực tiếp) và `area/alternative` (vùng đường thay thế/làn đường khác).
- **Các cách hiểu:**
  1. `area/drivable`: Toàn bộ phần đường xe có thể di chuyển trực tiếp, bao gồm cả các vùng có vạch sơn kẻ đường.
  2. `area/alternative`: Các vùng đường thay thế hoặc làn đường không nằm trong hướng di chuyển chính.
- **Xử lý tạm trong lúc chờ:** Vẽ Polygon trùm toàn bộ diện tích phần đường xe có thể chạy được cho `area/drivable`.
- **Kết quả:** ✅ [QĐ-004](so-quyet-dinh.md#qđ-004)

## P-002

**Đèn giao thông (`traffic light`): vẽ cả cột đèn hay chỉ cụm hộp đèn**

- **Loại:** Guideline mơ hồ
- **Mục guideline:** §3 — Quy tắc Bounding Box
- **Người phát hiện:** @emsiCUD · 16/09/2026
- **Link CVAT:**
  - [https://cvat.note.transformerlabs.ai/tasks/132?frame=](https://cvat.note.transformerlabs.ai/tasks/132/jobs/1378?frame=5)
- **Mô tả:** Không rõ đối với nhãn `traffic light` thì vẽ bounding box bao phủ cả cột đèn/thanh treo hay chỉ vẽ cụm hộp đèn.
- **Các cách hiểu:**
  1. Vẽ bounding box bao gồm cả cột đèn/giá treo.
  2. Chỉ vẽ bounding box ôm sát cụm hộp đèn (biển chứa bóng đèn).
- **Xử lý tạm trong lúc chờ:** Chốt quy định nhóm.
- **Kết quả:** ✅ [QĐ-003](so-quyet-dinh.md#qđ-003)

## P-003

**Các vật bị phản chiếu (trên bề mặt bóng, kính, gương chiếu hậu) có gán nhãn hay không**

- **Loại:** Guideline mơ hồ
- **Mục guideline:** §3 — Quy tắc Bounding Box ("Không annotate reflection...")
- **Người phát hiện:** @emsiCUD · 16/09/2026
- **Link CVAT:**
  - [https://cvat.note.transformerlabs.ai/tasks/132](https://cvat.note.transformerlabs.ai/tasks/132/jobs/1381?frame=76)
- **Mô tả:** Hình ảnh xe phản chiếu qua cửa kính tòa nhà, bề mặt kính xe khác hoặc xuất hiện trong gương chiếu hậu. Cần xác định rõ có gán nhãn Bounding Box hay không.
- **Các cách hiểu:**
  1. Không gán nhãn cho tất cả hình ảnh phản chiếu (theo quy tắc §3 - reflection).
  2. Vẫn gán nhãn nếu xe phản chiếu trong gương chiếu hậu rõ nét.
- **Xử lý tạm trong lúc chờ:** Tạm thời không gán nhãn các bóng/hình phản chiếu và gửi câu hỏi chờ Mentor chốt.
- **Kết quả:** ↗️ Hỏi Mentor

---

## Mẫu để copy

```markdown
## P-NNN

**Tóm tắt một dòng**

- **Loại:** Guideline chưa nói tới | Guideline mơ hồ | Guideline mâu thuẫn | Pain point công cụ
- **Mục guideline:** §
- **Người phát hiện:** @ · dd/mm/yyyy
- **Link CVAT:** (bỏ trống nếu không có)
  - https://…/tasks/<id>/jobs/<id>?frame=<n> — frame này có gì
- **Mô tả:**
- **Các cách hiểu:** (với pain point công cụ thì ghi **Hướng đang cân nhắc:**)
  1.
  2.
- **Xử lý tạm trong lúc chờ:**
- **Kết quả:** 🔴 Mở
```

Nhớ thêm một dòng vào bảng **Danh sách** ở đầu file.
