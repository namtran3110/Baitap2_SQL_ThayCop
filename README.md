# Bài kiểm tra số 2
## Thông tin sinh viên
+ **Họ và tên: Trần Nhất Nam**
+ **MSSV: K235480106001**
+ **Lớp: K59KMT**
+ **Trường Đại Học Kỹ Thuật Công Nghiệp**
---
## PHẦN 1: THIẾT KẾ VÀ KHỞI TẠO CẤU TRÚC DỮ LIỆU
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a487dbc1-a16c-42cd-8b4b-d0a262f8047a" />
[ Ảnh 1: Khởi tạo Database với tên QuanLyThuVien_K235480106001 ]  

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/17e4fdad-23f1-4527-b274-14784a48e0de" />
[ Ảnh 2: Trỏ vào Database và tạo bảng TheLoaiSach ] 

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/98902129-1e17-4dc7-8bc9-52a3c139e35c" />
[ Ảnh 3: Tạo bảng DocGia ] 

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/42ba3f6f-93f1-404a-b049-f461eb68f543" />
[ Ảnh 4: Tạo bảng Sach ]

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b5bfa352-e863-4659-ba3f-04a681e78037" />
[ Ảnh 5: Tạo bảng QLMuonTra ]

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7832141a-2665-4d33-9d1c-771a9178ecdb" />
[ Ảnh 6: Code tạo các bảng ]

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/edf8444b-3922-4491-b317-63600c0d4a6a" />
[ Ảnh 7: Bảng các trường và ràng buộc khóa ]

---
- **CÁC KHÓA CHÍNH (PK):**
  - Bảng [TheLoaiSach]: Khóa chính là cột [MaTheLoai]
  - Bảng [DocGia]: Khóa chính là cột [MaDocGia]
  - Bảng [Sach]: Khóa chính là cột [MaSach]
  - Bảng [QLMuonTra]: Khóa chính là cột [MaGiaoDich]

--> Tất cả các khóa chính đều được gắn thuộc tính IDENTITY(1,1) để hệ thống tự   động cấp phát và tăng dần mã số (1, 2, 3...). Điều này giúp người dùng không cần nhập tay mã ID, loại bỏ hoàn toàn rủi ro trùng lặp hoặc thiếu sót dữ liệu.

- **KHÓA NGOẠI (FK):**
  - FK tại bảng [Sach]: Cột [MaTheLoai] chiếu tới PK của bảng [TheLoaiSach].
    --> Nó   bắt buộc mỗi cuốn sách khi nhập kho phải thuộc về một danh mục thể loại đã có sẵn.
  - FK thứ nhất tại bảng [QLMuonTra]: Cột [MaDocGia] chiếu tới PK của bảng [DocGia]. 
    --> Nó đảm bảo không thể tạo phiếu mượn cho một mã thẻ độc giả không tồn tại.
  - FK thứ hai tại bảng [QLMuonTra]: Cột [MaSach] chiếu tới PK của bảng [Sach]. 
    --> Nó ngăn chặn việc thủ thư cho mượn một cuốn sách không có trong kho.

- **Ràng buộc kiểm tra (CK):** 
  - Bảng [Sach]: Trường [NamXuatBan] chỉ chấp nhận giá trị trong khoảng từ 1900-2026.
  - Bảng [DocGia]: Trường [TienCoc] bắt buộc phải >= 0 (không được âm).
  - Bảng [QLMuonTra]: Trường [TienPhat] bắt buộc phải >= 0 (không được âm).
---
## PHẦN 2: Xây dựng Function 
### YÊU CẦU 1: Hãy cho biết trong SQL Server có những loại function build_in (hàm có sẵn) nào, nêu 1 vài system function build_in mà em tìm hiểu được (ko cần nhiều, cần đặc sắc theo góc nhìn của em), cho SQL khai thác các hàm đó.

