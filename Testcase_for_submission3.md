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

---

## Test Cases cho Hệ thống Tutor Support System

### **Module 1: Authentication & Authorization**

#### **TC_AUTH_01: Đăng nhập thành công qua HCMUT_SSO**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_AUTH_01 |
| **Test Case Description** | Kiểm tra đăng nhập thành công với email hợp lệ qua HCMUT_SSO |
| **Pre-conditions** | 1. Hệ thống đang chạy trên http://localhost:3000<br>2. External APIs (SSO, DATACORE) hoạt động bình thường<br>3. Database đã được migrate |
| **Steps** | 1. Mở trình duyệt và truy cập http://localhost:3000/api-docs<br>2. Tìm endpoint POST /auth/login<br>3. Click "Try it out"<br>4. Nhập body: `{"email": "2112345@hcmut.edu.vn"}`<br>5. Click "Execute"<br>6. Quan sát response |
| **Expected Result** | - Status code: 200 OK<br>- Response body chứa:<br>  + `access_token`: JWT token hợp lệ<br>  + `user.email`: "2112345@hcmut.edu.vn"<br>  + `user.fullName`: Tên đầy đủ<br>  + `user.role`: "STUDENT"<br>  + `ssoInfo.authenticatedVia`: "HCMUT_SSO"<br>  + `ssoInfo.dataSyncedFrom`: "HCMUT_DATACORE"<br>- Token có thể decode được với payload chứa sub, email, role |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Mock SSO implementation - Production cần test với real SSO |

---

#### **TC_AUTH_02: Đăng nhập với email không hợp lệ**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_AUTH_02 |
| **Test Case Description** | Kiểm tra hệ thống từ chối đăng nhập với email không phải @hcmut.edu.vn |
| **Pre-conditions** | 1. Hệ thống đang chạy<br>2. External APIs hoạt động |
| **Steps** | 1. Mở Swagger UI tại /api-docs<br>2. Chọn POST /auth/login<br>3. Nhập body: `{"email": "invalid@gmail.com"}`<br>4. Click "Execute" |
| **Expected Result** | - Status code: 401 Unauthorized<br>- Response body chứa error message: "Invalid email domain. Must be @hcmut.edu.vn"<br>- Không có access_token được trả về |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Test validation logic trước khi gọi SSO |

---

#### **TC_AUTH_03: Truy cập protected endpoint với JWT hợp lệ**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_AUTH_03 |
| **Test Case Description** | Kiểm tra truy cập endpoint yêu cầu authentication với JWT token hợp lệ |
| **Pre-conditions** | 1. Đã đăng nhập thành công (TC_AUTH_01)<br>2. Đã có JWT access_token |
| **Steps** | 1. Copy access_token từ response của /auth/login<br>2. Trong Swagger UI, click nút "Authorize" ở góc phải<br>3. Nhập: `Bearer <access_token>`<br>4. Click "Authorize"<br>5. Chọn GET /auth/me<br>6. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body chứa thông tin user hiện tại:<br>  + id, email, fullName, mssv, role, createdAt |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | JwtAuthGuard working correctly |

---

#### **TC_AUTH_04: Truy cập protected endpoint không có token**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_AUTH_04 |
| **Test Case Description** | Kiểm tra hệ thống từ chối khi không có JWT token |
| **Pre-conditions** | Hệ thống đang chạy |
| **Steps** | 1. Mở Swagger UI<br>2. Nếu đã authorize, click "Logout" để xóa token<br>3. Chọn GET /auth/me<br>4. Click "Execute" |
| **Expected Result** | - Status code: 401 Unauthorized<br>- Response body: `{"statusCode": 401, "message": "Unauthorized"}` |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Test authentication guard |

---

