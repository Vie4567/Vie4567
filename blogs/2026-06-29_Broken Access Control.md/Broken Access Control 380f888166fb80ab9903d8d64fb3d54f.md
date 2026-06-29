# Broken Access Control

## Broken Access Control là gì?

Broken Access Control là lỗ hỏng xảy ra khi không kiểm tra **Quyền truy cập và danh tính của người dùng** có thể dẫn đến các trường hợp truy cập trái phép đến các dữ liệu như thông tin và quyền của tài khoản hoặc role khác.

## Authentication, Authorization, Accounting là gì?

- Authentication (Bạn là ai) Là bước xác thực danh tính người dùng thường thấy là khâu đăng nhập bằng username, password. Bước xác thực này dùng để xác định danh tính người dùng bằng 3 yếu tố chính thuộc về người sở hữu account:
    - something you are: là những yếu tố tạo thành người sở hữu như vân tay, móng mắt,…
    - something you have: là những gì bạn có và sở hữu như thiết bị điện thoại, thẻ nhân viên,…
    - something you know: là những gì bạn biết như thông tin cá nhân, gia đình, sở thích,…
- Authorization (Bạn được làm gì) là quá trình xác định người dùng được quyền truy cập vào những data gì và thực hiện những hành vi gì. Thường dựa trên role và danh tính của người dùng. Các cơ chế phân quyền phổ biến.
    - DAC (Discretionary Access Control) Người sở hữu quyết định ai được truy cập. Ví dụ up 1 file lên Drive là được chia sẻ cho bất cứ ai mà người sở hữu đó muốn.
    - MAC (Mandatory Access Control) Đây là một cơ chế phân quyền bảo mật rất cao thường áp dụng cho các hệ thống Bộ Quốc Phòng hoặc Chính Phủ để tránh rò rỉ thông tin mạnh mẽ. Các resource khi sinh ra đều được gán nhãn và người dùng thì có mức độ giải mật (Clearance Levels). Nổi bật là mô hình Bell Lapapula (No read up - No write down).
    - RBAC (Role Base Access Control) Phân chia quyền truy cập tài nguyên dựa trên các vai trò được hệ thống định nghĩa sẵn. Như root thì được truy cập tất cả tài nguyên, Admin thì được truy cập Tài nguyên quản trị, Thống kê,… User thì chỉ được truy cập những dữ liệu public,…
    - ABAC (Attribute Base Access Control) Phân quyền truy cập dựa trên các Attribute của
        - Resource: loại tài liệu, cơ sở, chi nhánh, phòng ban, nhãn nhạy cảm,…
        - User: vị trí, phòng ban, chi nhánh,…
        - Action: viết, sửa, xóa,…
        - Context: thời gian truy cập, IP, địa chỉ địa lý, VPN,…
        
        Sau đó so sánh với các policy được set trước của hệ thống nếu hợp lệ thì được phép truy cập đến. Ví dụ như: HR chỉ có thể truy cập đến dữ liệu tuyển dụng cùng quốc gia,…
        

## SOP là gì?

SOP là viết tắt của same-site Policy, chính sách bảo mật của trình duyệt ngăn việc gửi response và leak data từ một origin khác chống lại các cuộc tấn công CSRF.

> Origin gồm 3 phần:
                                     Protocol + Domain + Port
                                     https://domain.com:443
Khác Origin được định nghĩa là khác một trong 3 thành phần trên ví dụ:
   https://domain.com:443 ≠ http://domain.com:443
   https://app.domain.com:443 ≠ https://api.domain.com:443
   https://domain.com:9000 ≠ https://domain.com:8080
> 

Vậy Cross-origin request là gì?