**Trong SQL Server, Built-in Function được chia làm 4 nhóm chính**
  - Hàm vô hướng (Scalar Functions): Trả về một giá trị duy nhất (Hàm toán học, chuỗi, ngày tháng).
  - Hàm tập hợp (Aggregate Functions): Tính toán trên một tập dữ liệu (SUM, AVG, COUNT, MAX, MIN).
  - Hàm xếp hạng (Ranking Functions): Dùng trong phân tích dữ liệu (ROW_NUMBER, RANK).
  - Hàm logic: Các hàm kiểm tra điều kiện (IIF, CHOOSE).
  
**Hai hàm Built-in mà em cảm thấy hay và hữu dụng để dùng trong bài quản lý thư viện**
- 1.Hàm DATEDIFF()
  - Ý nghĩa: dùng để đo lường khoảng cách giữa hai mốc thời gian dựa trên một đơn vị chọn trước (ngày, giờ, tháng, năm...)
  - Cấu trúc: DATEDIFF(đơn_vị, ngày_bắt_đầu, ngày_kết_thúc)
--> Quản lý thư viện thực chất là quản lý thời hạn. Nếu không có hàm này, người cán bộ thư viện sẽ rất vất vả để tính xem độc giả đã quá hạn bao lâu. Thay vì phải ngồi cộng trừ ngày tháng phức tạp, DATEDIFF xử lý mọi thứ chỉ trong một dòng lệnh, bất chấp năm nhuận hay tháng có 28, 30 hay 31 ngày.
- 2.Hàm STRING_AGG()
  - Ý nghĩa: dùng để gộp các giá trị từ nhiều dòng dữ liệu khác nhau thành một chuỗi văn bản duy nhất, ngăn cách bởi một ký tự đã chọn.
  + Cấu trúc: STRING_AGG(tên_cột_cần_gộp, 'ký_tự_ngăn_cách')
--> Bình thường, nếu một độc giả mượn 3 cuốn sách, khi ta SELECT, SQL sẽ trả về 3 dòng lặp lại tên độc giả đó. Điều này làm báo cáo rất dài và khó xem. STRING_AGG sẽ làm cho tên độc giả chỉ hiện 1 dòng, và toàn bộ 3 cuốn sách sẽ nằm gọn gàng trong 1 ô, cách nhau bởi dấu phẩy.
---
### YÊU CẦU 2: Hàm do người dùng tự viết trong SQL thường mang mục đích gì? Nó có những loại nào? Mỗi loại thường được dùng khi nào? Tại sao có nhiều system function rồi mà vẫn cần tự viết fn riêng?

**HÀM DO NGƯỜI DÙNG TỰ VIẾT**
**PHÂN LOẠI LÀM 3 LOẠI CHÍNH:**  
- 1.Hàm vô hướng (Scalar Function): Hàm trả về duy nhất 1 giá trị
  - --> Dùng khi cần tính toán một giá trị cụ thể từ các tham số đầu vào. Ví dụ: Tính tổng tiền phạt của một độc giả dựa trên số ngày trễ.
- 2.Hàm giá trị bảng Inline (Inline TVF): Trả về một bảng dữ liệu thông qua duy nhất một câu lệnh SELECT.
  - --> Dùng khi cần lọc dữ liệu nhanh theo tham số mà không có logic phức tạp. Hiệu năng của loại này rất cao vì SQL Server coi nó như một View động.
- 3.Hàm giá trị bảng Multi-statement (MSTVF): Trả về một bảng dữ liệu nhưng có cấu trúc phức tạp, sử dụng biến bảng.
  - --> Dùng khi cần thực hiện nhiều bước xử lý: kiểm tra điều kiện IF-ELSE, dùng vòng lặp, hoặc kết hợp dữ liệu từ nhiều nguồn trước khi trả về kết quả cuối cùng.
**Mục đích chính của hàm người dùng tự viết**  
- Có 3 mục đích chính cốt lõi:
  - Tái sử dụng mã nguồn: Thay vì viết đi viết lại một công thức tính toán phức tạp ở nhiều câu truy vấn khác nhau, ta đóng gói nó vào một hàm và gọi tên hàm đó khi cần.
  - Tính đóng gói: Giấu đi các logic xử lý phức tạp bên trong hàm, giúp câu lệnh SQL chính trở nên ngắn gọn, dễ đọc và dễ bảo trì hơn.
  - Định nghĩa logic nghiệp vụ riêng: Hệ thống không thể biết quy trình tính điểm thưởng hay tiền phạt của từng thư viện cụ thể, UDF chính là nơi để ta hiện thực hóa các quy tắc đó.

