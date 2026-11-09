# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |Xe con,taxi,xe bán tải|Xe bán tải xếp vào car |
| `truck` |Xe tải chở hàng, xe tải lớn, xe đầu kéo |Phân biệt với xe bán tải thuộc car |
| `bus` |Xe buýt và minibus chở khách |Phân biệt với van/minivan |
| `van` |Xe van, minivan chở hàng hoặc chở người |Không xếp nhầm thành bus |

## Quy tắc chung

- Bounding box: Mỗi phương tiện được gán một bounding box riêng, bao sát phần thân xe nhìn thấy được.
- Vật thể bị che khuất: Vẫn gán nhãn nếu phần nhìn thấy đủ để nhận diện loại phương tiện.
- Vật thể bị cắt bởi rìa ảnh: Box kết thúc tại rìa ảnh, không ước lượng phần nằm ngoài khung hình.
- Vật thể rất nhỏ/mơ hồ: Không tự suy đoán class nếu không đủ thông tin để nhận diện.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_030` |Xe màu đen bị che bên cạnh xe bus xanh |Không gán nhãn |Không đủ thông tin nhận diện |
| `drive_030` |Xe màu vàng ở góc của ảnh |Không gán nhãn |Không đủ thông tin nhận diện |
| `drive_028` |Xe màu trắng gần nhà hàng giống xe van và xe truck |Gán nhãn truck |Giống xe tải chở hàng hơn |
