-- Create the database
CREATE DATABASE DuLieu;
GO

USE DuLieu;
GO

CREATE TABLE TaiKhoan (
    ID_TaiKhoan INT PRIMARY KEY IDENTITY(1,1),
    TenDangNhap NVARCHAR(50) NOT NULL,
    MatKhau NVARCHAR(100) NOT NULL,
    Email NVARCHAR(100),
    HoTen NVARCHAR(100),
    NgaySinh DATE,
    GioiTinh NVARCHAR(10),
    SDT NVARCHAR(20),
    DiaChi NVARCHAR(200)
);

--  Admin table
CREATE TABLE Admin (
    ID_Admin INT PRIMARY KEY IDENTITY(1,1),
    ChiTiet NVARCHAR(MAX),
    ID_TaiKhoan INT,
    FOREIGN KEY (ID_TaiKhoan) REFERENCES TaiKhoan(ID_TaiKhoan)
);

-- BenhNhan table
CREATE TABLE BenhNhan (
    ID_BenhNhan INT PRIMARY KEY IDENTITY(1,1),
    TienSuBenh NVARCHAR(MAX),
    DiUng NVARCHAR(MAX),
    ID_TaiKhoan INT,
    FOREIGN KEY (ID_TaiKhoan) REFERENCES TaiKhoan(ID_TaiKhoan)
);

--  BangCap table
CREATE TABLE BangCap (
    ID_BangCap INT PRIMARY KEY IDENTITY(1,1),
    TenBangCap NVARCHAR(100),
    MoTa NVARCHAR(MAX),
    HeSoLuong DECIMAL(5,2)
);

-- ChuyenKhoa table
CREATE TABLE ChuyenKhoa (
    ID_ChuyenKhoa INT PRIMARY KEY IDENTITY(1,1),
    TenChuyenKhoa NVARCHAR(100),
    MoTa NVARCHAR(MAX)
);

-- BacSi table
CREATE TABLE BacSi (
    ID_BacSi INT PRIMARY KEY IDENTITY(1,1),
    MoTa NVARCHAR(MAX),
    ID_BangCap INT,
    ID_TaiKhoan INT,
    ID_ChuyenKhoa INT,
    FOREIGN KEY (ID_BangCap) REFERENCES BangCap(ID_BangCap),
    FOREIGN KEY (ID_TaiKhoan) REFERENCES TaiKhoan(ID_TaiKhoan),
    FOREIGN KEY (ID_ChuyenKhoa) REFERENCES ChuyenKhoa(ID_ChuyenKhoa)
);

-- LichLamViec table
CREATE TABLE LichLamViec (
    ID_LichLamViec INT PRIMARY KEY IDENTITY(1,1),
    NgayLamViec DATE,
    ThoiGianBatDau TIME,
    ThoiGianKetThuc TIME,
    TrangThai NVARCHAR(50),
    ID_BacSi INT,
    FOREIGN KEY (ID_BacSi) REFERENCES BacSi(ID_BacSi)
);

-- LichHen table
CREATE TABLE LichHen (
    ID_LichHen INT PRIMARY KEY IDENTITY(1,1),
    NgayGioHen DATETIME,
    TrieuChung NVARCHAR(MAX),
    GhiChu NVARCHAR(MAX),
    TrangThai NVARCHAR(50),
    ID_BenhNhan INT,
    ID_BacSi INT,
    FOREIGN KEY (ID_BenhNhan) REFERENCES BenhNhan(ID_BenhNhan),
    FOREIGN KEY (ID_BacSi) REFERENCES BacSi(ID_BacSi)
);

-- HoSoBenhAn table
CREATE TABLE HoSoBenhAn (
    ID_HoSoBenhAn INT PRIMARY KEY IDENTITY(1,1),
    NgayKham DATE,
    ChuanDoan NVARCHAR(MAX),
    HuongDieuTri NVARCHAR(MAX),
    GhiChu NVARCHAR(MAX),
    KetQuaXetNghiem BIT,
    ID_BenhNhan INT,
    FOREIGN KEY (ID_BenhNhan) REFERENCES BenhNhan(ID_BenhNhan)
);
    ALTER TABLE HoSoBenhAn
    ALTER COLUMN KetQuaXetNghiem BIT;
    
