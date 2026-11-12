# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202603198
- Ngày / CVAT local: 11/11/2026
- Công cụ đã dùng: CVAT, mô hình mobie SAM 

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | 14/14| **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: 000000181542.jpg – class person, khu vực bên trái ảnh, bounding box khoảng (93, 163, 28, 96), annotation ID 1 trong file export. Đây là một người bị các phương tiện và đối tượng xung quanh che khuất một phần
- Class và quy tắc em dùng để chọn biên: Chọn class person. Em bám theo đường viền phần cơ thể và quần áo có thể quan sát được. Tại vị trí người và xe máy tiếp giáp hoặc che khuất nhau, em phân biệt phần cơ thể người với phần phương tiện, không mở rộng mask sang xe máy. Phần bị che khuất không được tự suy diễn thành vùng cơ thể nhìn thấy.

- Nếu dùng gợi ý sau đó: em sử dụng CVAT và MobileSAM để hỗ trợ annotation. Tuy nhiên, file export chỉ lưu mask cuối cùng, không lưu lại mask đề xuất ban đầu của MobileSAM. Vì vậy, em cần đối chiếu lại thao tác thực tế để ghi chính xác vùng đề xuất nào đúng/sai và phần nào được giữ hoặc chỉnh sửa, thay vì suy đoán từ kết quả cuối.

- Nếu không dùng gợi ý: ghi “không dùng”; vẫn giải thích một quyết định gán nhãn của mình.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: ảnh 000000458325.jpg, khu vực giữa lệch trái, tọa độ khoảng x=231–259, y=212–240, annotation ID 67.


- Lỗi thuộc loại: Nghi thừa vật / sai lớp (false positive hoặc phân loại sai).
- Bằng chứng tôi nhìn thấy: Trong instances_default.json, vùng này được gán class motorcycle, với bounding box kích thước khoảng 28 × 28 pixel và diện tích mask 373 pixel. Tuy nhiên, Ground Truth không có annotation motorcycle trong ảnh này. Vùng tương ứng nằm trong khu vực có nhiều người đi bộ; hình dạng phương tiện không đủ rõ để xác nhận đây là một instance motorcycle độc lập.


- Quy tắc và hành động sửa: Chỉ gán class motorcycle khi có đủ đặc điểm hình dạng để nhận diện phương tiện hai bánh độc lập. Tôi cần kiểm tra lại vùng này trên CVAT; nếu đây không phải một phương tiện riêng biệt thì xóa annotation thừa, nếu là phương tiện bị che khuất thì chỉnh lại mask theo phần có thể quan sát được và xác nhận class.

- Sau sửa đã Save và export lại chưa? Đã save và export lại medium_instance.zip.



Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): … / chưa có điểm. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 000000181542.jpg – người mặc áo dài trắng ở giữa ảnh, khoảng x=188–305, y=131–460 | 1. Mask chỉ bao gồm cơ thể và trang phục người. 2. Mask mở rộng sang phần xe máy tiếp giáp hoặc vùng bị che khuất. | Đường viền trang phục có thể phân biệt với bánh xe và phương tiện phía sau. | Giữ mask theo đường viền cơ thể và quần áo nhìn thấy được; không bao gồm xe máy hoặc phần bị che khuất. |
| 000000373353.jpg – người đi bộ phía trước taxi vàng, khoảng x=292–351, y=296–417 | 1. Mask person chỉ bao gồm phần người nhìn thấy. 2. Mask kéo dài qua phần thân người bị taxi che khuất. | Người và xe có hai class riêng; đường viền người được nhận diện qua đầu, thân, chân và quần áo. | Tách instance person khỏi car, không gán phần thân xe vào mask người. Không tự suy diễn phần cơ thể bị che. |
| 000000458325.jpg – hai xe đỗ gần nhau phía bên trái, khu vực khoảng x=150–205, y=185–262 | 1. Hai xe là hai instance riêng biệt. 2. Các vùng mask tiếp giáp có thể bị hiểu thành một instance. | Quan sát đường viền thân xe, cửa kính và vị trí tương đối của từng phương tiện. | Giữ hai instance riêng khi nhận diện được hai thân xe độc lập. Cần coach xác nhận cách xử lý khi ranh giới giữa hai xe bị che khuất gần như hoàn toàn. |
