# Báo cáo cá nhân — HoVaTen-MSSV

Đổi tên file theo quy tắc `MSSV_HoTen.md`. Ví dụ: `20241234_NguyenVanAn.md`.

Dữ liệu: 12 ảnh người COCO val2017 (Lin et al., 2014) và 8 ảnh chân dung Wikimedia Commons, chỉ dùng trong lớp.

## 0. Thông tin học viên

- Họ tên: Nguyễn Ngọc Đồng
- MSSV: 2A202603291
- Email: csjbvhfff@gmail.com
- Link repository: https://github.com/IsMeNotee/K5-L2-DAY04-NguyenNgocDong--2A202603291-KeypointPose-Student
- Phiên bản CVAT: 2.82.0
- Máy: Windows · RAM 12 GB · CVAT Online
- Bộ ảnh `src/exports/assignment.json`: Có
- Giờ bắt đầu: 18h
- Giờ kết thúc: 19h

## 1. Kiểm tra label (CP0)

| Label | Kiểu | Số điểm | Tên điểm đúng bảng của lab? | Đúng thứ tự? | Số cạnh |
|---|---|---:|---|---|---:|
| `person` | Skeleton | 17 | Có | Có | 17 |
| `face` | Skeleton | 13 | Có | Có | 13 |

Mẫu `person` dựng theo người quay mặt về camera: Có.

Cách đổi cờ: Chọn keypoint trong Skeleton, sử dụng thao tác Occluded hoặc Outside tương ứng, sau đó lưu annotation.

## 5. Kiểm file xuất và kết quả tự kiểm (CP4)

- File person: `NguyenNgocDong_persons.zip`
- Số ảnh: 12
- Số Skeleton: 22
- Số phần tử keypoints: 51
- Visible (`v=2`): 262 (70%)
- Occluded (`v=1`): 52 (14%)
- Outside (`v=0`): 60 (16%)
- `num_keypoints` trung bình: 14.3

- File face: `NguyenNgocDong_faces.zip`
- Số ảnh: 8
- Số Skeleton: 8
- Số phần tử keypoints: 39
- Visible (`v=2`): 95 (91%)
- Occluded (`v=1`): 1 (1%)
- Outside (`v=0`): 8 (8%)
- `num_keypoints` trung bình: 12.0

CVAT xuất Occluded thành `v=1`, Outside thành `v=0`. Một số điểm Outside vẫn giữ tọa độ cũ, nhưng không ảnh hưởng đến kết quả kiểm tra.

Kết quả kiểm tra tự động: **ĐẠT — 0 lỗi, 5 cảnh báo.**

Các cảnh báo tập trung vào khả năng đảo trái/phải ở khớp chân và những Skeleton có chiều cao điểm nhỏ hơn 60 px. Cần đối chiếu trực quan để xác nhận các trường hợp này.

## 7. Tự đánh giá

**1. Lỗi dễ mắc và cách phòng tránh:**

Khó khăn chính là phân biệt trái/phải của cơ thể trong các tư thế quay lưng, nghiêng người hoặc bắt chéo chân. Để hạn chế lỗi, cần kiểm tra hướng cơ thể và thứ tự các keypoint trước khi lưu.

**2. Quy tắc đề xuất bổ sung:**

Khi sử dụng AI tự động gán nhãn, cần kiểm tra các điểm bị che khuất, nằm ngoài ảnh và những trường hợp người có tư thế bất thường. Không nên giữ nguyên toàn bộ dự đoán của AI mà không kiểm tra.

**3. Ảnh hưởng của lỗi đến model:**

Nếu nhầm `left_knee` và `right_knee`, mô hình có thể học sai vị trí của các khớp trên heatmap. Khi huấn luyện với phép lật ảnh, việc hoán đổi sai nhãn trái/phải cũng khiến dữ liệu không nhất quán và làm giảm độ chính xác dự đoán tư thế.

## 8. Khó khăn và lỗi kỹ thuật

Trong quá trình thực hành, tôi sử dụng CVAT Online và thử nghiệm chức năng Auto-labeling. Ban đầu gặp khó khăn khi ánh xạ nhãn của model với Skeleton đã tạo trong project.

Sau khi cấu hình đúng mapping giữa `body` và `person`, đồng thời ghép 17 keypoint tương ứng, model Human pose estimation đã có thể tự động dự đoán tư thế người.

Đối với khuôn mặt, kết quả tự động chưa phù hợp với bộ 13 keypoint của bài nên cần điều chỉnh thủ công. Sau khi hoàn thành, tôi xuất dữ liệu và sử dụng script kiểm tra của repo, đạt kết quả 0 lỗi và 5 cảnh báo.

## 9. Tài liệu tham khảo và công cụ AI đã dùng

- CVAT Online: https://app.cvat.ai
- Bộ dữ liệu COCO val2017
- Wikimedia Commons
- Model Human pose estimation tích hợp trong CVAT
- ChatGPT: Hỗ trợ giải thích keypoint, mapping, visibility và hướng dẫn quy trình kiểm tra dữ liệu.
