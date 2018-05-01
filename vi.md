# MySQL :: Hướng dẫn tham khảo về MySQL 5.7 :: 4.2.6 Sử dụng các file tùy chọn

### 4.2.6 Sử dụng các file tùy chọn

Hầu hết các chương trình MySQL có thể đọc các tùy chọn khởi tạo từ các tùy chọn file (thi thoảng chúng được gọi là các file cấu hình). Các file tùy chọn cung cấp 1 cách tiện lợi để chỉ định các tùy chọn thường được sử dụng vì vậy chúng không cần phải được nhập từ các dòng lệnh mỗi khi chương trình của bạn chạy.

Để xác minh liệu một chương trình có đọc các file tùy chọn hay không, hãy gọi chúng bằng tùy chọn `\ --help`. (Đối với [**mysqld**][1], sử dụng [`\--verbose`][2] và [`\--help`][3].). Nếu một chương trình đọc các file tùy chọn, thông báo trợ giúp cho biết file nào sẽ tìm và nhóm tùy chọn nào sẽ nhận ra.

Lưu ý

Một chương trình MySQL được bắt đầu với tùy chọn `\--no-defaults` không đọc các file tùy chọn nào ngoài `.mylogin.cnf`. 

Rất nhiều các file tùy chọn là các file văn bản thuần túy, được tạo để sử dụng cho bất cứ trình soạn thảo nào. Ngoại trừ file `.mylogin.cnf` chứa những tùy chọn đường dẫn đăng nhập. Cái này là 1 file đã được mã hóa và tạo ra bởi tiện ích [**mysql_config_editor**][4]. Xem thêm tại [Section 4.6.6, "**mysql_config_editor** — MySQL Configuration Utility"][4]. Một "đường dẫn đăng nhập" ;à một nhóm tùy chọn mà chỉ các tùy chọn cụ thể là: host`, `user`, `password`, `port` và `socket`. Các chương trình phía client chỉ định đường dẫn đăng nhập nào sẽ đọc từ `.mylogin.cnf` sử dụng tùy chọn [`\--login-path`][5].

Để chỉ định một tên file chứa đường dẫn đăng nhập thay thế, cài đặt biến môi trường trong `MYSQL_TEST_LOGIN_FILE`. Biến này được sử dụng trong tiện ích test **mysql-test-run.pl**, nhưng cũng được tìm thấy trong [**mysql_config_editor**][4] và các MySQL client như [**mysql**][6], [**mysqladmin**][7] , v.v..

MySQL tìm kiếm các file tùy chọn theo thứ tự đã được mô tả trong phần thảo luận dưới đây và đọc bất kì cái nào tồn tại. Nếu một file tùy chọn bạn muốn dùng không tồn tại, hãy tạo chúng bằng cách sử dụng phương thức phù hợp, như vừa được đề cập

Trên Windows, các chương trình MySQL đọc các tùy chọn khởi tạo từ các file được hiển thị trong bảng dưới đây, trong thứ tự đã được sắp xếp (các file liệt kê đầu tiên được đọc đầu tiên, file liệt kê sau sẽ được đọc sau).

**Bảng 4.1 Các file tùy chọn đọc trên Windows**

| Tên file                                                                                 | Mục đích                                                       |  
| ------------------------------------------------------------------------------------------ | ------------------------------------------------------------- |  
| ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.ini`, ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.cnf` | Tùy chọn toàn cục                                               |  
| ``%WINDIR%`my.ini`, ``%WINDIR%`my.cnf`                                                     | Tùy chọn toàn cục                                                 |  
| `C:my.ini`, `C:my.cnf`                                                                     | Tùy chọn toàn cục                                                 |  
| `_`BASEDIR`_my.ini`, `_`BASEDIR`_my.cnf`                                                   | Tùy chọn toàn cục                                                 |  
| `defaults-extra-file`                                                                      | file được chỉ định với [`\--defaults-extra-file`][8], nếu có |  
| ``%APPDATA%`MySQL.mylogin.cnf`                                                             | Các tùy chọn được dẫn đăng nhập (dành cho client)                             |  

Ở bảng trên, `%PROGRAMDATA%` đại diện thư mục file hệ thống mà chứa các dữ liệu của ứng dụng cho tất cả người dùng trong tổ chức. Đường dẫn này mặc định nằm trong `C:ProgramData` trong Microsoft Windows Vista v.v, và `C:Documents and SettingsAll UsersApplication Data` trong các phiên bản Microsoft Windows cũ hơn

`%WINDIR%` đại diện cho vị trí của thư mục Window của bạn. Thông thường là `C:WINDOWS`. Sử dụng câu lệnh dưới đây để xác minh thực sự vị trí từ giá trị của biến môi trường `WINDIR`:

    
    C:> echo %WINDIR%

`%APPDATA%` đại diện cho giá trị của thư mục chứa dữ liệu ứng dụng trên Windows. Sử dụng câu lệnh dưới đây để xác minh chính xác vị trí từ giá trị của biến môi trường `APPDATA`

    
    C:> echo %APPDATA%

_`BASEDIR`_ đại diện cho thư mục gốc cài MySQL. Khi MySQL 5.7 được cài đặt sử dụng MySQL Installer, thường là `C:_`PROGRAMDIR`_MySQLMySQL 5.7 Server` nơi mà where _`PROGRAMDIR`_ đại diện cho các thư mục chương trình (`Program Files` trong phiên bản tiếng anh của Window), xem thêm tại [Phần 2.3.3, "MySQL Installer cho Windows"][9]. 

Trong Unix và các hệ thống tương tự Unix, chương trình MySQL đọc các tùy chọn khởi tạo từ các file hiện thi trong bảng dưới đây, và đã được sắp xếp cụ thể (các file đứng trước đọc trước, các file đứng sau đọc sau).

Lưu ý

Trong nền tảng Unix, MySQL bỏ qua các file cấu hình mà được viết toàn cục. Đây là một sự cố ý để bảo mật.

**Bảng 4.2 Đọc các file tùy chọn trong Unix và hệ thống giống vs Unix**

| Tên file               | Mục đích                                                       |  
| ----------------------- | ------------------------------------------------------------- |  
| `/etc/my.cnf`           | Sử dụng toàn cục                                                |  
| `/etc/mysql/my.cnf`     | Sử dụng toàn cục                                                |  
| `_`SYSCONFDIR`_/my.cnf` | Sử dụng toàn cục                                                 |  
| `$MYSQL_HOME/my.cnf`    | Tùy chọn chỉ định của Server (dành cho server)                         |  
| `defaults-extra-file`   | file đã chỉ định với [`\--defaults-extra-file`][8], nếu có |  
| `~/.my.cnf`             | Tùy chọn chỉ định của User                                       |  
| `~/.mylogin.cnf`        | Tùy chọn đường dẫn đăng nhập chỉ định của USER (dành cho Client)               |  

Trong bảng trên, `~` đại diện cho thư mục home hiện tại của người dùng (giá trị của `$HOME`).

_`SYSCONFDIR`_ đại diện cho thư mục được chỉ định với tùy chọn [`SYSCONFDIR`][10] để **CMake** khi MySQL được xây dựng. Mặc định, đây là thư mục `etc` được đặt dưới thư mục biên dichj cài đặt.

`MYSQL_HOME` là 1 biến môi trường chứa đường dẫn đến thư mục mà trong đó file chỉ định server `my.cnf` nằm. Nếu `MYSQL_HOME` không được cài đặt và bạn khởi động server sử dụng chương trình [**mysqld_safe**][11], [**mysqld_safe**][11] thiết lập chúng vào _`BASEDIR`_, thư mục mà MySQL cài đặt vào.

_`DATADIR`_ thông thường là `/usr/local/mysql/data`, mặc dù nó có thể rất đa dạng trên mỗi nền tảng hoặc phương thức cài đặt. Giá trị của vị trí thư mục dữ liệu được xây dựng trong khi MySQL biên dịch, không phải nằm ở vị trí được chỉ định  vs tùy chọn [`\--datadir`][12] khi [**mysqld**][1] khởi động. Sử dụng [`\--datadir`][12] trong khi chạy không ảnh hưởng gì tới việc server tìm kiếm các file tuufy chọn cái mà nó đọc trước khi thực thi các tùy chọn.

Nếu nhiều các thể của một tùy chọn nhất định được tìm thấy, thì cá thể cuối cùng sẽ được ưu tiên, với một ngoại lệ: Đối với **[mysqld**][1], cá thể _first_ của tùy chọn `[\--user`][13] được sử dụng là một cách bảo mật, để ngăn chăn một người được chỉ định trong một file tùy chọn từ một câu lệnh đang ghi đè.

Đoạn mô tả dưới đây của một cú pháp file tùy chọn áp dụng cho những file mà bạn sửa bằng tay. Nó ngoại trừ file `.mylogin.cnf` mà được tạo để sử dụng **[mysql_config_editor**][4] và được mã hóa.

Bất kì một tùy chọn dài dòng nào được gửi đi bằng các dòng lệnh khi đang chạy một chương trình MySQL có thể được gửi tới môt file tùy chọn. Để lấy danh sách các tùy chọn có sẵn cho chương trình, chạy lệnh cùng với tùy chọn `\--help`. (Đối với **[mysqld**][1], sử dụng `[\--verbose`][2] và `[\--help`][3].)

Cú pháp để chỉ định tùy chọn trong một file tùy chọn là tương tự như cú pháp của dòng lệnh (xem thêm [Phần 4.2.4, "Sử dụng các tùy chon trên Command Line"][14]). Tuy nhiên, trong một file tùy chọn, bạn bỏ qua hai dấu gạch ngang trên đầu từ một tên tùy chọn và bạn chỉ định chỉ một tùy chọn trên một dòng. Ví dụ, `\--quick` và `\--host=localhost` trong các dòng lệnh nên được chỉ định rằng `quick` và `host=localhost` trên các dòng riêng lẻ trong một file tùy chọn. Để chỉ định một tùy chọn trong hình thức `\--loose-_`opt_name`_` trong một file tùy chon, viết nó là `\--loose-_`opt_name`_`.

Các dòng trống trong các file tùy chọn được bỏ qua. Các dòng khác có thể được thể hiện dưới các hình thức sau:

* `#_`comment`_`, `;_`comment`_`

Các dòng comment bắt đầu bằng `#` hay `;`. Một dòng comment `#` có thể bắt đầu ở giữa dòng.

* `[_`group`_]`

_`group`_ là tên của chương trình hoặc nhóm mà bạn muốn thiết lập các tùy chọn đối với chúng. Sau một nhóm dòng, bất kì dòng tùy chọn thiết lập nào áp dụng phải tên của các nhóm đã đặt đến hết file tùy chọn hoặc nhóm dòng khác được đưa ra. Tên của các nhóm tùy chọn không phải là chữ hoa.

* `_`opt_name`_`

Nó tương đương với `\--_`opt_name`_` trong dòng lệnh

* `_`opt_name`_=_`value`_`

Nó tương đượng với `\--_`opt_name`_=_`value`_` trong các dòng lệnh. Trong một file tùy chọn, bạn có thể có khoảng trống xung quanh dấu `=`, điều đó lại không đúng trong các dòng lệnh. Giá trị tùy chọn có thể được đặt trong dấu nháy đơn hoặc dấu ngoặc kép, rất hữu ích nếu giá trị chứa ký tự nhận xét `#`.

Không gian hàng đầu và cuối được tự động xóa khỏi tên và giá trị tùy chọn.

Bạn có thể sử dụng các trình tự thoát sau `b`, `t`, `n`, `r`, `\`, và `s` trong giá trị tùy chọn để đại diện cho nút backspace, tab, dòng mới, lùi lại, '\' và kí tự cách. Trong các file tùy chọn, các quy tắc thoát này được áp dụng:

* Dấu gạch chéo ngược theo sau là một ký tự chuỗi thoát hợp lệ được chuyển đổi thành ký tự được trình bày theo trình tự. Ví dụ, `s` được chuyển đổi thành một dấu cách.
* Dấu gạch chéo ngược không được theo sau bởi một ký tự chuỗi thoát hợp lệ vẫn không thay đổi. Ví dụ, `S` được giữ nguyên.

Các quy tắc trước có nghĩa là dấu gạch chéo ngược có thể được đưa ra dưới dạng `\`, hoặc là `` nếu nó không được theo sau bởi một ký tự chuỗi thoát hợp lệ.

Các quy tắc cho các chuỗi thoát trong các tệp tùy chọn khác nhau một chút so với các quy tắc cho các chuỗi thoát trong các chuỗi ký tự trong các câu lệnh SQL. Trong bối cảnh sau, nếu "_`x`_" không phải là ký tự thứ tự thoát hợp lệ, `_`x`_` trở thành" _`x`_ "thay vì` _`x`_`. Xem [Mục 9.1.1, "Chuỗi văn bản"] [15].

Các quy tắc thoát cho các giá trị tệp tùy chọn đặc biệt thích hợp cho các tên đường dẫn Windows, sử dụng `` làm dấu tách tên đường dẫn. Dấu phân cách trong tên đường dẫn Windows phải được viết là `\` nếu nó được theo sau bởi một ký tự chuỗi thoát. Nó có thể được viết dưới dạng `\` hoặc `` nếu không. Ngoài ra, `/` có thể được sử dụng trong tên đường dẫn Windows và sẽ được coi là ``. Giả sử bạn muốn chỉ định một thư mục cơ sở của `C: Program FilesMySQLMySQL Server 5.7` trong một tệp tùy chọn. Điều này có thể được thực hiện theo nhiều cách. Vài ví dụ:

    basedir="C:Program FilesMySQLMySQL Server 5.7"
    basedir="C:\Program Files\MySQL\MySQL Server 5.7"
    basedir="C:/Program Files/MySQL/MySQL Server 5.7"
    basedir=C:\ProgramsFiles\MySQL\MySQLsServers5.7

Nếu một tên nhóm tùy chọn trùng với tên một chương trình, các tùy chọn trong nhóm áp dụng cụ thể cho chương trình đó. Ví dụ, nhóm `[mysqld]` và `[mysql]` áp dụng cho chương trình **[mysqld**][1] server và client tương ứng.

Tùy chọn nhóm `[client]` được đọc bởi các chương trình client được cung cấp bởi sự phân phối của MySQL, (chứ _không_ phải **[mysqld**][1]). Để hiểu rõ cách mà các chương trình third-party client sử dụng C API có thể sử dụng các file tùy chọn, xem trong C API documentation tại [Phần 27.8.7.50, "mysql_options()"][16].

Các nhóm `[client]` cho phép bạn chỉ định các tùy chọn áp dụng cho tất cả các client. Ví dụ, `[client]` là một nhóm thích hợp để sử dụng chỉ định mật khẩu cho việc kết nối tới server. (Nhưng đảm bảo rằng file tùy chọn đó có thể được truy cập bởi riêng bạn, để những người khác không thể tìm ra mật khẩu của bạn). Đảm bảo rằng không đẩy file tùy chọn trong nhóm `[client]` trừ phi nó được nhận ra bởi _tất cả_ các chương trình client mà bạn sử dụng. Các chương trình mà không hiểu tùy chọn sẽ thoát sau khi hiển thi lỗi nếu bạn cố gắng chạy chúng.

Liệt kê các nhóm tùy chọn chung đầu tiên và các nhóm cụ thể ở sau.

Dưới đây là một tùy chọn file toàn cục cụ thể:
    
    [client]
    port=3306
    socket=/tmp/mysql.sock
    
    [mysqld]
    port=3306
    socket=/tmp/mysql.sock
    key_buffer_size=16M
    max_allowed_packet=8M
    
    [mysqldump]
    quick

Dưới đây là một tùy chọn file user cụ thể:
    
    
    [client]
    # The following password will be sent to all standard MySQL clients
    password="my password"
    
    [mysql]
    no-auto-rehash
    connect_timeout=2

Để tạo ra các nhóm tùy chọn  và được đọc chỉ bởi các chương trình **[mysqld**][1] server từ các bài dẫn chỉ định của MySQL, sử dụng các nhóm với tên `[mysqld-5.6]`, `[mysqld-5.7]`, .v.v. Nhóm dưới đây cho biết thiết lập `[sql_mode`][18] nên được sử dụng bởi MySQL server với phiên bản 5.7.x:

    
    [mysqld-5.7]
    sql_mode=TRADITIONAL

Nó có thể sử dụng `!include` trực tiếp trong các file tùy chọn để thêm các file tùy chọn khác và `!includedir` để tìm cụ thể các thư mục chứa file tùy chọn. Ví dụ, để thêm file `/home/mydir/myopt.cnf`, sử dụng trực tiếp như sau:

    
    !include /home/mydir/myopt.cnf

Để tìm thư mục `/home/mydir` và đọc những file tùy chọn tìm được ở đó, sử dụng: 
    
    !includedir /home/mydir

MySQL không tạo ra sự bảo vệ đối với những thư mục được đưa ra mà ở đó các file tùy chọn sẽ được đọc.

Lưu ý

Bất kì một file nào được tìm và thêm vào thông qua việc `!includedir` trực tiếp trên các hệ thống Unix _phải_ có tên kết thúc bằng `.cnf`. Trong Windows, nó sẽ kiểm tra trực tiếp các file kết thúc là `.ini` hoặc `.cnf`.

Viết nội dung cho một file tùy chọn đã được thêm vào như viết những file tùy chọn khác. Vậy thôi, nó cũng nên chưa các nhóm của các tùy chọn, mỗi nhóm đứng trước một dòng `[_`group`_]` chỉ ra chương trình nào mà các tùy chọn áp dụng.

Khi một file được thêm vào đang được thực thi, chỉ có các tùy chọn trong nhóm đó mà chương trình hiện tại mới được tìm để sử dụng. Các nhóm khác bị bỏ qua. Giả sử rằng một file `my.cnf` chứa dòng sau:

        
    !include /home/mydir/myopt.cnf

Và giả sử rằng `/home/mydir/myopt.cnf` sẽ như thế này:

    
    [mysqladmin]
    force
    
    [mysqld]
    key_buffer_size=16M

Nếu `my.cnf` được thực thi bởi **[mysqld**][1], chỉ có nhóm `[mysqld]` trong `/home/mydir/myopt.cnf` được sử dụng. Nếu file được thực thi bởi **[mysqladmin**][7], chỉ có nhóm `[mysqladmin]` được sử dụng. Nếu file được thực thi bởi bất kì chương trình nào khác, không một tùy chọn nào trong `/home/mydir/myopt.cnf` được sử dụng.

Chỉ thị `!includedir` được thực thi tương đương ngoại trừ tất cả các tùy chọn file trong thư mục có thên đươc đọc.
 
Nếu một tùy chọn file chứa chỉ thị `!include` hoặc `!includedir`, các file có tên nằm trong chỉ thị đều được thực thi bất kể khi nào file được chọn xử lí, bất chấp chúng xuất hiện ở chỗ nào trong file.

[1]: https://dev.mysql.com/mysqld.html "4.3.1 mysqld — The MySQL Server"
[2]: https://dev.mysql.com/server-options.html#option_mysqld_verbose
[3]: https://dev.mysql.com/server-options.html#option_mysqld_help
[4]: https://dev.mysql.com/mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility"
[5]: https://dev.mysql.com/option-file-options.html#option_general_login-path
[6]: https://dev.mysql.com/mysql.html "4.5.1 mysql — The MySQL Command-Line Tool"
[7]: https://dev.mysql.com/mysqladmin.html "4.5.2 mysqladmin — Client for Administering a MySQL Server"
[8]: https://dev.mysql.com/option-file-options.html#option_general_defaults-extra-file
[9]: https://dev.mysql.com/mysql-installer.html "2.3.3 MySQL Installer for Windows"
[10]: https://dev.mysql.com/source-configuration-options.html#option_cmake_sysconfdir
[11]: https://dev.mysql.com/mysqld-safe.html "4.3.2 mysqld_safe — MySQL Server Startup Script"
[12]: https://dev.mysql.com/server-options.html#option_mysqld_datadir
[13]: https://dev.mysql.com/server-options.html#option_mysqld_user
[14]: https://dev.mysql.com/command-line-options.html "4.2.4 Using Options on the Command Line"
[15]: https://dev.mysql.com/string-literals.html "9.1.1 String Literals"
[16]: https://dev.mysql.com/mysql-options.html "27.8.7.50 mysql_options()"
[17]: https://dev.mysql.com/mysqldump.html "4.5.4 mysqldump — A Database Backup Program"
[18]: https://dev.mysql.com/server-system-variables.html#sysvar_sql_mode