#### **TC_AUTH_05: Truy cập admin endpoint với role STUDENT**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_AUTH_05 |
| **Test Case Description** | Kiểm tra RBAC - STUDENT không thể truy cập admin endpoint |
| **Pre-conditions** | 1. Đã đăng nhập với role STUDENT<br>2. Đã authorize với student token |
| **Steps** | 1. Đăng nhập với email student: `2112345@hcmut.edu.vn`<br>2. Authorize với token nhận được<br>3. Chọn GET /management/users (Admin only)<br>4. Click "Execute" |
| **Expected Result** | - Status code: 403 Forbidden<br>- Response body: `{"statusCode": 403, "message": "Forbidden resource"}` |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | RolesGuard protecting admin endpoints |

---

### **Module 2: External APIs Integration**

#### **TC_EXT_01: Health check tất cả external services**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_EXT_01 |
| **Test Case Description** | Kiểm tra health status của cả 3 external services (SSO, DATACORE, LIBRARY) |
| **Pre-conditions** | 1. Đã đăng nhập với role ADMIN<br>2. External services đang chạy |
| **Steps** | 1. Đăng nhập với email admin: `ADMIN001@hcmut.edu.vn`<br>2. Authorize với admin token<br>3. Chọn GET /external/health-all<br>4. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body chứa:<br>```json<br>{<br>  "services": {<br>    "HCMUT_SSO": {<br>      "status": "healthy",<br>      "message": "HCMUT_SSO service is available (MOCK)"<br>    },<br>    "HCMUT_DATACORE": {<br>      "status": "healthy",<br>      "message": "HCMUT_DATACORE service is available (MOCK)"<br>    },<br>    "HCMUT_LIBRARY": {<br>      "status": "healthy",<br>      "message": "HCMUT_LIBRARY service is available (MOCK)"<br>    }<br>  },<br>  "timestamp": "<current_timestamp>"<br>}<br>``` |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Mock implementation - all services return healthy |

---

#### **TC_EXT_02: Sync user data từ DATACORE**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_EXT_02 |
| **Test Case Description** | Kiểm tra đồng bộ dữ liệu user từ HCMUT_DATACORE |
| **Pre-conditions** | 1. Đã đăng nhập với role ADMIN hoặc COORDINATOR<br>2. DATACORE service hoạt động |
| **Steps** | 1. Authorize với admin/coordinator token<br>2. Chọn POST /external/datacore/sync-user<br>3. Click "Try it out"<br>4. Nhập body: `{"userId": "2112345"}`<br>5. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body chứa:<br>  + userId: "2112345"<br>  + email: "2112345@hcmut.edu.vn"<br>  + fullName: Tên sinh viên<br>  + department: "Computer Science & Engineering"<br>  + role: "STUDENT"<br>  + status: "active"<br>  + phoneNumber, studentClass (if applicable) |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Mock data với deterministic name generation |

---

#### **TC_EXT_03: Tìm kiếm tài liệu trong LIBRARY**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_EXT_03 |
| **Test Case Description** | Kiểm tra tìm kiếm tài liệu từ HCMUT_LIBRARY |
| **Pre-conditions** | 1. Đã đăng nhập (bất kỳ role nào)<br>2. LIBRARY service hoạt động |
| **Steps** | 1. Authorize với bất kỳ token hợp lệ<br>2. Chọn GET /external/library/search<br>3. Nhập query parameters:<br>   - query: "Cấu trúc dữ liệu"<br>   - page: 1<br>   - limit: 5<br>4. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body chứa:<br>  + documents: Array chứa ít nhất 1 document<br>  + documents[0].title: "Cấu trúc dữ liệu và Giải thuật - Tập 1"<br>  + documents[0].author: "TS. Nguyễn Văn A"<br>  + documents[0].category: "course_material"<br>  + total: Tổng số documents tìm được<br>  + page: 1<br>  + limit: 5 |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Mock dataset với 5 documents mẫu |

---

