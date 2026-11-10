# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe ô tô chở người thông thường, bao gồm sedan, SUV, taxi và xe bán tải. | Không gán xe buýt, xe tải và mô tô. |
| `truck` | Xe tải chở hàng, xe đầu kéo và các phương tiện vận tải hàng hóa cỡ lớn. | Không gán xe bán tải hoặc cabin nhỏ thuộc car. |
| `bus` | Xe buýt, xe buýt nhỏ (minibus) được thiết kế để chở hành khách. | Phân biệt với van chở người và xe van chở hàng. |
| `van` | Xe van, minivan chở hàng hoặc chở người. | Không gán xe buýt lớn hoặc xe con thông thường. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Bounding Box phải bao quanh toàn bộ phần vật thể nhìn thấy, không lấy quá nhiều nền xung quanh và không cắt mất phần vật thể nhìn thấy.
- Vật thể bị che khuất: Nếu vẫn nhận diện được phương tiện, tiến hành gán nhãn theo phần vật thể quan sát được. Không tự suy đoán phần bị che khuất.
- Vật thể bị cắt bởi rìa ảnh: Nếu phần phương tiện còn nhìn thấy đủ để xác định lớp, vẫn gán nhãn và box theo phần xuất hiện trong ảnh.
- Vật thể rất nhỏ/mơ hồ: Chỉ gán nhãn khi có đủ căn cứ xác định phương tiện và lớp. Nếu không thể xác định chắc chắn thì không gán, ghi nhận để kiểm tra lại.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_007.jpg` | Một số phương tiện ở phía xa, bị che khuất một phần bởi cây cối và các xe khác, khó xác định chính xác loại xe. | Chỉ gán car khi đủ đặc điểm nhận diện; trường hợp không xác định được thì không gán. | Schema yêu cầu xác định đúng lớp phương tiện. Không suy đoán loại xe khi hình ảnh không đủ căn cứ. |
| `drive_009.jpg` | Xe tải màu xanh có thùng chở hàng, hình dáng tương đối giống xe van cỡ lớn. | truck | Đây là phương tiện chở hàng có thùng hàng phía sau, phù hợp với định nghĩa xe tải chở hàng trong schema, không phải van.|
| `drive_029.jpg.` | Xe buýt lớn và ô tô con xuất hiện gần nhau, các Bounding Box có nguy cơ chồng lấn. | Xe buýt gán bus, ô tô con gán car. | Xe buýt lớn chở khách thuộc lớp bus; xe con thông thường thuộc lớp car. Mỗi Bounding Box phải bao quanh đúng một phương tiện.|
