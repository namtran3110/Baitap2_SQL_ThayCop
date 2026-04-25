# Bài kiểm tra số 2
## Thông tin sinh viên
+ **Họ và tên: Trần Nhất Nam**
+ **MSSV: K235480106001**
+ **Lớp: K59KMT**
+ **Trường Đại Học Kỹ Thuật Công Nghiệp**
---
## Phần 1: Thiết kế và Khởi tạo Cấu trúc Dữ liệu
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a487dbc1-a16c-42cd-8b4b-d0a262f8047a" />
Ảnh 1: Khởi tạo Database với tên QuanLyThuVien_K235480106001

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
## Phần 2: Xây dựng Function 
- Trong SQL Server, Built-in Function được chia làm 4 nhóm chính
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
  - Các hàm hệ thống sẵn có đơn thuần phục vụ các tác vụ chung mà mọi hệ thống đều cần. Ngược lại, hàm UDF được viết để giải quyết  các quy tắc nghiệp vụ riêng của từng dự án






