#### **TC_EXT_04: Lấy URL download tài liệu**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_EXT_04 |
| **Test Case Description** | Kiểm tra lấy signed URL để download tài liệu |
| **Pre-conditions** | 1. Đã đăng nhập<br>2. Biết documentId hợp lệ (từ TC_EXT_03) |
| **Steps** | 1. Authorize với token<br>2. Chọn POST /external/library/document-url<br>3. Nhập body:<br>```json<br>{<br>  "documentId": "DOC001",<br>  "userId": "2112345"<br>}<br>```<br>4. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body chứa:<br>  + documentId: "DOC001"<br>  + downloadUrl: URL có format "https://library.hcmut.edu.vn/download?token=..."<br>  + expiresAt: Timestamp 60 phút từ hiện tại<br>  + fileSize: Size of file |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Mock URL với base64 token |

---

#### **TC_EXT_05: Bulk sync nhiều users từ DATACORE**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_EXT_05 |
| **Test Case Description** | Kiểm tra đồng bộ hàng loạt users |
| **Pre-conditions** | 1. Đã đăng nhập với role ADMIN<br>2. DATACORE service hoạt động |
| **Steps** | 1. Authorize với admin token<br>2. Chọn POST /external/datacore/bulk-sync<br>3. Nhập body:<br>```json<br>{<br>  "userIds": ["2112345", "2112346", "GV001"]<br>}<br>```<br>4. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body: Array chứa 3 objects<br>- Mỗi object chứa đầy đủ thông tin user: userId, email, fullName, department, role, status |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Useful cho migration data |

---

### **Module 3: Users & Profile Management**

#### **TC_USER_01: Xem profile cá nhân**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_USER_01 |
| **Test Case Description** | Kiểm tra xem thông tin profile cá nhân |
| **Pre-conditions** | 1. Đã đăng nhập thành công<br>2. User đã tồn tại trong database |
| **Steps** | 1. Đăng nhập với bất kỳ email hợp lệ<br>2. Authorize với token<br>3. Chọn GET /users/me<br>4. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body chứa:<br>  + id: User ID<br>  + email: Email đã dùng login<br>  + fullName: Tên đầy đủ<br>  + mssv: MSSV (nullable)<br>  + role: Role của user<br>  + tutorProfile: null hoặc object (nếu là TUTOR)<br>  + createdAt: Timestamp |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Trả về cả tutorProfile nếu user là TUTOR |

---

#### **TC_USER_02: Cập nhật profile thành công**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_USER_02 |
| **Test Case Description** | Kiểm tra cập nhật thông tin profile |
| **Pre-conditions** | 1. Đã đăng nhập<br>2. User có quyền edit profile |
| **Steps** | 1. Authorize với token<br>2. Chọn PUT /users/me<br>3. Nhập body:<br>```json<br>{<br>  "fullName": "Nguyễn Văn An Updated",<br>  "bio": "Chuyên môn: Lập trình hướng đối tượng"<br>}<br>```<br>4. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body chứa user với thông tin đã cập nhật:<br>  + fullName: "Nguyễn Văn An Updated"<br>  + bio được update (nếu là tutor) |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Chỉ update được các field cho phép (fullName, bio, expertise) |

---

#### **TC_USER_03: Cập nhật với dữ liệu không hợp lệ**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_USER_03 |
| **Test Case Description** | Kiểm tra validation khi cập nhật với dữ liệu sai format |
| **Pre-conditions** | Đã đăng nhập |
| **Steps** | 1. Authorize với token<br>2. Chọn PUT /users/me<br>3. Nhập body:<br>```json<br>{<br>  "fullName": "",<br>  "expertise": "not an array"<br>}<br>```<br>4. Click "Execute" |
| **Expected Result** | - Status code: 400 Bad Request<br>- Response body chứa validation errors:<br>  + Lỗi về fullName không được rỗng<br>  + Lỗi về expertise phải là array |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | DTOs validation với class-validator |

---

### **Module 4: Tutors & Availability**

