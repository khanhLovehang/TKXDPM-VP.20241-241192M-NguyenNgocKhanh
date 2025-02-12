# Place Order Use Case Outline

## Mục tiêu:
Cho phép khách hàng đặt hàng cho các sản phẩm đã chọn trong giỏ hàng và hoàn tất giao dịch thanh toán.

## Actor chính:
- **Customer** (Khách hàng)

---

## Preconditions (Tiền điều kiện):
- Giỏ hàng không rỗng.
- (Nếu áp dụng) Khách hàng đã đăng nhập hoặc đang sử dụng phiên làm việc dưới dạng khách.

---

## Postconditions (Hậu điều kiện):
- Đơn hàng được lưu trữ thành công trong hệ thống.
- Hệ thống gửi email xác nhận đơn hàng tới khách hàng.
- Giỏ hàng được làm trống sau khi đặt hàng thành công.

---

## Basic Flow (Luồng cơ bản):

1. **Review Cart:**  
   - Khách hàng xem lại giỏ hàng của mình.

2. **Initiate Place Order:**  
   - Khách hàng chọn “Đặt hàng” trên giao diện giỏ hàng.

3. **Inventory Verification:**  
   - Hệ thống kiểm tra số lượng tồn kho của từng sản phẩm trong giỏ hàng.

4. **Enter Delivery Information:**  
   - Hệ thống yêu cầu khách hàng nhập thông tin giao hàng (tên, email, số điện thoại, địa chỉ, tỉnh/thành phố).

5. **Calculate Delivery Fee:**  
   - Hệ thống tính và hiển thị phí vận chuyển dựa trên trọng lượng sản phẩm và khu vực giao hàng.

6. **Display Invoice:**  
   - Hệ thống tạo và hiển thị hóa đơn tạm thời gồm:
     - Danh sách sản phẩm và số lượng.
     - Tổng giá (chưa có VAT và có VAT).
     - Phí vận chuyển.
     - Tổng số tiền cần thanh toán.

7. **Make Payment:**  
   - Khách hàng chọn phương thức thanh toán (trong phạm vi khoá học, chỉ hỗ trợ thanh toán bằng thẻ tín dụng).
   - Hệ thống tích hợp với VNPay để xử lý giao dịch thanh toán.

8. **Order Confirmation:**  
   - Nếu thanh toán thành công:
     - Hệ thống xác nhận đặt hàng.
     - Gửi email xác nhận kèm thông tin hóa đơn và giao dịch cho khách hàng.
     - Giỏ hàng được làm trống.

---

## Alternative Flows (Luồng thay thế):

### Alternative Flow 1: Inventory Insufficiency (Tồn kho không đủ)
- **Điểm xảy ra:** Bước 3 (Inventory Verification)  
- **Mô tả:**  
  - Nếu hệ thống phát hiện sản phẩm nào đó không đủ số lượng tồn kho, hiển thị thông báo cảnh báo cho khách hàng.
  - Hệ thống yêu cầu khách hàng cập nhật giỏ hàng (chọn bỏ hoặc giảm số lượng sản phẩm không đủ).
  - Sau khi khách hàng cập nhật, quá trình quay trở lại bước **Review Cart**.

---

### Alternative Flow 2: Invalid or Incomplete Delivery Information (Thông tin giao hàng không hợp lệ/thiếu)
- **Điểm xảy ra:** Bước 4 (Enter Delivery Information)  
- **Mô tả:**  
  - Nếu khách hàng nhập thông tin giao hàng thiếu hoặc không đúng định dạng, hệ thống sẽ:
    - Hiển thị thông báo lỗi cụ thể cho từng trường thông tin không hợp lệ.
    - Yêu cầu khách hàng chỉnh sửa và nhập lại thông tin.
  - Sau khi sửa, quá trình tiếp tục từ bước **Calculate Delivery Fee**.

---

### Alternative Flow 3: Payment Failure (Thanh toán thất bại)
- **Điểm xảy ra:** Bước 7 (Make Payment)  
- **Mô tả:**  
  - Nếu giao dịch thanh toán thất bại (ví dụ: thẻ không hợp lệ, hết hạn, hoặc lỗi kết nối với VNPay):
    - Hệ thống hiển thị thông báo lỗi và cung cấp tùy chọn thử lại thanh toán hoặc chọn phương thức thanh toán khác (nếu có).
  - Nếu khách hàng thử lại mà giao dịch vẫn không thành công, hệ thống sẽ hủy quá trình đặt hàng và giữ nguyên giỏ hàng để khách hàng có thể chỉnh sửa hoặc thử lại sau.

---

### Alternative Flow 4: Customer Cancels Order
- **Điểm xảy ra:** Trong bất kỳ bước nào trước khi xác nhận đơn hàng cuối cùng  
- **Mô tả:**  
  - Nếu khách hàng thay đổi quyết định và hủy quá trình đặt hàng, hệ thống sẽ:
    - Ngưng quá trình đặt hàng.
    - Giữ nguyên giỏ hàng như hiện trạng để khách hàng tiếp tục mua sắm hoặc chỉnh sửa.

---

## Notes:
- Luồng sự kiện này bao gồm các bước chính và các tình huống ngoại lệ nhằm đảm bảo tính liên tục và an toàn của quá trình đặt hàng.
- Các yêu cầu phi chức năng (như thời gian phản hồi, khả năng hoạt động 24/7, số lượng khách hàng đồng thời, …) được quản lý riêng và không nằm trong phạm vi outline use case này.
