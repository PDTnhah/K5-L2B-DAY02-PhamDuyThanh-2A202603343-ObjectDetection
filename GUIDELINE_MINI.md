# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Kích thước nhỏ gọn, gầm thấp hoặc vừa. Thường có thiết kế chia khoang rõ rệt (capo – khoang người – cốp/đuôi). Thường 4 bánh. | Xe taxi hoặc xe cho mục đích cá nhân, thường là loại xe nhỏ nhất. Phân biệt với `van`: `car` có capo rõ, nóc thấp, đuôi dốc/có cốp. Xe bán tải có thùng hở thấp phía sau vẫn là `car`, không phải `truck`. |
| `truck` | Xe chở hàng cỡ trung/lớn, thân cao: cabin lái tách rời với thùng hàng (thùng kín, thùng hở, thùng ben, bồn) hoặc đầu kéo + rơ-moóc/container. | Ví dụ: xe tải thùng, xe ben, xe bồn, đầu kéo container. Phân biệt: thấy khe/ranh giới rõ giữa cabin và thùng → `truck`; cabin và khoang sau liền một khối → `van`; pickup cabin nhỏ, thùng thấp → `car`. Đầu kéo kèm rơ-moóc là một box chung. |
| `bus` | Xe chở khách cỡ lớn, thân dài hình hộp liền khối, dãy cửa sổ chạy dọc hai bên thân, cửa lên xuống ở hông, đầu xe phẳng. | Ví dụ: xe buýt thành phố, xe khách đường dài, minibus/xe khách cỡ nhỏ có dãy cửa sổ dài. Phân biệt với `truck`: không có thùng hàng tách rời, có cửa sổ suốt thân. Phân biệt với `van`: dài và cao hơn rõ rệt, nhiều hàng cửa sổ hơn. |
| `van` | Xe dạng hộp một khối (cabin và khoang sau chung một thân), đầu xe ngắn, nóc cao hơn xe con, đuôi gần thẳng đứng; chở hàng hoặc chở người. | Ví dụ: minivan, xe van chở hàng, xe 7–16 chỗ dạng hộp. Phân biệt với `car`: nóc cao, capo ngắn, không có cốp thấp. Phân biệt với `bus`: ngắn hơn, chỉ 2–3 hàng cửa sổ. Phân biệt với `truck`: không có thùng hàng tách khỏi cabin. |

## Quy tắc chung

- **Box bám sát vật thể nghĩa là:** bốn cạnh box chạm điểm ngoài cùng của phần thân xe nhìn thấy được (tính cả bánh xe, gương chiếu hậu, thùng hàng/rơ-moóc gắn liền), không lấy thêm bóng đổ, mặt đường, trời hay xe bên cạnh. Mỗi xe là một box riêng, không gộp nhiều xe vào một box.
- **Vật thể bị che khuất:** vẫn gán nếu phần nhìn thấy đủ để nhận diện lớp. Box ôm phần nhìn thấy, không đoán phần bị che hoàn toàn. Nếu bị cột/cây/biển báo che ở giữa nhưng hai đầu xe vẫn thấy thì box trải hết chiều dài xe. Hai xe chồng nhau thì vẽ hai box riêng; box của xe phía sau được phép chồng lên box xe phía trước.
- **Vật thể bị cắt bởi rìa ảnh:** vẫn gán nếu phần trong khung đủ nhận diện lớp. Cạnh box chạm đúng rìa ảnh, không kéo ra ngoài và không đoán phần nằm ngoài khung.
- **Vật thể rất nhỏ/mơ hồ:** nếu zoom vẫn nhận ra chắc chắn là xe và xác định được lớp thì gán bình thường. Nếu chỉ còn mảnh rất nhỏ, quá mờ hoặc quá xa nên không phân biệt được lớp thì không gán khung.
- **Ngoài phạm vi:** không gán người đi bộ, xe đạp, mô tô/xe máy, biển báo, đèn giao thông. Xe đang đỗ hoặc đứng yên vẫn gán theo loại xe.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_004` | xe bị che bới rìa ảnh, góc chụp từ trên cao và đang di chuyển nên hơi mờ, không phân biệt rõ car hay van | Gán nhãn `car` | phán đoán dáng xe có vẻ nhỏ hơn 1 con xe van, khá giống xe bán tải hơn |
| `drive_014` | xe `truck` chở đồ nhiều trên thùng, cao hơn đầu xe, không biết nên vẽ khung đến mức nào | chỉ vẽ khung cao bằng đầu xe | tập trung khung vào xe `truck` hơn |
| `drive_020` | Xe trắng nhỏ, đầu xe khá giống với xe `van`, nhưng kích cỡ lại khá nhỏ, khó phân biệt với `car` | gán nhãn `car` | chọn `car` vì kích thước nhỏ hơn van bình thường, có thể chỉ là xe cá nhân |