**TẠI SAO CÓ SẴN SYSTEM FUNCTION RỒI VẪN PHẢI TỰ VIẾT?**
- Nguyên nhân:
  - Tính chuyên biệt: Các hàm System functions sẵn có đơn thuần phục vụ các tác vụ chung mà mọi hệ thống đều cần. Ngược lại, hàm UDF được viết để giải quyết các quy tắc nghiệp vụ riêng của từng dự án
  - Khả năng tùy biến sâu: Các hàm System functions là 1 hộp đen mà ta không thể thay đổi cách nó chạy. Hàm UDF thì ngược lại, ta có toàn quyền kiểm soát logic xử lý bên trong để phù hợp với yêu cầu thay đổi người sử dụng hoặc khách hàng.
  - Gọn gàng và sạch chương trình: UDF giúp ẩn đi sự phức tạp của các phép tính lồng nhau, chuyển đổi chúng thành một tên hàm gợi nhớ. Điều này giúp các câu lệnh SQL chính trở nên ngắn gọn, minh bạch và dễ dàng kiểm soát lỗi hơn.

--- 
### YÊU CẦU 3: Viết 01 Scalar Function (Hàm trả về một giá trị): Đưa ra 1 logic cho cơ sở dữ liệu của em, mà cần dùng đến function này. (SV TỰ NGHĨ RA YÊU CẦU CỦA HÀM VÀ VIẾT HÀM GIẢI QUYẾT NÓ). Sau khi đã có hàm, viết câu lệnh sql khai thác hàm đó.

**TÌNH HUỐNG LOGIC ĐẶT RA KHI QUẢN LÝ THƯ VIỆN TRÊN THỰC TẾ**  
- Trong thực tế, thư viện luôn có quy định giới hạn số lượng sách được mượn cùng lúc của một độc giả, ví dụ: mỗi người chỉ được mượn tối đa 3 cuốn. Để thủ thư kiểm tra nhanh xem một độc giả hiện tại đang giữ bao nhiêu cuốn sách (những sách đã mượn nhưng chưa mang trả)
--> Để dễ dàng quản lý hơn khi sử dụng phần mềm SQL Server, ta cần tạo 1 hàm vô hướng (Scalar Function).
Yêu cầu của hàm:
  - Đầu vào là Mã độc giả (@MaDocGia).
  - Đầu ra là một con số nguyên (INT) đếm tổng số lượt mượn mà trường [NgayTraThucTe] đang bị bỏ trống (NULL).

**XÂY DỰNG CHƯƠNG TRÌNH TRONG SQL SERVER**
- **1. NẠP DỮ LIỆU ĐẦU VÀO**
<img width="1920" height="1078" alt="image" src="https://github.com/user-attachments/assets/6684563c-22a8-496f-8236-29cf9f071917" />
[ Ảnh 8: Khởi tạo dữ liệu cho 2 bảng TheLoai và DocGia ]

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e94d086b-525e-4147-aa2b-42dbfe8abbd9" />
[ Ảnh 9: Nạp thông tin các quyển sách ]

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e56195fa-df38-492d-a258-90e8ee871d26" />
[ Ảnh 10: Nạp dữ liệu cho bảng QLMuonTra ]  

- **2. XÂY DỰNG SCALAR FUNCTION THỎA MÃN LOGIC THỰC TẾ**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/27b70b0d-2df6-466f-8514-4c1df752c239" />
[ Ảnh 10: Chương trình hàm vô hướng với tên là "fn_DemSoSachDangMuon" ]

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0a25fed3-87c1-4f25-afb4-b144f94b3437" />
[ Ảnh 11: Kết quả trả về của Scalar function ]  

- Khi chạy câu lệnh SELECT khai thác hàm fn_DemSoSachDangMuon, kết quả trả về hoàn toàn khớp với logic dữ liệu đã nạp ở Bảng [QLMuonTra]. Cụ thể, hàm chỉ đếm những giao dịch có NgayTraThucTe IS NULL

