# Use-case testing

**Use-case testing** là kỹ thuật kiểm thử **dựa trên các Use Case của hệ thống**
→ Mỗi **Use Case** (ví dụ: “Đăng nhập”, “Đặt lịch học”, “Gửi phản hồi”) được chuyển thành **một tập test case** mô tả rõ:

* Luồng chính (main flow)
* Luồng thay thế (alternate flow)
* Các đầu vào/đầu ra kỳ vọng (input/output)
* Điều kiện tiên quyết và hậu điều kiện (pre-/post-condition)

Mục tiêu: đảm bảo hệ thống thực hiện **đúng hành vi của người dùng trong từng tình huống nghiệp vụ**.

---

## Cách viết Test Report từ Use Case

![](https://browserstack.wpenginepowered.com/wp-content/uploads/2024/11/Test-Case-Example.png)


1. **Test Case ID** Mã định danh duy nhất của mỗi trường hợp kiểm thử, giúp phân biệt và truy vết dễ dàng.

2. **Test Case Description** Mô tả ngắn gọn mục tiêu hoặc chức năng cần được kiểm thử.

3. **Pre-conditions** Các điều kiện tiên quyết phải được đáp ứng trước khi bắt đầu thực hiện kiểm thử.

4. **Steps** Danh sách các bước cụ thể mà người kiểm thử cần thực hiện để kiểm tra chức năng.

5. **Expected Result** Kết quả dự kiến hệ thống phải hiển thị hoặc thực hiện nếu hoạt động đúng.

6. **Actual Result** Kết quả thực tế thu được khi thực hiện kiểm thử.

7. **Status** Trạng thái của test case: **Pass** nếu kết quả thực tế trùng khớp với kết quả mong đợi, **Fail** nếu không.

8. **Comments** *(tùy chọn)* Ghi chú bổ sung về vấn đề gặp phải, môi trường thử nghiệm, hoặc đề xuất cải tiến.