#### **TC_TUT_01: Xem danh sách tất cả tutors**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_TUT_01 |
| **Test Case Description** | Kiểm tra lấy danh sách tất cả tutors trong hệ thống |
| **Pre-conditions** | 1. Đã đăng nhập<br>2. Có ít nhất 1 tutor trong hệ thống |
| **Steps** | 1. Authorize với token<br>2. Chọn GET /tutors<br>3. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body: Array of tutor profiles<br>- Mỗi tutor chứa:<br>  + id, userId<br>  + available: boolean<br>  + bio: string hoặc null<br>  + expertise: string[]<br>  + user: UserResponseDto với thông tin cơ bản |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Endpoint public cho STUDENT tìm tutors |

---

#### **TC_TUT_02: Tutor thêm availability slot**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_TUT_02 |
| **Test Case Description** | Kiểm tra tutor tạo khung giờ rảnh |
| **Pre-conditions** | 1. Đã đăng nhập với role TUTOR<br>2. User có TutorProfile |
| **Steps** | 1. Đăng nhập với email tutor: `GV001@hcmut.edu.vn`<br>2. Authorize với tutor token<br>3. Chọn POST /tutors/availability<br>4. Nhập body:<br>```json<br>{<br>  "startTime": "2025-11-20T09:00:00Z",<br>  "endTime": "2025-11-20T11:00:00Z"<br>}<br>```<br>5. Click "Execute" |
| **Expected Result** | - Status code: 201 Created<br>- Response body chứa:<br>  + id: Slot ID<br>  + tutorId: ID của tutor<br>  + startTime: "2025-11-20T09:00:00Z"<br>  + endTime: "2025-11-20T11:00:00Z"<br>  + isBooked: false |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Tutor tự quản lý lịch rảnh |

---

#### **TC_TUT_03: Tutor xem schedule của mình**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_TUT_03 |
| **Test Case Description** | Kiểm tra tutor xem tất cả availability slots của mình |
| **Pre-conditions** | 1. Đã đăng nhập với role TUTOR<br>2. Đã có ít nhất 1 availability slot (TC_TUT_02) |
| **Steps** | 1. Authorize với tutor token<br>2. Chọn GET /tutors/schedule<br>3. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body: Array of AvailabilitySlotDto<br>- Chứa tất cả slots của tutor hiện tại<br>- Bao gồm cả slots đã booked và chưa booked |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Tutor quản lý lịch của mình |

---

#### **TC_TUT_04: Student không thể thêm availability slot**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_TUT_04 |
| **Test Case Description** | Kiểm tra RBAC - STUDENT không thể tạo availability slot |
| **Pre-conditions** | Đã đăng nhập với role STUDENT |
| **Steps** | 1. Đăng nhập với student email<br>2. Authorize với student token<br>3. Chọn POST /tutors/availability<br>4. Nhập body hợp lệ<br>5. Click "Execute" |
| **Expected Result** | - Status code: 403 Forbidden<br>- Response body: `{"statusCode": 403, "message": "Forbidden resource"}` |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | RolesGuard protecting tutor-only endpoints |

---

#### **TC_TUT_05: Xóa availability slot chưa được book**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_TUT_05 |
| **Test Case Description** | Kiểm tra tutor xóa slot chưa có booking |
| **Pre-conditions** | 1. Đã đăng nhập với role TUTOR<br>2. Có slot chưa booked (isBooked: false) |
| **Steps** | 1. Authorize với tutor token<br>2. Lấy slotId từ GET /tutors/schedule<br>3. Chọn DELETE /tutors/availability/{id}<br>4. Nhập slotId vào path parameter<br>5. Click "Execute" |
| **Expected Result** | - Status code: 200 OK hoặc 204 No Content<br>- Slot bị xóa khỏi database<br>- Gọi lại GET /tutors/schedule sẽ không thấy slot này |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Tutor có thể xóa slots chưa booked |

---

### **Module 5: Meetings & Bookings**