--> Kết luận: Hàm chạy hoàn toàn chính xác, giúp xác định được độc giả đang giữ bao nhiêu sách, giúp thủ thư quản lý và đưa ra quyết định có cho mượn thêm sách hay không.

---
### YÊU CẦU 4: Viết 01 Inline Table-Valued Function: Trả về danh sách các bản ghi theo một điều kiện lọc cụ thể (SV TỰ NGHĨ RA YÊU CẦU CỦA HÀM VÀ VIẾT HÀM GIẢI QUYẾT NÓ). Sau khi đã có hàm, viết câu lệnh sql khai thác hàm đó.


**TÌNH HUỐNG LOGIC THỰC TẾ**  
- Khi một sinh viên đến gặp thủ thư để thắc mắc về lịch sử mượn sách, thủ thư cần xem nhanh toàn bộ danh sách các cuốn sách mà sinh viên đó đã từng mượn từ trước đến nay, bao gồm cả ngày mượn và ngày trả thực tế.
--> Để thuận tiện cho việc truy soát trên SQL thay vì phải kiểm tra thủ công lịch sử mượn từng ngày, ta thiết kế 1 Inline Table-Valued Function để giải quyết vấn đề nhanh gọn. 
- Yêu cầu của hàm: Đầu vào là Mã độc giả (@MaDocGia). Đầu ra là một bảng gồm các cột: Tên sách, Ngày mượn, Ngày trả dự kiến, Ngày trả thực tế.
  
**XÂY DỰNG VÀ KIỂM NGHIỆM HÀM**  
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d28c2ae8-bff6-421e-98b5-ea6083b79dca" />
[ Ảnh 12: Chương trình của Inline Table-Valued Function với tên là "fn_XemLichSuMuon" ] 

- Có 2 hình thức để truy xuất lịch sử tùy theo thủ thư:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d0002af0-96a0-4139-9367-d99411518106" />
[ Ảnh 13: Thủ thư truy xuất đến lịch sử của duy nhất SV id 1 - Trần Tuấn Anh ]

  - Cơ chế: Khi truyền MaDocGia = 1, SQL sẽ lọc trong bảng QLMuonTra tất cả các dòng của sinh viên đó, kết nối sang bảng Sach để lấy tên sách tương ứng và ném ra một bảng kết quả tạm thời.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/360dff63-9c9d-4b7e-99a8-009d6dd252d3" />
[ Ảnh 14: Thủ thư truy xuất toàn bộ lịch sử mượn/trả của thư viện ]

---
### YÊU CẦU 5: Viết 01 Multi-statement Table-Valued Function: Thực hiện xử lý logic phức tạp bên trong (có sử dụng biến bảng) trước khi trả về kết quả. (SV TỰ NGHĨ RA YÊU CẦU CỦA HÀM VÀ VIẾT HÀM GIẢI QUYẾT NÓ). Sau khi đã có hàm, viết câu lệnh sql khai thác hàm đó.

**TÌNH HUỐNG LOGIC THỰC TẾ**  
- Một sinh viên đến hỏi: "Em còn nợ những sách gì và nếu bây giờ em trả thì em phải nộp bao nhiêu tiền phạt?". Vấn đề là dữ liệu trong bảng [QLMuonTra] chỉ có: Ngày mượn, Ngày trả dự kiến. Nó không có sẵn cột "Tiền phạt hiện tại" vì tiền phạt thay đổi theo từng ngày (nếu trả hôm nay là 5k, nhưng sang ngày có thể đã lên 10k).
--> Cần xây dựng 1 Multi-statement Table-Valued Function để thuận tiện cho việc kiểm tra sách nào quá hạn và tính số tiền phạt tăng lên theo từng ngày.

**XÂY DỰNG VÀ KIỂM NGHIỆM HÀM**  
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/92ea0036-19f3-40d8-a56d-0592c0115742" />
[ Ảnh 15: Chương trình của Multi-statement Table-Valued Function với tên là fn_BaoCaoChiTietCongNo ]