-- ChiSoXetNghiem table
CREATE TABLE ChiSoXetNghiem (
    ID_ChiSoXetNghiem INT PRIMARY KEY IDENTITY(1,1),
    TenChiSo NVARCHAR(100),
    DonViDo NVARCHAR(50),
    MucBinhThuong NVARCHAR(100)
);

-- KetQuaXetNghiem table
CREATE TABLE KetQuaXetNghiem (
    ID_KetQuaXetNghiem INT PRIMARY KEY IDENTITY(1,1),
    GiaTri NVARCHAR(100),
    GhiChu NVARCHAR(MAX),
    ID_BenhNhan INT,
    ID_ChiSoXetNghiem INT,
    FOREIGN KEY (ID_BenhNhan) REFERENCES BenhNhan(ID_BenhNhan),
    FOREIGN KEY (ID_ChiSoXetNghiem) REFERENCES ChiSoXetNghiem(ID_ChiSoXetNghiem)
);

-- DichVu table
CREATE TABLE DichVu (
    ID_DichVu INT PRIMARY KEY IDENTITY(1,1),
    TenDichVu NVARCHAR(100),
    MoTa NVARCHAR(MAX),
    Gia DECIMAL(10,2),
    ID_ChuyenKhoa INT,
    FOREIGN KEY (ID_ChuyenKhoa) REFERENCES ChuyenKhoa(ID_ChuyenKhoa)
);

--  GoiKham table
CREATE TABLE GoiKham (
    ID_GoiKham INT PRIMARY KEY IDENTITY(1,1),
    TenGoiKham NVARCHAR(100),
    MoTa NVARCHAR(MAX),
    Gia DECIMAL(10,2),
    NgayCapNhat DATE
);

-- ChiTietGoiKham table
CREATE TABLE ChiTietGoiKham (
    ID_GoiKham INT,
    ID_DichVu INT,
    PRIMARY KEY (ID_GoiKham, ID_DichVu),
    FOREIGN KEY (ID_GoiKham) REFERENCES GoiKham(ID_GoiKham),
    FOREIGN KEY (ID_DichVu) REFERENCES DichVu(ID_DichVu)
);

-- Create HoaDon table
CREATE TABLE HoaDon (
    ID_HoaDon INT PRIMARY KEY IDENTITY(1,1),
    NgayTao DATE,
    TongTien DECIMAL(10,2),
    TrangThaiThanhToan NVARCHAR(50),
    ID_HoSoBenhAn INT,
    FOREIGN KEY (ID_HoSoBenhAn) REFERENCES HoSoBenhAn(ID_HoSoBenhAn)
);

-- ChiTietHoaDon table
CREATE TABLE ChiTietHoaDon (
    ID_ChiTietHoaDon INT PRIMARY KEY IDENTITY(1,1),
    ID_HoaDon INT,
    ID_DichVu INT,
    FOREIGN KEY (ID_HoaDon) REFERENCES HoaDon(ID_HoaDon),
    FOREIGN KEY (ID_DichVu) REFERENCES DichVu(ID_DichVu)
);

-- DonThuoc table
CREATE TABLE DonThuoc (
    ID_DonThuoc INT PRIMARY KEY IDENTITY(1,1),
    NgayKe DATE,
    ID_HoSoBenhAn INT,
    FOREIGN KEY (ID_HoSoBenhAn) REFERENCES HoSoBenhAn(ID_HoSoBenhAn)
);

-- Thuoc table
CREATE TABLE Thuoc (
    ID_Thuoc INT PRIMARY KEY IDENTITY(1,1),
    TenThuoc NVARCHAR(100)
);

