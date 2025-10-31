            # Hệ thống Thương mại Điện tử Microservices

            Đây là một dự án mô phỏng hệ thống thương mại điện tử cơ bản, được xây dựng theo kiến trúc Microservices. Dự án bao gồm các dịch vụ độc lập để xử lý các nghiệp vụ khác nhau như xác thực, quản lý sản phẩm và đặt hàng. Toàn bộ hệ thống được đóng gói và vận hành bằng Docker.



            [Image of a microservices architecture diagram]


            ---

            ## Kiến trúc Hệ thống

            Dự án được chia thành 4 dịch vụ chính:

            `api-gateway`: Là cổng vào duy nhất của hệ thống. Mọi yêu cầu từ client sẽ đi qua đây trước khi được định tuyến đến dịch vụ phù hợp.
            `auth-service`: Chịu trách nhiệm xử lý tất cả các vấn đề liên quan đến xác thực người dùng, bao gồm đăng ký, đăng nhập và tạo JSON Web Token (JWT).
            `product-service`: Quản lý toàn bộ thông tin liên quan đến sản phẩm (tạo mới, xem danh sách sản phẩm).
            `order-service`: Xử lý các nghiệp vụ liên quan đến việc tạo đơn hàng.

            Các dịch vụ giao tiếp với nhau qua mạng nội bộ do Docker tạo ra, và mỗi dịch vụ có một cơ sở dữ liệu MongoDB riêng biệt.

            ---

            ## Hướng dẫn Cài đặt và Khởi chạy

            ### Yêu cầu hệ thống
            * [Docker](https://www.docker.com/products/docker-desktop/) và Docker Compose đã được cài đặt.

            ### Khởi chạy hệ thống
            1.  Clone repository này về máy của bạn.
            2.  Mở Terminal hoặc PowerShell tại thư mục gốc của dự án.
            3.  Chạy lệnh sau để build và khởi động toàn bộ hệ thống:
                ```bash
                docker-compose up --build
                ```
            4.  Hệ thống sẽ khởi chạy thành công khi bạn thấy log của tất cả các dịch vụ thông báo đã kết nối tới MongoDB và đang lắng nghe trên các cổng tương ứng.

            ---

            ## 🛠️ Kiểm thử API với Postman

            Sau khi hệ thống đã khởi chạy, bạn có thể sử dụng Postman để kiểm tra các chức năng. Tất cả các yêu cầu đều được gửi đến API Gateway qua cổng **`8088`**.

            #### 1. Đăng ký tài khoản
            * **Method**: `POST`
            * **URL**: `http://localhost:8088/auth/register`
            * **Body**: `raw` (JSON)
                ```json
                {
                    "username": "testuser",
                    "password": "password123"
                }
                ```

            #### 2. Đăng nhập
            * **Method**: `POST`
            * **URL**: `http://localhost:8088/auth/login`
            * **Body**: `raw` (JSON)
                ```json
                {
                    "username": "testuser",
                    "password": "password123"
                }
                ```
            Hành động: Copy giá trị `accessToken` từ kết quả trả về.

            #### 3. Tạo sản phẩm mới**
            * Method: `POST`
            * URL: `http://localhost:8088/products`
            * Authorization: Chọn `Bearer Token` và dán `accessToken` đã copy.
            * Body: `raw` (JSON)
                ```json
                {
                    "name": "Áo Thun Basic Cotton",
                    "price": 350000,
                    "description": "Chất liệu 100% cotton thoáng mát"
                }
                ```
            Hành động: Copy giá trị `_id` của sản phẩm vừa tạo.

            #### 4. Tạo đơn hàng
            * Method: `POST`
            * URL: `http://localhost:8088/orders`
            * Authorization**: Chọn `Bearer Token` và dán `accessToken`.
            * Body: `raw` (JSON)
                ```json
                {
                    "products": ["<ID_SẢN_PHẨM_VỪA_COPY>"],
                    "totalPrice": 350000
                }
                ```

            ---

            ## Công nghệ sử dụng

            * Backend: Node.js, Express.js
            * Database: MongoDB (với Mongoose)
            * Kiến trúc: Microservices, API Gateway
            * Containerization: Docker, Docker Compose
            * Xác thực: JSON Web Token (JWT)