- Để phù hợp với bài tập mô phỏng, logic tính tiền phạt và cấm mượn sách của thư viện sẽ là:
  - Thời hạn mượn: 30 ngày kể từ ngày mượn.
  - Tiền phạt cơ bản: 5.000đ/ngày, bắt đầu tính sau khi hết 30 ngày mượn.
  - Trần tiền phạt: Tiền phạt không được vượt quá giá tiền cuốn sách (GiaBan).
  - Phụ phí vi phạm: Nếu tiền phạt đã bằng giá sách mà vẫn chưa trả, cộng thêm 50% giá trị cuốn sách vào tổng tiền phạt (mục đích là tránh để bảo quản những cuốn đắt tiền luôn được trả sớm).
  - Cấm mượn: Nếu trễ quá 30 ngày tính phạt (tổng cộng 60 ngày cầm sách), độc giả sẽ bị cấm mượn 6 tháng.
  - Biên bản cảnh cáo: Nếu tổng tiền trong suốt quá trình mượn >300k thì bị biên bản cảnh cáo lần 1, nếu còn vi phạm (tổng tiền >600k) thì bị biên bản lần 2 và bị cấm mượn trong 1 năm

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d3a6ce50-b787-4005-9bd6-d5dc533aac5a" />
[ Ảnh 16: Truy xuất và tính thử tiền phạt SV ID 02 - Phạm Thị Thu Trà ]

---
## PHẦN 3: Xây dựng Store Procedure
### YÊU CẦU 1: Trong SQL Server có những SP có sẵn nào? nêu 1 vài system sp mà em tìm hiểu được, giải thích cách dùng chúng.

**MỘT VÀI Store Procedure CÓ SẴN TRONG SQL VÀ CÁCH SỬ DỤNG**  
- sp_help:
  - Dùng để cung cấp thông tin chi tiết về một đối tượng (bảng, view, index...). Nó cho biết bảng có những cột nào, kiểu dữ liệu gì.
  - Cú pháp: EXEC sp_help 'Sach';
- sp_helptext:
  - Dùng để hiển thị mã nguồn của các đối tượng không bị mã hóa như: Function, Stored Procedure, hoặc View. Rất hữu ích khi cần xem lại logic đã viết.
  - Cú pháp: EXEC sp_helptext 'fn_BaoCaoTongHopViPham';
- sp_rename:
  - Dùng để đổi tên một đối tượng (bảng hoặc cột) trong cơ sở dữ liệu hiện tại mà không cần phải xóa đi và tạo lại từ đầu.
  - Cú pháp: EXEC sp_rename 'Old_Table_Name', 'New_Table_Name';
- sp_databases:
  - Dùng để liệt kê danh sách tất cả các cơ sở dữ liệu đang tồn tại trên SQL Server instance đang kết nối.
  - Cú pháp: EXEC sp_databases;
- sp_who:
  - Dùng để kiểm tra các phiên làm việc, người dùng và các tiến trình đang chạy trên SQL Server. Giúp xác định ai đang thực thi lệnh và kiểm tra tình trạng nghẽn hệ thống.
  - Cú pháp: EXEC sp_who;
---

### YÊU CẦU 2: Viết 01 Store Procedure đơn giản để thực hiện lệnh INSERT hoặc UPDATE dữ liệu, có kiểm tra điều kiện logic (SV TỰ NGHĨ RA YÊU CẦU CỦA SP VÀ VIẾT SP GIẢI QUYẾT NÓ)

**YÊU CẦU CỦA SP**
- Nhằm tối ưu hóa trải nghiệm đọc, thư viện liên tục cập nhật và bổ sung các ấn phẩm mới nhất trên thị trường. Tuy nhiên, thách thức đặt ra là sự bùng nổ về số lượng đầu sách cùng sự biến động liên tục của giá cả theo thời gian, đòi hỏi một cơ chế quản lý dữ liệu linh hoạt và chính xác.
--> Cần phải sử dụng SP để tăng tốc độ xử lý nghiệp vụ này của thư viện.

