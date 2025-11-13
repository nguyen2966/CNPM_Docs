# Deployment Diagrams Overview

* Là **structure diagram** mô tả **kiến trúc hệ thống** dưới dạng **triển khai phần mềm (artifacts) lên các deployment targets** (phần cứng hoặc môi trường thực thi).
* Giúp hình dung **cách phần mềm được phân phối và chạy trên các node**.

![](https://www.uml-diagrams.org/examples/deployment-example-clusters.png)

## **Các thành phần chính**

| Thành phần                   | Ý nghĩa                                                                                             |
| ---------------------------- | --------------------------------------------------------------------------------------------------- |
| **Artifact**                 | Phần tử vật lý, kết quả của quá trình phát triển: file thực thi, thư viện, database schema, config… |
| **Deployment Target / Node** | Mục tiêu triển khai: máy chủ, client, môi trường ảo…                                                |
| **Component**                | Module phần mềm, được **manifest (hiện thực) bởi artifact**.                                        |
| **Communication Path**       | Kết nối giữa các node, cho phép giao tiếp mạng hoặc trao đổi dữ liệu.                               |

## **Các loại Deployment Diagram**

1. **Specification Level (Type Level)**

   * Mô tả tổng quan **triển khai artifact lên target**, **không nêu cụ thể các instance**.

2. **Instance Level**

   * Mô tả **triển khai các instance cụ thể** của artifact lên các node cụ thể.
   * Dùng để phân biệt **môi trường development, staging, production**.

3. **Manifestation Diagram**

   * Thể hiện **component được hiện thực bởi artifact**.
   * UML 2.x cho phép thể hiện bằng **component diagram hoặc deployment diagram**.

4. **Network Architecture**

   * Hiển thị **mạng lưới logical/physical** của hệ thống, với hoặc không với artifacts.
