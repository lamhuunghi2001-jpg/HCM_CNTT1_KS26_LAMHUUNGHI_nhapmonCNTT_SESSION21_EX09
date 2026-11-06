# HỌ VÀ TÊN: **LÂM HỮU NGHỊ**
# LỚP: **HCM_KS26_CNTT1**

## Phần 1: Thiết kế Prompt 3 thành phần
Câu 1: Vì sao câu hỏi trả lời cụt ngủn khiến Al trả lời sai
Câu *'AI ơi phần mềm trạm sạc của tôi bị hỏng rồi làm sao đây?'* không cho Al biết hệ điều hành, tên phần mềm, nội dung lỗi ra sao thao tác ra sao. Khi Al không có ngữ cảnh cụ thể nó sẽ cố trả lời bằng cách dự đoán những điều nghe có thể là hợp lý nhất, thay vì hỏi lại. Chữ "trạm sạc hỏng" thường đi kèm sự cố liên quan đến phần cứng, nên nó khuyến tháo vỏ kiểm tra dây điện. Câu hỏi trả lời trôi chảy, tự tin nhưng không dựa trên tình huống thật. Đó là ảo Al (Hallucination). Muốn tránh, phải đưa ngữ cảnh đầy đủ và dán nguyên văn dòng lỗi.
Câu 2: Prompt hoàn chỉnh

[Context]
Tôi là nhân viên trực ban vận hành Kiosk sạc xe điện VoltCharge tại Bến xe Miền Đông. Trụ sạc nhanh số 04 chạy máy tính Windows 11, đột ngột dừng cấp điện cho một xe taxi điện. Màn hình Terminal hiện log lỗi bên dưới khi dịch vụ điều khiển (ControllerDaemon) khởi động. Tôi là kỹ thuật viên mới, chưa rành tiếng Anh chuyên ngành.

[Raw Error]
2026-09-21 21:15:30 [FATAL] ControllerDaemon failed to start.
System.UnauthorizedAccessException: Access to the path 'C:\Program Files\VoltCharge\data\session.log' is denied.
  at System.IO.FileStream.ValidateFileHandle(SafeFileHandle handle)
  at VoltCharge.Core.SessionManager.Initialize()
InnerException: None. Process terminated with exit code 5 (0x00000005).
Suggestion: Run the application with elevated administrator privileges or verify NTFS file permissions.

[Instruction]
Hãy dịch và giải thích lỗi trên bằng tiếng Việt bình dân, cho biết nguyên nhân gốc rễ, sau đó đưa ra 3 bước khắc phục cụ thể trên Windows 11. Nếu thông tin chưa đủ để kết luận chắc chắn, hãy nói rõ thay vì đoán.

## Phần 2: Giải mã lỗi và kịch bản khắc phục

Câu 3: Giải thích bằng tiếng Việt đời thường
- Phần mềm trạm sạc cần ghi nhật ký vào file session.log trong thư mục C:\Program Files\VoltCharge\data. Program Files là khu vực Windows bảo vệ, giống một căn nhà có khóa: chỉ người có chìa (quyền Quản Trị Viên) khóa mới mở được ngôi nhà. Không ghi được file này thì bộ điều khiển không khởi động nổi và tự dừng với mã lỗi 5 (bị từ chối truy cập). Đây là lỗi phân quyền phần mềm, không phải hỏng dây điện hay phần cứng.

Câu 4: Ba bước xử lý trên Windows 11
1. Chạy bằng quyền quản trị viên. Nhấp chuổ phải vào biểu tượng VoltChargee, chọn Run as administrator, bấm Yes ở hộp thoại UAC. Nếu chạy được thì đã xác định được đúng nguyên nhân.
2. Cấp quyền ghi cho thư mục data. Mở File Explorer, vào C:\Program Files\VoltCharge, nhấp chuột phải thư mục data, chọn Properties, sang thẻ Security, bấm Edit. Chọn tài khoản đang chạy dịch vụ (Users hoặc tài khoản dịch vụ của VoltCharge), tick Write (và Modify nếu cần) ở cột Allow, rồi bấm Apply và OK.

3. Đặt chạy quyền cao lâu dài và kiểm tra lại. Nhấp chuột phải file .exe, chọn Properties, thẻ Compatibility, tick Run this program as an administrator, bấm Apply. Khởi động lại ứng dụng hoặc dịch vụ, mở log xác nhận hết lỗi và trụ sạc số 04 cấp điện bình thường.

Câu 5. Bẫy an ninh với API_KEY
Chuỗi API_KEY: volt_secret_token_889922 là thông tin bí mật. Trước khi dán vào ChatGPT hoặc Gemini, kỹ thuật viên phải xóa hoặc che giá trị thật và thay bằng chỗ giữ chỗ, ví dụ API_KEY: [REDACTED]. Cấu trúc dòng log vẫn còn nên AI vẫn phân tích được lỗi.