**XÂY DỰNG Store Procedure**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9e5f9ace-74a9-4625-b2af-373c7adb5368" />
[ Ảnh 17: Chương trình Store Procedure với tên sp_CapNhatThongTinSach ]

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c46e1cd4-ba2d-475d-be98-402b227ed236" />
[ Ảnh 18: Thêm 1 sách mới với mã loại 2-CNTT ]

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9b324f81-4614-4e83-95b0-42a664274ee5" />
[ Ảnh 19: Cập nhật giá cho 1 sách đã có từ trước ]

- Store Procedure hoạt động hoàn hảo, giúp chèn, cập nhật và sửa đổi các dữ liệu đã có và dữ liệu mới thay cho các câu lệnh insert, update của sql

---
### YÊU CẦU 3: Viết 01 Store Procedure có sử dụng tham số OUTPUT để trả về một giá trị tính toán (SV TỰ NGHĨ RA YÊU CẦU CỦA SP VÀ VIẾT SP GIẢI QUYẾT NÓ, SP NÀY CÓ DÙNG THAM SỐ LOẠI OUTPUT)

**YÊU CẦU CỦA SP**  
- Trong quản lý thư viện, việc thống kê là cực kỳ quan trọng. Chúng ta sẽ viết một SP để thống kê tổng số đầu sách và tổng giá trị tiền sách của một thể loại cụ thể.

**XÂY DỰNG Store Procedure**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/df618476-868a-417d-a968-7365ba786fc1" />
[ Ảnh 20: Chương trình với tên là sp_ThongKeSachTheoLoai ]

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4846832a-ee87-44b1-8a19-433792b318bf" />
[ Ảnh 21: Thống kê tổng số sách và giá trị các cách loại 2-CNTT ]

Các thành phần và vai trò:  
- Thủ thư:  Người ra lệnh và cung cấp "thùng chứa" (Biến OUTPUT) để nhận kết quả.
- SQL Engine: Bộ máy xử lý trung gian, nhận lệnh, biên dịch và điều phối việc truy xuất dữ liệu.
- Database: Nơi lưu trữ bảng Sach. SQL sẽ truy cập vào đây để quét dữ liệu thực tế.
- Tham số OUTPUT: Đóng vai trò là "cổng giao tiếp ngược", cho phép dữ liệu đi từ bên trong Database ra ngoài môi trường người dùng mà không cần tạo tập kết quả cồng kềnh.

---
### YÊU CẦU 4: Viết 01 Store Procedure trả về một tập kết quả (Result set) từ lệnh SELECT sau khi đã join nhiều bảng. (SV TỰ NGHĨ RA YÊU CẦU CỦA SP VÀ VIẾT SP GIẢI QUYẾT NÓ)

**Ý TƯỞNG**  
- Thiết lập thủ tục sp_BaoCaoChiTietMuonTra. Thủ tục này thực hiện kết nối 4 bảng: DocGia, QLMuonTra, Sach, và TheLoaiSach để thay thế các mã ID khô khan bằng thông tin định danh chi tiết. Kết quả trả về là một danh sách trực quan phục vụ công tác đối soát và in ấn báo cáo tại thư viện.

**XÂY DỰNG Store Procedure**  
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/562a3090-bdcb-40f1-9482-c4eded39b9b5" />
[ Ảnh 22: Chương trình Store Procedure trả về một tập kết quả với tên là sp_BaoCaoChiTietMuonTra ]

- Kết quả thực thi thủ tục sp_BaoCaoChiTietMuonTra cho thấy tập kết quả đã hiển thị đầy đủ thông tin định danh thay vì các mã ID số học. Thông qua các phép liên kết INNER JOIN, hệ thống đã truy xuất dữ liệu từ bảng QLMuonTra, đối chiếu với bảng DocGia để lấy tên, bảng Sach để lấy tên sách và bảng TheLoaiSach để xác định thể loại. Điều này chứng minh cấu trúc cơ sở dữ liệu và các mối quan hệ đã được thiết lập đúng đắn và hoạt động ổn định.