#### **TC_MTG_01: Student đặt lịch meeting thành công**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_MTG_01 |
| **Test Case Description** | Kiểm tra student đặt lịch học với tutor |
| **Pre-conditions** | 1. Đã đăng nhập với role STUDENT<br>2. Có availability slot chưa booked (từ TC_TUT_02) |
| **Steps** | 1. Đăng nhập với student email<br>2. Authorize với student token<br>3. Lấy tutorId và slotId từ GET /tutors<br>4. Chọn POST /meetings<br>5. Nhập body:<br>```json<br>{<br>  "tutorId": 1,<br>  "slotId": 1,<br>  "topic": "Học OOP - Inheritance"<br>}<br>```<br>6. Click "Execute" |
| **Expected Result** | - Status code: 201 Created<br>- Response body chứa:<br>  + id: Meeting ID<br>  + studentId: ID của student hiện tại<br>  + tutorId: 1<br>  + slotId: 1<br>  + status: "PENDING"<br>  + topic: "Học OOP - Inheritance"<br>  + startTime, endTime từ slot<br>- Notification được gửi đến tutor |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Meeting status = PENDING, chờ tutor confirm |

---

#### **TC_MTG_02: Tutor confirm meeting**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_MTG_02 |
| **Test Case Description** | Kiểm tra tutor xác nhận meeting request |
| **Pre-conditions** | 1. Đã đăng nhập với role TUTOR<br>2. Có meeting với status PENDING (từ TC_MTG_01) |
| **Steps** | 1. Authorize với tutor token<br>2. Lấy meetingId từ GET /meetings/my-meetings<br>3. Chọn PUT /meetings/{id}/confirm<br>4. Nhập meetingId vào path parameter<br>5. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body chứa meeting với:<br>  + status: "CONFIRMED"<br>- Slot.isBooked = true<br>- Notification được gửi đến student |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Meeting status chuyển sang CONFIRMED |

---

#### **TC_MTG_03: Student hủy meeting**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_MTG_03 |
| **Test Case Description** | Kiểm tra student hủy meeting |
| **Pre-conditions** | 1. Đã đăng nhập với role STUDENT<br>2. Có meeting với status PENDING hoặc CONFIRMED |
| **Steps** | 1. Authorize với student token<br>2. Lấy meetingId từ GET /meetings/my-meetings<br>3. Chọn PUT /meetings/{id}/cancel<br>4. Nhập meetingId<br>5. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body chứa meeting với:<br>  + status: "CANCELED"<br>- Slot.isBooked = false (slot được giải phóng)<br>- Notification được gửi đến tutor |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Student có thể hủy meeting |

---

#### **TC_MTG_04: Tutor complete meeting**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_MTG_04 |
| **Test Case Description** | Kiểm tra tutor đánh dấu meeting hoàn thành |
| **Pre-conditions** | 1. Đã đăng nhập với role TUTOR<br>2. Có meeting với status CONFIRMED |
| **Steps** | 1. Authorize với tutor token<br>2. Lấy meetingId<br>3. Chọn PUT /meetings/{id}/complete<br>4. Nhập meetingId<br>5. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body chứa meeting với:<br>  + status: "COMPLETED"<br>- Notification được gửi đến student để đánh giá |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Sau khi complete, student có thể rating |

---

#### **TC_MTG_05: Student rating meeting**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_MTG_05 |
| **Test Case Description** | Kiểm tra student đánh giá buổi học |
| **Pre-conditions** | 1. Đã đăng nhập với role STUDENT<br>2. Có meeting với status COMPLETED (từ TC_MTG_04) |
| **Steps** | 1. Authorize với student token<br>2. Lấy meetingId của meeting COMPLETED<br>3. Chọn POST /meetings/{id}/rating<br>4. Nhập body:<br>```json<br>{<br>  "score": 5,<br>  "comment": "Tutor giảng rất hay, dễ hiểu!"<br>}<br>```<br>5. Click "Execute" |
| **Expected Result** | - Status code: 201 Created<br>- Response body chứa:<br>  + id: Rating ID<br>  + studentId: ID của student<br>  + meetingId: ID của meeting<br>  + score: 5<br>  + comment: "Tutor giảng rất hay, dễ hiểu!"<br>  + createdAt: Timestamp |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Mỗi meeting chỉ có 1 rating (1:1 relationship) |

---

