# SOC257 - VPN Connection Detected from Unauthorized Country

## Information

Event ID: 225

Rule: SOC257 - VPN Connection Detected from Unauthorized Country

Time:13/02/2024, 02:04 AM

![image.png](image.png)

## Alert

Đây là một cảnh báo về một kết nối VPN từ một quốc gia trái phép. Để kiểm chứng cảnh báo trên thì chúng ta cần phải điều tra trong Log Management xem những log liên quan đến IP source 113.161.158.12 này đã làm những gì.

![image.png](image%201.png)

Kết quả tìm kiếm cho thấy IP này gửi rất nhiều request đăng nhập thành công tới dịch vụ VPN nhưng khi tới bước xác thực mức 2 của tường lửa (mã OTP) thì lại failed hoàn toàn suy đoán có thể đây là một vụ tấn công do người dùng nội bộ bị lộ thông tin tài khoản/bị bruteforce và hacker không vượt qua phần xác thực mức 2 OTP.

![image.png](image%202.png)

![image.png](image%203.png)

Giả thuyết nếu bấm nhầm mã OTP thì có thể nhầm 1 - 2 lần và sau cùng phải có 1 lần nhập thành công nhưng theo kết quả điều tra cho thấy họ đã nhập sai nhiều lần và không có lần nào thành công.

![image.png](image%204.png)

![image.png](image%205.png)

Cho ta thấy đây là một True Positive alert về việc truy cập trái phép vào dịch vụ.

Dùng AbuseIPdb để check xem độ uy tín của IP gửi yêu cầu đến thì thấy IP này là 1 IP được cung cấp bởi một nhà mạng Việt Nam **vnpt** đã thực hiện rất nhiều cuộc tấn công và độ uy tín =0%.

![image.png](image%206.png)

## Start Playbook

![image.png](image%207.png)

- True Positive

![image.png](image%208.png)

- Đây là một cuộc tấn công truy cập trái phép nhưng không thành công

![image.png](image%209.png)

- Đây là một cuộc tấn công từ thiết bị bên ngoài với IP độc hại đã từng tấn công nhiều mục tiêu.

![image.png](image%2010.png)

- Check IP trên AbuseIPdb thì ra độ uy tín 0% với nhiều “tiền án”.

![image.png](image%2011.png)

- Như đã nói ở cuộc xâm nhập này thực hiện không thành công.

![image.png](image%2012.png)

- Đăng nhập thất bại nên không khai thác được thông tin gì, Mặc dù cũng là dấu hiệu cho thấy khả năng bảo mật bị ảnh hưởng.

![image.png](image%2013.png)

- Không cần thiết vì máy nạn nhân chưa bị ảnh hưởng bởi cuộc tấn công cũng chưa dính mã độc.

![image.png](image%2014.png)

## Close Alert

![image.png](image%2015.png)

## MITRE  ATT&CK

![image.png](image%2016.png)

## Artifacts

Field

Attacker IP

Victim’s Email

Value

113.161.158.12

[monica@letsdefend.io](mailto:monica@letsdefend.io)