---
## Phần 4: Trigger và Xử lý logic nghiệp vụ (Kiến thức 11)
### YÊU CẦU 1: Viết 01 Trigger để tự động làm gì đó tại 1 bảng B khi mà dữ liệu thay đổi dữ liệu ở bảng A. Logic giải quyết do sv tự nghĩ ra, sao cho thực tế và thuyết phục.

**Ý TƯỞNG**
- Bảng A: Sach.
- Bảng B: LichSuGiaSach.
- Logic: Mỗi khi thủ thư cập nhật giá bán của một cuốn sách ở bảng Sach, Trigger sẽ tự động ghi lại một dòng vào bảng LichSuGiaSach bao gồm: Mã sách, giá cũ, giá mới và thời gian thay đổi.
- --> Mục đích chính là giúp thủ thư kiểm soát được việc tăng/giảm giá sách bất thường, tránh thất thoát tài chính hoặc sai sót trong nhập liệu.
 
**XÂY DỰNG TRIGGER**
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/82029d2b-c127-4e1b-9f9d-e217b1bacf08" />
[ Ảnh 23: Khởi tạo bảng LichSuGiaSach (bảng B) ]

- Bảng LichSuGiaSach sẽ được sử dụng để lưu mã sách, giá cũ, giá mới và thời gian thay đổi khi thủ thư chỉnh sửa.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/89147e63-b5ca-4206-99b0-29162e935fde" />
[ Ảnh 24: Viết trigger lưu lịch sử với tên là trg_LuuLichSuGiaSach ]

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5dfc0e2b-3e95-4b81-8170-d0a73fb426dd" />
[ Ảnh 25: Cập nhật sách mã số 1 lên 99k ]

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/dccc6c50-c568-4752-b2e7-b17e9957fae9" />
[ Ảnh 26: Giá cuốn ID 1 đã được sửa thành 99k ]

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e591de2f-ba59-4896-97a8-c12e126d9f58" />
[ Ảnh 27: Trigger đã thực hiện ghi lại lịch sử thay đổi cuốn sách ID 1 từ 48k lên 99k ]

---
### YÊU CẦU 2: Thử viết Trigger cho Bảng A : Khi insert thì cập nhật dữ liệu vào bảng B; sau đó viết trigger cho bảng B để khi B được cập nhật thì cập nhật sang bảng A : Quan sát các thông báo (nếu có) của hệ thống, giải thích các thông báo đó (nếu có). Đưa ra nhật xét cuối cùng về tình trạng này.

**XÂY DỰNG CHƯƠNG TRÌNH**

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/37019ecb-1ae7-4517-9e4b-0e89ae744a27" />
[ Ảnh 28: Thêm 1 cột ghi chú vào bảng TheLoaiSach để tiện quan sát ]

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ec20b0f8-be84-4717-865e-b34cb8283f6f" />
[ Ảnh 29: Viết trigger cho bảng A ]
- Trigger này có nhiệm vụ khi ta insert dữ liệu vào bảng A thì chèn thông tin và bảng B.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8a159c41-497e-4165-bc27-308376fbf32d" />
[ Ảnh 30: Viết trigger cho bảng B ]
- Nhiệm vụ của trigger này là khi thông tin ở bảng B đã được chèn thì cập nhật lại ghi chú ở bảng A.

<img width="1920" height="1078" alt="image" src="https://github.com/user-attachments/assets/41e9d74f-78a7-472a-a31a-65f3b169a1c2" />
[ Ảnh 31: Insert 1 cuốn sách mới vào bảng A ]
- Trigger lồng ở trên chỉ mới chạy ở 2 cấp theo thứ tự A->B->A, hệ thống hoạt động hoàn toàn bình thường mà không báo lỗi.

## Phần 5: Cursor và Duyệt dữ liệu (Kiến thức 11) 

---
### YÊU CẦU 1: Viết một đoạn script sử dụng CURSOR để duyệt qua danh sách của 1 câu lệnh SQL dạng SELECT, duyệt qua từng bản ghi, xử lý riêng từng bản ghi (THEO LOGIC SV TỰ ĐẶT RA: SAO CHO HỢP LÝ VÀ THUYẾT PHỤC) 