#### **TC_MTG_06: Xem danh sách meetings của user**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_MTG_06 |
| **Test Case Description** | Kiểm tra xem tất cả meetings của user hiện tại |
| **Pre-conditions** | 1. Đã đăng nhập<br>2. User có ít nhất 1 meeting |
| **Steps** | 1. Authorize với token<br>2. Chọn GET /meetings/my-meetings<br>3. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body: Array of MeetingDto<br>- Nếu là STUDENT: chứa meetings mà studentId = user.id<br>- Nếu là TUTOR: chứa meetings mà tutorId = tutorProfile.id<br>- Mỗi meeting có đầy đủ thông tin: status, topic, student, tutor, slot, rating (if any) |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Endpoint này filter theo role tự động |

---

#### **TC_MTG_07: Student complaint về meeting**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_MTG_07 |
| **Test Case Description** | Kiểm tra student gửi khiếu nại về buổi học |
| **Pre-conditions** | 1. Đã đăng nhập với role STUDENT<br>2. Có meeting |
| **Steps** | 1. Authorize với student token<br>2. Chọn POST /meetings/{id}/complaint<br>3. Nhập meetingId và body:<br>```json<br>{<br>  "description": "Tutor đến trễ 30 phút và không giảng đủ nội dung"<br>}<br>```<br>4. Click "Execute" |
| **Expected Result** | - Status code: 201 Created<br>- Response body chứa:<br>  + id: Complaint ID<br>  + studentId: ID của student<br>  + meetingId: ID của meeting<br>  + description: Nội dung khiếu nại<br>  + status: "OPEN"<br>  + createdAt: Timestamp<br>- Notification được gửi đến Coordinator |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Coordinator sẽ xử lý complaint này |

---

### **Module 6: Progress Tracking**

#### **TC_PRG_01: Tutor tạo progress record cho student**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_PRG_01 |
| **Test Case Description** | Kiểm tra tutor ghi nhận tiến độ học của student |
| **Pre-conditions** | 1. Đã đăng nhập với role TUTOR<br>2. Có student đã học với tutor |
| **Steps** | 1. Authorize với tutor token<br>2. Chọn POST /my-schedule/progress<br>3. Nhập body:<br>```json<br>{<br>  "studentId": 1,<br>  "note": "Student đã nắm vững OOP Inheritance, cần ôn lại Polymorphism"<br>}<br>```<br>4. Click "Execute" |
| **Expected Result** | - Status code: 201 Created<br>- Response body chứa:<br>  + id: Progress ID<br>  + studentId: 1<br>  + tutorId: ID của tutor hiện tại<br>  + note: "Student đã nắm vững..."<br>  + createdAt: Timestamp |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Tutor theo dõi tiến độ từng student |

---

#### **TC_PRG_02: Xem progress của 1 student**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_PRG_02 |
| **Test Case Description** | Kiểm tra xem tất cả progress records của 1 student |
| **Pre-conditions** | 1. Đã đăng nhập với role TUTOR/TBM/COORDINATOR<br>2. Có progress records (từ TC_PRG_01) |
| **Steps** | 1. Authorize với token có quyền<br>2. Chọn GET /my-schedule/progress/{studentId}<br>3. Nhập studentId = 1<br>4. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body: Array of ProgressRecordDto<br>- Chứa tất cả progress records của student với ID = 1<br>- Mỗi record có: id, studentId, tutorId, note, createdAt, student info, tutor info |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Useful cho TBM theo dõi student trong bộ môn |

---

#### **TC_PRG_03: Student xem progress của chính mình**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_PRG_03 |
| **Test Case Description** | Kiểm tra student xem progress records về mình |
| **Pre-conditions** | 1. Đã đăng nhập với role STUDENT<br>2. Có progress records về student này |
| **Steps** | 1. Authorize với student token<br>2. Chọn GET /my-schedule/my-progress<br>3. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body: Array of ProgressRecordDto<br>- Chỉ chứa progress records mà studentId = current user ID<br>- Student thấy được các ghi nhận từ tutors |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Student biết được tiến độ học của mình |

