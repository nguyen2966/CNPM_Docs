# 📌 Quy trình làm một dự án Công Nghệ Phần Mềm

## 1. Phân Tích và Thiết Kế  
*(BTL CNPM – tương tự Đồ Án Chuyên Ngành kỳ 1 năm 4 hướng CNPM)*  

### 1.1 Project Details Specification  
- Mô tả bối cảnh dự án.  
- Xác định các bên liên quan (stakeholders) và vai trò của họ.  
- Đặt ra mục tiêu và phạm vi của dự án.  

![](https://uploads-ssl.webflow.com/62fcfcf2e1a4c21ed18b80e6/64915d704f1983a69591db54_whats_included_in_a_project_scope_statement_ukwu.png)

### 1.2 Functional Requirements  
- **Use-case diagram**: Mô tả toàn bộ chức năng hệ thống.  

![](https://www.dragon1.com/images/uml-use-case-diagram.jpg)

- **Use-case detail/scenario**: Mỗi thành viên phụ trách 1 use-case cụ thể.  

![](https://www.researchgate.net/publication/220868395/figure/tbl1/AS:650034450272339@1531991698911/An-example-of-use-case-descriptions.png)


- **Các yêu cầu chức năng khác**. 

### 1.3 Non-functional Requirements  
- Yêu cầu về hiệu năng, bảo mật, khả năng mở rộng, tính dễ sử dụng…  

![](https://www.altexsoft.com/static/blog-post/2023/12/576a423e-5681-4792-be66-b03f86539214.jpg)


### 1.4 Các mô hình UML  
- **Activity diagrams**: Mô tả luồng hoạt động.  

![](https://cdn-images.visual-paradigm.com/guide/uml/what-is-activity-diagram/04-activity-diagram-example-process-order.png)

- **Sequence diagrams**: Mô tả tương tác giữa các đối tượng theo thời gian.  

![](https://cdn-images.visual-paradigm.com/guide/uml/what-is-sequence-diagram/01-sequence-diagram-example.png)


- **State-chart diagrams**: Mô tả trạng thái và chuyển đổi trạng thái.  

![](https://www.uml-diagrams.org/notation/behavioral-state-machine-frame.png)

### 1.5 Thiết kế giao diện  
- **UI design**: Wireframe, Mockup cho toàn hệ thống.  

![](https://cdn.prod.website-files.com/685d3f27e667cdf05fe197f8/685d3f27e667cdf05fe1bb16_62cdb498f2fe30d45e9c465a_Blog_UI%2520mockups_wireframe%2520example.jpeg)

### 1.6 Kiến trúc hệ thống  
- **Deployment view**: Sơ đồ triển khai (server, client, database, cloud…).  

- **Development/Implementation view**: Cấu trúc module, package, framework.  

![](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b9/Deployment_Diagram.PNG/1200px-Deployment_Diagram.PNG)

### 1.7 Mô hình dữ liệu và lớp  
- **Class diagram**: Toàn bộ class và quan hệ.  

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRC6qGtJV0djsC5_px4njPG_4Of4GqlWtrnQw&s)

- **Method description**: Mô tả chi tiết các phương thức.

- **Database diagram (ERD)**: Mô hình dữ liệu, quan hệ giữa các bảng.  

![](https://upload.wikimedia.org/wikipedia/commons/7/72/ER_Diagram_MMORPG.png)

### 1.8 Thiết kế chức năng đặc biệt  
- Các module khó, ví dụ:  
  - AI (hệ gợi ý, NLP, ML, Chatbot…).  
  - Xử lý dữ liệu lớn (Big Data).  
  - Bảo mật (Authentication, Authorization, Encryption).  

![](https://markovate.com/wp-content/uploads/2024/04/Exploring-Advanced-RAG-Techniques-for-AI.webp)

---

## 2. Hiện Thực và Kiểm Thử  
*(Đồ Án Tốt Nghiệp kỳ 2 năm 4 hướng CNPM)*  

### 2.1 Công nghệ sử dụng  
- **Frontend**: ReactJS, Angular, Vue, hoặc Flutter (tùy dự án).  
- **Backend**: Node.js, Django, Spring Boot…  
- **Database**: MySQL, PostgreSQL, MongoDB…  
- **AI/ML frameworks** (nếu có): TensorFlow, PyTorch.  

### 2.2 Hiện thực  
- Xây dựng từng module theo thiết kế.  
- Kết nối frontend ↔ backend ↔ database.  
- Triển khai API, service, microservice (nếu có).  

### 2.3 Kiểm thử  
- **Unit test**: Kiểm thử từng hàm / method.  
- **Integration test**: Kiểm thử tích hợp module.  
- **System test**: Kiểm thử toàn hệ thống.  
- **Acceptance test**: Kiểm thử với người dùng cuối.  
- **Automated test** (bonus): Selenium, Jest, Pytest…  

---

## 3. Triển Khai và Bảo Trì  
*(Khi đi làm thực tế)*  

### 3.1 Triển khai  
- Deploy hệ thống lên **server / cloud (AWS, GCP, Azure, Docker, Kubernetes)**.  
- Cấu hình domain, SSL, reverse proxy (Nginx, Apache).  

### 3.2 CI/CD  
- Sử dụng **GitHub Actions / GitLab CI / Jenkins** để tự động hóa build & deploy.  
- Pipeline:  
  1. Pull code → Build → Test → Deploy → Monitor.  

### 3.3 Bảo trì  
- Sửa lỗi (bug fixing).  
- Tối ưu hiệu năng.  
- Cập nhật công nghệ mới.  
- Thêm tính năng mới theo yêu cầu người dùng.  

---