**Ý TƯỞNG BAN ĐẦU**
- Do biến động thị trường, thư viện cần cập nhật lại giá bán của toàn bộ đầu sách hiện có. Tuy nhiên, mỗi thể loại sẽ có mức tăng khác nhau:
  - Thể loại 2 (Công nghệ thông tin): Tăng 5% (do sách kỹ thuật nhanh lỗi thời).
  - Thể loại 4 (Ngoại ngữ): Tăng 10% (do chi phí bản quyền cao).
  - Các thể loại còn lại: Tăng 2% đồng loạt.
  - Yêu cầu: Sau khi tính toán, in ra thông báo chi tiết cho từng cuốn sách để thủ thư đối soát.

**XÂY DỰNG CHƯƠNG TRÌNH SỬ DỤNG CURSOR**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ff7c0e7a-3295-491d-bbcc-2765a773357e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2bc6eb70-6244-4c7d-a119-6f3fc920d0ae" />
[ Ảnh 32+33: Đoạn script sử dụng cursor để tăng toàn bộ giá sách trong thư viện ]

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8245cc12-45af-44fd-bc80-d6adb8fce741" />
[ Ảnh 34: Kết quả sau khi tăng giá sách hàng loạt bằng đoạn script ]

- Giải thích cơ chế hoạt động:
  - Cơ chế duyệt: Cursor hoạt động như một con trỏ chạy dọc theo tập kết quả của câu lệnh SELECT. Thay vì xử lý cả bảng cùng một lúc, nó "gắp" từng dòng dữ liệu và nạp vào các biến @MaSach, @MaTheLoai... mà chúng ta đã khai báo.
  - Biến @@FETCH_STATUS: Đây là biến hệ thống quan trọng. Nó trả về 0 nếu việc lấy dữ liệu thành công và khác 0 khi đã duyệt hết danh sách hoặc gặp lỗi. Đây là "chìa khóa" để điều khiển vòng lặp WHILE.
  - Tính thuyết phục của Logic: Cursor cho phép thực hiện các phép toán phức tạp (như IF...ELSE phân tầng) trên từng dòng mà một câu lệnh UPDATE đơn giản khó thực hiện được một cách tường minh. Việc in thông báo (Print) trong vòng lặp giúp thủ thư theo dõi được tiến trình xử lý theo thời gian thực (real-time auditing).

---
### YÊU CẦU 2: Tìm cách không sử dụng CURSOR để giải quyết bài toán mà em đã dùng CURSOR mới giải quyết được ở trên. thử so sánh tốc độ giữa có dùng cursor và không dùng cursor (nếu cùng kết quả) thì thời gian xử lý cái nào nhanh hơn, cần ảnh chụp màn hình minh chứng.

- Thay vì sử dụng cursor như ở yêu cầu trên, tại yêu cầu này em sẽ dùng Set-based Update để làm thực hiện công việc tương tự và so sánh tốc độ của 2 cách:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b3a7c249-9055-467e-88dd-5af2723cd12f" />
[ Ảnh 35: Cập nhật giá sách sử dụng lệnh update kết hơp biểu thức case-when ]

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c3dab1b6-1dec-40b3-a212-370ec511bdd3" />
[ Ảnh  : Tốc độ xử lý của lệnh update ]

<img width="1920" height="1078" alt="image" src="https://github.com/user-attachments/assets/df896621-2240-4ce7-bd6e-64d0131a9fa1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/18752437-d74b-42c4-b4a9-57c5e0f9c6d0" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8a8984d0-6fa9-414e-bc52-de80c5b9bf2e" />
[ Ảnh : Tốc độ xử lý của đoạn script sử dụng cursor ]

**SO SÁNH 2 HƯỚNG XỬ LÝ BÀI TOÁN SỬA GIÁ SÁCH THƯ VIỆN**
- 



---
### YÊU CẦU 3: Nếu vẫn tìm được cách dùng SQL để giải quyết vấn đề mà ko cần CURSOR: thử nghĩ bài toán khác, mà chỉ CURSOR mới giải quyết được, còn SQL rất khó giải quyết đc (theo logic suy nghĩ của em)