---

### **Module 7: Management & Admin**

#### **TC_MGT_01: Admin xem tất cả users**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_MGT_01 |
| **Test Case Description** | Kiểm tra admin lấy danh sách tất cả users |
| **Pre-conditions** | 1. Đã đăng nhập với role ADMIN<br>2. Database có users |
| **Steps** | 1. Đăng nhập với admin email<br>2. Authorize với admin token<br>3. Chọn GET /management/users<br>4. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body: Array of UserResponseDto<br>- Chứa tất cả users trong hệ thống<br>- Mỗi user: id, email, fullName, mssv, role, createdAt |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Admin có full access user list |

---

#### **TC_MGT_02: Admin tạo user mới**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_MGT_02 |
| **Test Case Description** | Kiểm tra admin tạo user thủ công |
| **Pre-conditions** | Đã đăng nhập với role ADMIN |
| **Steps** | 1. Authorize với admin token<br>2. Chọn POST /management/users<br>3. Nhập body:<br>```json<br>{<br>  "email": "newuser@hcmut.edu.vn",<br>  "fullName": "Nguyễn Văn Test",<br>  "mssv": "2199999",<br>  "role": "STUDENT"<br>}<br>```<br>4. Click "Execute" |
| **Expected Result** | - Status code: 201 Created<br>- Response body chứa user mới với thông tin đã nhập<br>- User được lưu vào database<br>- User có thể login ngay |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Admin có thể tạo user manually |

---

#### **TC_MGT_03: Coordinator xem tất cả complaints**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_MGT_03 |
| **Test Case Description** | Kiểm tra coordinator xem danh sách complaints |
| **Pre-conditions** | 1. Đã đăng nhập với role COORDINATOR<br>2. Có complaints trong hệ thống (từ TC_MTG_07) |
| **Steps** | 1. Đăng nhập với coordinator email<br>2. Authorize với coordinator token<br>3. Chọn GET /management/complaints<br>4. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body: Array of ComplaintDto<br>- Chứa tất cả complaints với các status: OPEN, IN_PROGRESS, RESOLVED<br>- Mỗi complaint: id, studentId, meetingId, description, status, createdAt |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Coordinator quản lý complaints |

---

#### **TC_MGT_04: Coordinator resolve complaint**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_MGT_04 |
| **Test Case Description** | Kiểm tra coordinator xử lý khiếu nại |
| **Pre-conditions** | 1. Đã đăng nhập với role COORDINATOR<br>2. Có complaint với status OPEN |
| **Steps** | 1. Authorize với coordinator token<br>2. Lấy complaintId từ GET /management/complaints<br>3. Chọn PUT /management/complaints/{id}/resolve<br>4. Nhập body:<br>```json<br>{<br>  "resolution": "Đã nhắc nhở tutor về việc đúng giờ. Hoàn tiền 50% học phí cho student."<br>}<br>```<br>5. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body chứa complaint với:<br>  + status: "RESOLVED"<br>  + coordinatorId: ID của coordinator hiện tại<br>- Notification được gửi đến student và tutor |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Complaint được đóng sau khi resolve |

---

#### **TC_MGT_05: TBM tạo learning roadmap**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_MGT_05 |
| **Test Case Description** | Kiểm tra TBM tạo lộ trình học chuẩn |
| **Pre-conditions** | Đã đăng nhập với role TBM |
| **Steps** | 1. Đăng nhập với TBM email<br>2. Authorize với TBM token<br>3. Chọn POST /management/roadmaps<br>4. Nhập body:<br>```json<br>{<br>  "title": "Lộ trình học OOP - Semester 1",<br>  "description": "Tuần 1-4: Basic OOP, Tuần 5-8: Inheritance & Polymorphism, Tuần 9-12: Design Patterns",<br>  "documentUrl": "https://library.hcmut.edu.vn/files/oop-roadmap.pdf"<br>}<br>```<br>5. Click "Execute" |
| **Expected Result** | - Status code: 201 Created<br>- Response body chứa:<br>  + id: Roadmap ID<br>  + authorId: ID của TBM<br>  + title, description, documentUrl<br>- Roadmap được lưu và visible cho students |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | TBM quản lý curriculum |