> Cross-origin request là việc thực hiện truy cập đến dữ liệu của một origin từ một origin khác.
Ví dụ: từ [https://facebook.com](https://facebook.com) truy cập đến [https://github.com](https://github.com) từ một comment.
> 

Các cờ của thuộc tính samesite:

> samesite Attribute là thuộc tính quyết định có gửi kèm cookie cho các Cross-origin hay không
> 
- Strict: Đây là một cờ với độ bảo mật khá cao vì sẽ không bao giờ gửi kèm cookie với các yêu cầu chéo trang. Với các yêu cầu từ các Origin khác trình duyệt sẽ hiểu là người dùng chưa đăng nhập và truy cập dữ liệu thì phải đăng nhập hoặc xác thực lại.
- Lax: Đây là một cờ cân bằng hơn, cho phép truy cập đến các URL bình thường và ngăn gửi cookie với hầu hết các URL nhạy cảm như có method POST,…
- None: Trình duyệt sẽ đính kèm cookie với mọi request đến.

## CORS là gì?

CORS là viết tắt của Cross-Origin resource sharing là phần cấu hình ngoại lệ cho SOP, Khi web cần lấy dữ liệu hoặc call api đến dịch vụ của partner nếu không có CORS thì response từ partner trả về sẽ bị trình duyệt chặn lại do khác Origin

> Example
https://abc.com → [https://xyz.com](https://xyz.com) 

Get user/profile → 200 ok Alice
Host https://xyz.com

Origin https://abc.com
> 

Nhưng sẽ bị trình duyệt chặn phản hồi này do https://abc.com và https://xyz.com là 2 origin khác nhau. Nếu muốn sử dụng API từ đối tác thì bên họ phải sửa lại cấu hình CORS, thuộc tính Access-Control-Allow-Origin: https://abc.com.

Các thuộc tính thường thấy của CORS:

- Access-Control-Allow-Origin: Origin nào được phép đọc response.

```jsx
// allow specific origin(Recommence).
Access-Control-Allow-Origin: https://abc.com.
// allow all
Access-Control-Allow-Origin: *
```

- Access-Control-Allow-Method: Khai báo các HTTP Method được phép.

```jsx
Access-Control-Allow-Methods:
GET, POST, PUT, DELETE
```

- Access-Control-Allow-Credential: Cho phép gửi các thông tin xác thực như Cookie, Authorization header,…

```jsx
Access-Control-Allow-Credentials: true
```

- Access-Control-Allow-Headers: Khai báo các request header được phép gửi.

```jsx
Access-Control-Allow-Headers:
Authorization, Content-Type
```

- Access-Control-Request-Method: Header do browser gửi trong preflight. Method có trong list thì phải xin phép trước khi gửi.

```jsx
OPTIONS /users

Access-Control-Request-Method:
PUT
```

## Các lỗ hỏng phổ biến thuộc Broken Access Control

### 1. IDOR (Insecure Direct Object Reference)

Đây là lỗ hỏng phổ biến nhất ở việc không kiểm tra quyền truy cập đến dự liệu của người dùng đúng cách khiến cho attacker có thể xem và tác động đến dữ liệu trái phép bằng cách sửa ID trên URL.

Khi người dùng gửi Request API để xem profile như sau:

```jsx
GET api/users/?id=1
```

để xem thông tin cá nhân của người dùng, nhưng khi đổi id thành id bất kì và gửi Request như:

```jsx
GET api/users/?id=2
```

```jsx
GET api/users/?id=3
```

Ứng dụng vẫn trả về dữ liệu người dùng khác thì ứng dụng đã bị tấn công IDOR.

### Nguyên nhân

Nguyên nhân chính là do server chỉ truy vấn ID của người dùng khi yêu cầu gửi đến và trả về kết quả của họ mà không kiểm tra quyền truy cập dữ liệu có hợp lệ hay không.

Thay vì kiểm tra Id từ JWT token và kiểm tra quyền truy cập thì các dev chỉ truy vấn id từ parameter từ body API truyền vào và trả về dữ liệu không qua kiểm duyệt.

```jsx
// Ví dụ Mã giả
user.find(
	"userId": id;
)
```

### Kết quả

IDOR có thể dẫn đến hành vi xem và sửa dữ liệu của người dùng khác mà đáng lẽ không có quyền truy cập → lộ thông tin cá nhân và các thông tin nhạy cảm, chỉnh sửa khiến phá vỡ tính bảo toàn dữ liệu hoặc làm một hành động dưới danh nghĩa một người khác, đôi khi có thể chiếm quyền tài khoản.

## 2. Forced Browsing

Các Dev đôi khi lại chỉ ngăn người dùng truy cập tính năng bằng việc ẩn nút sử dụng hoặc truy cập chức năng mà không kiểm tra quyền ở Backend bằng cách bruteforce URL Kẻ tấn công có thể truy cập vào những chức năng trái phép.

Ví dụ:

```jsx
//URL 
https://abc.com

//Admin Dashboard URL
https://abc.com/dashboard
```

Có thể thấy nếu chỉ đơn giản là ẩn nút trên UI thì khi hacker dùng burpsuit hay công cụ tấn công bruteforce URL thì có thể truy cập vào chức năng không được phân quyền.

### Nguyên nhân

Nguyên nhân chính là do các dev chỉ kiểm tra role ở Frontend nếu là role này thì thể hiện UI với các nút chức năng như thế nào mà quên mất phải kiểm tra phân quyền ở Backend khiến cho việc kẻ tấn công dò đến Endpoint có chức năng nhạy cảm và thực hiện chức năng hoặc xem dữ liệu một cách không kiểm duyệt.

### Hậu quả

- Có thể xem được các dữ liệu nhạy cảm cuẩ hệ thống, người dùng khác.
- Thêm, sửa, xóa một số dữ liệu.
- Chiếm quyền Admin.

## 3. CSRF

## 4. SSRF