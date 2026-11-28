# RBAC

Dựa theo vai trò của người dùng mà các chức năng cũng như phạm vi áp dụng sẽ khác nhau.

## Role

| Vai trò | Phạm vi chức năng |
| --- | --- |
| **`user`** | Vai trò mặc định sau khi kích hoạt tài khoản. Cho phép quản lý hồ sơ cá nhân, đăng bài viết, bình luận, thả biểu cảm, lưu bài viết, theo dõi tài khoản khác và sử dụng tính năng dịch tự động. |
| **`moderator`** | Bao gồm toàn bộ chức năng của `user`, bổ sung quyền thay đổi trạng thái hiển thị hoặc khôi phục bài viết, bình luận, khóa bình luận trên bài viết, tạo thông báo hệ thống và điều chỉnh trạng thái hoạt động của tài khoản `user`. |
| **`admin`** | Bao gồm toàn bộ chức năng của `moderator`, bổ sung quyền quản lý trạng thái đối với tài khoản `moderator` và thay đổi vai trò của tài khoản giữa hai mức `user` và `moderator`. |

## Status

| Trạng thái | Xem nội dung công khai | Tạo mới, chỉnh sửa, tương tác | Đăng nhập và duy trì phiên | Mô tả áp dụng |
| --- | --- | --- | --- | --- |
| **`pending`** | Không | Không | Không | Trạng thái mặc định ngay sau khi gửi thông tin đăng ký. Tài khoản chưa được kích hoạt chính thức cho đến khi hoàn tất bước xác minh địa chỉ email. |
| **`active`** | Có | Có | Có | Trạng thái hoạt động bình thường sau khi xác thực email thành công. Tài khoản có thể sử dụng đầy đủ các chức năng theo vai trò được cấp. |
| **`active` (kèm `muted_until`)** | Có | Không | Có | Tài khoản đang bị tạm dừng tương tác có thời hạn. Người dùng vẫn đăng nhập và đọc nội dung công khai nhưng bị chặn mọi thao tác tạo hoặc chỉnh sửa dữ liệu cho đến khi hết hạn. |
| **`banned`** | Không | Không | Không | Trạng thái đình chỉ truy cập. Tài khoản bị từ chối đăng nhập và toàn bộ các phiên làm việc đang tồn tại đều lập tức mất hiệu lực. |

## Ma trận phân quyền 

### 1. Quản lý tài khoản và phân quyền

| Chức năng | Khách (Chưa xác thực) | `user` (Chính chủ) | `user` (Tài khoản khác) | `moderator` | `admin` |
| --- | --- | --- | --- | --- | --- |
| Đăng ký, đăng nhập tài khoản | Có | — | — | — | — |
| Đăng xuất (thu hồi phiên mọi thiết bị) | Không | Có | Không | Có | Có |
| Xem hồ sơ công khai | Có | Có | Có | Có | Có |
| Cập nhật hồ sơ cá nhân, đổi mật khẩu | Không | Có | Không | Có (chính chủ) | Có (chính chủ) |
| Tự xóa tài khoản cá nhân | Không | Có | Không | Có (chính chủ) | Không |
| Khóa / Mở khóa tài khoản (`banned`) | Không | Không | Không | Chỉ với `user` | Với `user`, `moderator` |
| Cấm tương tác có thời hạn (`muted_until`) | Không | Không | Không | Chỉ với `user` | Với `user`, `moderator` |
| Thay đổi vai trò (`user` hoặc `moderator`) | Không | Không | Không | Không | Với `user`, `moderator` |

### 2. Bài viết, bình luận và thẻ phân loại

| Chức năng | Khách (Chưa xác thực) | `user` (Tác giả) | `user` (Người khác) | `moderator` | `admin` |
| --- | --- | --- | --- | --- | --- |
| Xem bài viết, bình luận công khai | Có | Có | Có | Có | Có |
| Đăng bài viết mới | Không | Có | — | Có | Có |
| Chỉnh sửa nội dung bài viết | Không | Có (khi chưa bị ẩn) | Không | Không | Không |
| Xóa bài viết (xóa mềm) | Không | Có | Không | Không | Không |
| Khóa / Mở khóa bình luận trên bài viết | Không | Có | Không | Có | Có |
| Thay đổi trạng thái kiểm duyệt bài viết | Không | Không | Không | Có | Có |
| Khôi phục bài viết đã xóa | Không | Không | Không | Có | Có |
| Gửi bình luận và phản hồi bình luận | Không | Có (khi chưa khóa) | Có (khi chưa khóa) | Có | Có |
| Chỉnh sửa, xóa bình luận của mình | Không | Có | Không | Không | Không |
| Xem danh sách bình luận kiểm duyệt | Không | Không | Không | Có | Có |
| Thay đổi trạng thái, khôi phục bình luận | Không | Không | Không | Có | Có |
| Xem danh sách thẻ và bài viết theo thẻ | Có | Có | Có | Có | Có |
| Gắn thẻ hoặc gỡ thẻ khỏi bài viết | Không | Có | Không | Có | Có |

### 3. Tương tác xã hội, thông báo và dịch thuật

| Chức năng | Khách (Chưa xác thực) | `user` (Chính chủ) | `user` (Người khác) | `moderator` | `admin` |
| --- | --- | --- | --- | --- | --- |
| Lưu / Bỏ lưu bài viết cá nhân | Không | Có | Không | Có | Có |
| Xem thống kê biểu cảm bài viết | Có | Có | Có | Có | Có |
| Thả / Thay đổi / Gỡ biểu cảm bài viết | Không | Có | Có | Có | Có |
| Theo dõi / Hủy theo dõi tài khoản khác | Không | — | Có | Có | Có |
| Tạo thông báo nhắc tên, thông báo bài mới | Không | — | Có | Có | Có |
| Tạo thông báo loại hệ thống | Không | Không | Không | Có | Có |
| Xem, đánh dấu đã đọc, xóa thông báo | Không | Có | Không | Có (chính chủ) | Có (chính chủ) |
| Yêu cầu dịch bài viết và bình luận | Không | Có | Có | Có | Có |

## Ràng buộc 

* **Bảo toàn nội dung gốc**: Chỉ tác giả mới có quyền chỉnh sửa nội dung văn bản của bài viết, bình luận hoặc thông tin hồ sơ cá nhân. Đội ngũ `moderator` và `admin` thực hiện kiểm duyệt thông qua việc thay đổi trạng thái hiển thị hoặc khôi phục dữ liệu đã xóa mềm.
* **Giới hạn phân cấp quản trị**: Tài khoản `moderator` chỉ được điều chỉnh trạng thái hoạt động của tài khoản `user`, không thể tác động lên một `moderator` khác hoặc `admin`. Chỉ `admin` được phân quyền giữa `user` và `moderator`, đồng thời hệ thống không cho phép gán vai trò `admin` qua API.
* **Ngăn chặn tự tác động**: Tài khoản quản trị không thể tự khóa, tự cấm tương tác hoặc tự thay đổi vai trò của chính mình; tài khoản `admin` không thể sử dụng tính năng tự xóa tài khoản. Người dùng không thể tự theo dõi hoặc tự gửi thông báo nhắc tên cho chính mình.
* **Ràng buộc theo trạng thái nội dung**: Tác giả không thể chỉnh sửa bài viết đã bị chuyển sang trạng thái ẩn. Khi bài viết bật chế độ khóa bình luận, toàn bộ người dùng không thể tạo bình luận hoặc phản hồi mới trên bài viết đó.