---

### **Module 8: Notifications**

#### **TC_NTF_01: Xem notifications của user**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_NTF_01 |
| **Test Case Description** | Kiểm tra xem tất cả notifications của user hiện tại |
| **Pre-conditions** | 1. Đã đăng nhập<br>2. User có notifications (từ các actions trước) |
| **Steps** | 1. Authorize với token<br>2. Chọn GET /notifications<br>3. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body: Array of NotificationDto<br>- Chứa tất cả notifications của user<br>- Mỗi notification: id, recipientId, title, message, isRead, createdAt |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Notifications được tạo tự động từ system events |

---

#### **TC_NTF_02: Mark notification as read**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_NTF_02 |
| **Test Case Description** | Kiểm tra đánh dấu notification đã đọc |
| **Pre-conditions** | 1. Đã đăng nhập<br>2. Có notification với isRead = false |
| **Steps** | 1. Authorize với token<br>2. Lấy notificationId từ GET /notifications<br>3. Chọn PUT /notifications/{id}/read<br>4. Nhập notificationId<br>5. Click "Execute" |
| **Expected Result** | - Status code: 200 OK<br>- Response body chứa notification với:<br>  + isRead: true<br>- Notification được update trong database |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | User quản lý notification status |

---

#### **TC_NTF_03: Admin gửi notification thủ công**

| Field | Description |
|-------|-------------|
| **Test Case ID** | TC_NTF_03 |
| **Test Case Description** | Kiểm tra admin/coordinator gửi notification thủ công |
| **Pre-conditions** | Đã đăng nhập với role ADMIN hoặc COORDINATOR |
| **Steps** | 1. Authorize với admin/coordinator token<br>2. Chọn POST /notifications/send<br>3. Nhập body:<br>```json<br>{<br>  "recipientId": 1,<br>  "title": "Thông báo quan trọng",<br>  "message": "Hệ thống sẽ bảo trì vào 2h sáng ngày 15/11"<br>}<br>```<br>4. Click "Execute" |
| **Expected Result** | - Status code: 201 Created<br>- Response body chứa notification đã tạo<br>- Recipient (userId=1) sẽ thấy notification này<br>- Email được gửi (nếu configured) |
| **Actual Result** | *(To be filled during testing)* |
| **Status** | *(Pass/Fail)* |
| **Comments** | Admin có thể broadcast notifications |

---

## Tổng kết Test Cases

### Statistics:

| Module | Test Cases | Coverage |
|--------|-----------|----------|
| **Authentication & Authorization** | 5 | JWT, SSO, RBAC |
| **External APIs** | 5 | SSO, DATACORE, LIBRARY |
| **Users & Profile** | 3 | Profile management |
| **Tutors & Availability** | 5 | Scheduling |
| **Meetings & Bookings** | 7 | Full booking flow |
| **Progress Tracking** | 3 | Student progress |
| **Management & Admin** | 5 | Admin functions |
| **Notifications** | 3 | Notification system |
| **TOTAL** | **36** | **Full system coverage** |

### Test Execution Guidelines:

1. **Environment Setup:**
   ```bash
   # Start server
   npm run start:dev
   
   # Access Swagger UI
   http://localhost:3000/api-docs
   ```

2. **Test Order:**
   - Run authentication tests first (TC_AUTH_01-05)
   - Then run module-specific tests
   - Follow dependencies (e.g., create meeting before rating)

3. **Data Preparation:**
   - Use mock emails: `2112345@hcmut.edu.vn` (STUDENT), `GV001@hcmut.edu.vn` (TUTOR), `ADMIN001@hcmut.edu.vn` (ADMIN)
   - Save tokens for reuse across tests
   - Keep track of created IDs (slotId, meetingId, etc.)

---