-- ChiTietDonThuoc table
CREATE TABLE ChiTietDonThuoc (
    ID_DonThuoc INT,
    ID_Thuoc INT,
    SoLuong INT,
    DonVi NVARCHAR(50),
    CachDung NVARCHAR(MAX),
    PRIMARY KEY (ID_DonThuoc, ID_Thuoc),
    FOREIGN KEY (ID_DonThuoc) REFERENCES DonThuoc(ID_DonThuoc),
    FOREIGN KEY (ID_Thuoc) REFERENCES Thuoc(ID_Thuoc)
);

-- TaiKhoan
INSERT INTO TaiKhoan (TenDangNhap, MatKhau, Email, HoTen, NgaySinh, GioiTinh, SDT, DiaChi)
VALUES 
('admin1', 'password123', 'admin1@hospital.com', N'Nguyễn Văn Admin', '1980-01-01', N'Nam', '0123456789', N'Hà Nội'),
('bacsi1', 'doctor123', 'bacsi1@hospital.com', N'Trần Thị Bác Sĩ', '1985-05-15', N'Nữ', '0987654321', N'Hồ Chí Minh'),
('bacsi2', 'doctor456', 'bacsi2@hospital.com', N'Phạm Văn Khoa', '1982-03-25', N'Nam', '0754321698', N'Hải Phòng'),
('benhnhan1', 'patient123', 'benhnhan1@gmail.com', N'Lê Văn Bệnh', '1990-10-20', N'Nam', '0369852147', N'Đà Nẵng'),
('benhnhan2', 'patient456', 'benhnhan2@gmail.com', N'Hoàng Thị Khỏe', '1988-12-05', N'Nữ', '0321654987', N'Cần Thơ');

-- Admin
INSERT INTO Admin (ChiTiet, ID_TaiKhoan)
VALUES 
(N'Quản trị viên cấp cao', 1),
(N'Quản trị viên hệ thống', 1),
(N'Quản trị viên dữ liệu', 1),
(N'Quản trị viên bảo mật', 1),
(N'Quản trị viên mạng', 1);

-- BenhNhan
INSERT INTO BenhNhan (TienSuBenh, DiUng, ID_TaiKhoan)
VALUES 
(N'Không có', N'Không có', 4),
(N'Tiểu đường', N'Penicillin', 5),
(N'Huyết áp cao', N'Hải sản', 4),
(N'Hen suyễn', N'Bụi nhà', 5),
(N'Không có', N'Lactose', 4);

-- BangCap
INSERT INTO BangCap (TenBangCap, MoTa, HeSoLuong)
VALUES 
(N'Bác sĩ đa khoa', N'Tốt nghiệp đại học Y', 1.0),
(N'Thạc sĩ Y học', N'Tốt nghiệp cao học Y', 1.2),
(N'Tiến sĩ Y học', N'Có bằng tiến sĩ y khoa', 1.5),
(N'Bác sĩ chuyên khoa I', N'Chuyên khoa cấp I', 1.3),
(N'Bác sĩ chuyên khoa II', N'Chuyên khoa cấp II', 1.4);

-- ChuyenKhoa
INSERT INTO ChuyenKhoa (TenChuyenKhoa, MoTa)
VALUES 
(N'Nội khoa', N'Chẩn đoán và điều trị các bệnh nội khoa'),
(N'Ngoại khoa', N'Phẫu thuật và điều trị ngoại khoa'),
(N'Sản phụ khoa', N'Chăm sóc sức khỏe phụ nữ và thai sản'),
(N'Nhi khoa', N'Chăm sóc sức khỏe trẻ em'),
(N'Tim mạch', N'Chẩn đoán và điều trị bệnh tim mạch');

