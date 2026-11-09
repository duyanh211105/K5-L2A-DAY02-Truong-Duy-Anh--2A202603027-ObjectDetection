# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe chở người cỡ nhỏ, thân thấp, thường 4–7 chỗ | Sedan, hatchback, SUV, taxi, xe bán tải. Bán tải có thùng hở phía sau nhưng cabin nhỏ nên vẫn là `car` |
| `truck` | Xe chuyên chở hàng hóa, có cabin tách biệt với thùng hàng hoặc rơ-moóc | Xe tải thùng, xe ben, xe đầu kéo container. Phân biệt với bán tải: thùng hàng lớn, cao hơn cabin, xe dài và cao hẳn |
| `bus` | Xe chở khách nhiều chỗ, thân dài liền khối, nhiều cửa sổ chạy dọc thân | Xe buýt thành phố, xe khách, minibus chở khách. Dài hơn van rõ rệt, có nhiều hàng cửa sổ |
| `van` | Xe thân hộp liền khối, cao hơn xe con nhưng ngắn hơn bus, dùng chở hàng hoặc chở người | Minivan, xe van chở hàng (kiểu Ford Transit, Hyundai Solati cỡ nhỏ). Đầu xe ngắn, thân cao vuông; cao hơn `car`, ngắn hơn `bus` |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: bốn cạnh box chạm vào điểm ngoài cùng của phần thân xe nhìn thấy được (kể cả gương chiếu hậu nếu thấy rõ), không lấy thêm mặt đường, bóng xe, bầu trời hay xe bên cạnh. Mỗi xe một box riêng, không gộp hai xe vào một box.
- Vật thể bị che khuất: vẫn vẽ box quanh phần nhìn thấy được nếu đủ để nhận ra loại xe; không đoán và kéo box ra phần bị che. Hai xe chồng nhau thì vẽ hai box riêng.
- Vật thể bị cắt bởi rìa ảnh: vẽ box tới đúng rìa ảnh, không ước lượng phần nằm ngoài khung; vẫn gán nếu phần còn lại nhận ra được loại xe.
- Vật thể rất nhỏ/mơ hồ: chỉ gán khi xác định được chắc chắn là xe thuộc 4 lớp và nhận ra loại; nếu quá nhỏ hoặc không phân biệt được lớp thì không đoán, ghi lại tên ảnh vào bảng dưới và hỏi giảng viên.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_...` | | | |
| `drive_...` | | | |
| `drive_...` | | | |
