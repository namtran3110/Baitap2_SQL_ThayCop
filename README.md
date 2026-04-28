# Bài kiểm tra số 2
## Thông tin sinh viên
+ **Họ và tên: Trần Nhất Nam**
+ **MSSV: K235480106001**
+ **Lớp: K59KMT**
+ **Trường Đại Học Kỹ Thuật Công Nghiệp**
---
## PHẦN 1: Thiết kế và Khởi tạo Cấu trúc Dữ liệu
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
___
- **Các khóa chính (PK):**
  - Bảng [TheLoaiSach]: Khóa chính là cột [MaTheLoai]
  - Bảng [DocGia]: Khóa chính là cột [MaDocGia]
  - Bảng [Sach]: Khóa chính là cột [MaSach]
  - Bảng [QLMuonTra]: Khóa chính là cột [MaGiaoDich]
--> Tất cả các khóa chính đều được gắn thuộc tính IDENTITY(1,1) để hệ thống tự   động cấp phát và tăng dần mã số (1, 2, 3...). Điều này giúp người dùng không cần nhập tay mã ID, loại bỏ hoàn toàn rủi ro trùng lặp hoặc thiếu sót dữ liệu.

- **Khóa ngoại (FK):**
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

soạn lại code và chụp lại ảnh, ảnh này ảnh cũ


- Để phù hợp với bài tập mô phỏng, logic tính tiền phạt và cấm mượn sách của thư viện sẽ là:
  - Thời hạn mượn: 30 ngày kể từ ngày mượn.
  - Tiền phạt cơ bản: 5.000đ/ngày, bắt đầu tính sau khi hết 30 ngày mượn.
  - Trần tiền phạt: Tiền phạt không được vượt quá giá tiền cuốn sách (GiaBan).
  - Phụ phí vi phạm: Nếu tiền phạt đã bằng giá sách mà vẫn chưa trả, cộng thêm 50% giá trị cuốn sách vào tổng tiền phạt.
  - Cấm mượn: Nếu trễ quá 30 ngày tính phạt (tổng cộng 60 ngày cầm sách), độc giả sẽ bị cấm mượn 6 tháng.
  - Biên bản: Nếu tổng tiền trong suốt quá trình mượn >300k thì bị biên bản cảnh cáo lần 1, nếu còn vi phạm (tổng tiền >600k) thì bị biên bản lần 2 và bị cấm mượn trong 1 năm

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d3a6ce50-b787-4005-9bd6-d5dc533aac5a" />
[ Ảnh 16: Truy xuất và tính thử tiền phạt SV ID 02 - Phạm Thị Thu Trà ]

---
## PHẦN 3: Xây dựng Store Procedure





