-- BacSi
INSERT INTO BacSi (MoTa, ID_BangCap, ID_TaiKhoan, ID_ChuyenKhoa)
VALUES 
(N'Bác sĩ nội khoa với 10 năm kinh nghiệm', 1, 2, 1),
(N'Bác sĩ phẫu thuật tim', 3, 3, 5),
(N'Bác sĩ sản khoa', 2, 2, 3),
(N'Bác sĩ nhi khoa', 4, 3, 4),
(N'Bác sĩ chuyên khoa tim mạch', 5, 2, 5);
-- LichLamViec
INSERT INTO LichLamViec (NgayLamViec, ThoiGianBatDau, ThoiGianKetThuc, TrangThai, ID_BacSi)
VALUES 
('2024-09-06', '08:00', '17:00', N'Đã lên lịch', 1),
('2024-09-06', '08:00', '17:00', N'Đã lên lịch', 2),
('2024-09-07', '08:00', '12:00', N'Đã lên lịch', 3),
('2024-09-07', '13:00', '17:00', N'Đã lên lịch', 4),
('2024-09-08', '08:00', '17:00', N'Đã lên lịch', 5);

-- Continue LichHen inserts
INSERT INTO LichHen (NgayGioHen, TrieuChung, GhiChu, TrangThai, ID_BenhNhan, ID_BacSi)
VALUES 
('2024-09-07 14:00', N'Ho kéo dài', N'Đã uống thuốc không đỡ', N'Đã xác nhận', 4, 4),
('2024-09-08 10:30', N'Khó thở', N'Tiền sử bệnh tim', N'Chờ khám', 5, 5);

-- HoSoBenhAn
INSERT INTO HoSoBenhAn (NgayKham, ChuanDoan, HuongDieuTri, GhiChu, KetQuaXetNghiem, ID_BenhNhan)
VALUES 
('2024-09-06', N'Viêm xoang', N'Uống thuốc và nghỉ ngơi', N'Tái khám sau 1 tuần', 0, 1),
('2024-09-06', N'Viêm dạ dày', N'Điều trị nội khoa', N'Cần theo dõi', 1, 2),
('2024-09-07', N'Cúm mùa', N'Uống thuốc và nghỉ ngơi', N'Cách ly tại nhà', 1, 3),
('2024-09-07', N'Viêm phế quản', N'Kháng sinh và thuốc ho', N'Tái khám sau 5 ngày', 0, 4),
('2024-09-08', N'Rối loạn nhịp tim', N'Thuốc điều hòa nhịp tim', N'Theo dõi sát', 1, 5);
-- ChiSoXetNghiem
INSERT INTO ChiSoXetNghiem (TenChiSo, DonViDo, MucBinhThuong)
VALUES 
(N'Huyết sắc tố (Hemoglobin)', 'g/dL', '12-16'),
(N'Đường huyết lúc đói', 'mg/dL', '70-100'),
(N'Cholesterol toàn phần', 'mg/dL', '<200'),
(N'AST (SGOT)', 'U/L', '10-40'),
(N'Creatinine', 'mg/dL', '0.6-1.2');

-- KetQuaXetNghiem
INSERT INTO KetQuaXetNghiem (GiaTri, GhiChu, ID_BenhNhan, ID_ChiSoXetNghiem)
VALUES 
('14.5', N'Bình thường', 1, 1),
('95', N'Bình thường', 2, 2),
('220', N'Cao, cần chế độ ăn và vận động phù hợp', 3, 3),
('35', N'Bình thường', 4, 4),
('1.0', N'Bình thường', 5, 5);

-- DichVu
INSERT INTO DichVu (TenDichVu, MoTa, Gia, ID_ChuyenKhoa)
VALUES 
(N'Khám tổng quát', N'Khám sức khỏe tổng quát định kỳ', 500000, 1),
(N'Chụp X-quang', N'Chụp X-quang ngực', 200000, 2),
(N'Siêu âm thai', N'Siêu âm thai định kỳ', 300000, 3),
(N'Tiêm vắc-xin', N'Tiêm vắc-xin cho trẻ em', 100000, 4),
(N'Đo điện tim', N'Đo và phân tích điện tim', 250000, 5);

