# Component Diagrams

**Component Diagram** (Sơ đồ thành phần) dùng để mô tả **cấu trúc cấp cao** của hệ thống phần mềm — tức là **hệ thống được tạo thành từ những thành phần nào**, và **các thành phần đó tương tác ra sao**.

![](https://www.uml-diagrams.org/component-diagrams/component-diagram-overview.png)

| **Mục**                           | **Ý nghĩa chính**                                                   |
| --------------------------------- | ------------------------------------------------------------------- |
| **Component Diagram**             | Mô tả cách chia hệ thống thành các module lớn và quan hệ giữa chúng |
| **Component**                     | Một module phần mềm độc lập                                         |
| **Provided / Required Interface** | Các dịch vụ được cung cấp hoặc yêu cầu giữa các component           |
| **Port**                          | Điểm giao tiếp giữa component và bên ngoài                          |
| **Connector**                     | Liên kết giữa các component                                         |
| **Component Realization**         | Lớp hiện thực hóa component đó                                      |

--- 

## Component

* **Component**: module phần mềm độc lập, đóng gói, có thể thay thế.
* **Behavior** xác định qua **provided interface** (cung cấp) và **required interface** (yêu cầu).
* Hiện thực bởi **artifact** (file `.jar`, `.dll`, `.py`…).

![](https://ducmanhphan.github.io/img/UML/interfaces/provided-&&-required-interfaces.png)

### Interfaces

| Loại     | Ý nghĩa                     | Notation |
| -------- | --------------------------- | -------- |
| Provided | Cung cấp cho component khác | Lollipop |
| Required | Yêu cầu từ component khác   | Socket   |


### Stereotypes tiêu chuẩn

* `«BuildComponent»`: cho phát triển, version
* `«Entity»`: persistent data
* `«Service»`: chức năng stateless
* `«Specification»`: chỉ định domain, không hiện thực
* `«Realization»`: class hiện thực specification
* `«Process»`: transaction-based
* `«Subsystem»`: thành phần lớn, phân cấp hệ thống


### Nguyên tắc

* **Encapsulation:** ẩn chi tiết, chỉ interface được nhìn thấy
* **Reusability:** có thể tái sử dụng
* **Replaceability:** thay thế component tương thích không ảnh hưởng hệ thống
* **Deployment:** triển khai qua artifact

--- 

## Connector

* **Connector** là **liên kết** giữa hai hoặc nhiều **instances** (thực thể) trong một **structured classifier** (ví dụ component).
* Connector cho phép **giao tiếp hoặc chuyển tín hiệu** giữa các instance.
* Khác với association (kết nối giữa class), connector **kết nối các instance cụ thể**, không phải mọi instance của class.
* Có **hai loại connector chính**:

![](https://media.licdn.com/dms/image/v2/C4D12AQHkE0haKnqpBQ/article-inline_image-shrink_1000_1488/article-inline_image-shrink_1000_1488/0/1649921932031?e=2147483647&v=beta&t=04OlHI1w-oleZ4IfvfeET34cOcMWyzeB5Jnr3RQYitc)

### Assembly Connector

* Kết nối giữa **hai hoặc nhiều part/port**.
* Một part cung cấp **service** mà part khác **sử dụng**.
* Notation: ball-and-socket cho simple ports, hoặc connector line cho n-ary connectors.
* Cho phép **thay thế component** nếu interfaces tương thích.


### Delegation Connector

* Kết nối từ **external port của component** đến **realization (hiện thực) của behavior**.
* Chuyển tiếp **signal/operation** đến port hoặc part bên trong.
* Mô hình hóa **hierarchical decomposition**, tức services bên ngoài có thể được thực hiện bởi component nested nhiều tầng.
* Notation: connector từ **delegating port** đến **target port/part**.