-- GoiKham
INSERT INTO GoiKham (TenGoiKham, MoTa, Gia, NgayCapNhat)
VALUES 
(N'Gói khám sức khỏe cơ bản', N'Bao gồm khám tổng quát và xét nghiệm cơ bản', 1000000, '2024-01-01'),
(N'Gói khám tim mạch', N'Khám chuyên sâu về tim mạch', 2000000, '2024-01-01'),
(N'Gói khám thai sản', N'Khám thai định kỳ', 1500000, '2024-01-01'),
(N'Gói khám nhi', N'Khám tổng quát cho trẻ em', 800000, '2024-01-01'),
(N'Gói tầm soát ung thư', N'Các xét nghiệm tầm soát ung thư cơ bản', 3000000, '2024-01-01');

-- ChiTietGoiKham
INSERT INTO ChiTietGoiKham (ID_GoiKham, ID_DichVu)
VALUES 
(1, 1), (1, 2),
(2, 1), (2, 5),
(3, 1), (3, 3),
(4, 1), (4, 4),
(5, 1), (5, 2);

-- HoaDon
INSERT INTO HoaDon (NgayTao, TongTien, TrangThaiThanhToan, ID_HoSoBenhAn)
VALUES 
('2024-09-06', 700000, N'Đã thanh toán', 1),
('2024-09-06', 800000, N'Chưa thanh toán', 2),
('2024-09-07', 400000, N'Đã thanh toán', 3),
('2024-09-07', 450000, N'Đã thanh toán', 4),
('2024-09-08', 2250000, N'Chưa thanh toán', 5);

-- ChiTietHoaDon
INSERT INTO ChiTietHoaDon (ID_HoaDon, ID_DichVu)
VALUES 
(1, 1), (1, 2),
(2, 1), (2, 5),
(3, 1), (3, 4),
(4, 1), (4, 2),
(5, 1), (5, 2), (5, 5);

-- DonThuoc
INSERT INTO DonThuoc (NgayKe, ID_HoSoBenhAn)
VALUES 
('2024-09-06', 1),
('2024-09-06', 2),
('2024-09-07', 3),
('2024-09-07', 4),
('2024-09-08', 5);

-- Thuoc
INSERT INTO Thuoc (TenThuoc)
VALUES 
(N'Paracetamol'),
(N'Amoxicillin'),
(N'Omeprazole'),
(N'Salbutamol'),
(N'Metoprolol');

-- ChiTietDonThuoc
INSERT INTO ChiTietDonThuoc (ID_DonThuoc, ID_Thuoc, SoLuong, DonVi, CachDung)
VALUES 
(1, 1, 20, N'Viên', N'Uống 1 viên khi sốt hoặc đau, cách 6 tiếng/lần'),
(2, 3, 14, N'Viên', N'Uống 1 viên trước bữa sáng'),
(3, 2, 21, N'Viên', N'Uống 1 viên sau bữa ăn, ngày 3 lần'),
(4, 4, 1, N'Ống', N'Xịt 2 nhát mỗi lần, khi khó thở'),
(5, 5, 30, N'Viên', N'Uống 1 viên sáng và tối');


-- Check
UPDATE HoSoBenhAn
SET KetQuaXetNghiem = 
    CASE 
        WHEN KetQuaXetNghiem = N'Không có' THEN 0
        ELSE 1
    END;

-- DELETE FROM HoSoBenhAn;

INSERT INTO HoSoBenhAn (NgayKham, ChuanDoan, HuongDieuTri, GhiChu, KetQuaXetNghiem, ID_BenhNhan)
VALUES 
('2024-09-06', N'Viêm xoang', N'Uống thuốc và nghỉ ngơi', N'Tái khám sau 1 tuần', 0, 1),
('2024-09-06', N'Viêm dạ dày', N'Điều trị nội khoa', N'Cần theo dõi', 1, 2),
('2024-09-07', N'Cúm mùa', N'Uống thuốc và nghỉ ngơi', N'Cách ly tại nhà', 1, 3),
('2024-09-07', N'Viêm phế quản', N'Kháng sinh và thuốc ho', N'Tái khám sau 5 ngày', 1, 4),
('2024-09-08', N'Rối loạn nhịp tim', N'Thuốc điều hòa nhịp tim', N'Theo dõi sát', 1, 5);
