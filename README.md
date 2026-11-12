# DuyBarberShop

**Website quản lý và đặt lịch cắt tóc trực tuyến** được xây dựng bằng **ASP.NET Core MVC (C#)** và **SQL Server**, phục vụ bài tập/đồ án ngành Công nghệ phần mềm.

## 1. Giới thiệu

DuyBarberShop hỗ trợ khách hàng xem dịch vụ và đặt lịch cắt tóc trực tuyến; nhân viên theo dõi lịch làm việc; quản trị viên quản lý hoạt động của tiệm. **Một lịch đặt có thể bao gồm nhiều dịch vụ.**

**Website triển khai tham khảo:** http://duybarbershop.somee.com (đường dẫn HTTP; khả năng truy cập phụ thuộc tình trạng hosting).

## 2. Chức năng chính

| Vai trò | Chức năng |
| --- | --- |
| Khách hàng | Đăng ký, đăng nhập, xem dịch vụ, đặt lịch với một hoặc nhiều dịch vụ, xem lịch sử đặt lịch, quản lý hồ sơ cá nhân. |
| Nhân viên | Đăng nhập, xem lịch làm việc và các lịch được phân công. |
| Quản trị viên (Admin) | Quản lý dịch vụ, nhân viên, tài khoản, lịch đặt và xem thống kê. |

Các thao tác cụ thể được giới hạn theo quyền của tài khoản đăng nhập.

## 3. Công nghệ sử dụng

- **Backend:** C#, ASP.NET Core MVC, **.NET 8**.
- **Cơ sở dữ liệu:** Microsoft SQL Server.
- **Truy cập dữ liệu:** Entity Framework Core 8 (SQL Server provider, Migrations).
- **Xác thực:** ASP.NET Core Cookie Authentication; mật khẩu được băm bằng `PasswordHasher<TaiKhoan>`.
- **Frontend:** Razor Views (`.cshtml`), HTML, CSS, JavaScript và các thư viện giao diện trong `wwwroot`.

## 4. Yêu cầu trước khi cài đặt

1. Windows và **Visual Studio 2022** có workload **ASP.NET and web development** (hoặc IDE hỗ trợ .NET 8).
2. **.NET 8 SDK**.
3. **SQL Server** (SQL Server Express/Developer đều có thể sử dụng) và công cụ quản lý như **SQL Server Management Studio (SSMS)** nếu cần.
4. Git (nếu muốn clone repository).

## 5. Tải mã nguồn

Clone repository:

```bash
git clone https://github.com/nguynduy353/DuyBarberShop.git
```

Mở thư mục `DuyBarberShop/SourceCode` và mở file `DuyBarberShop.slnx` bằng Visual Studio, hoặc mở trực tiếp `DuyBarberShop.csproj`.

> Nếu tải bằng **Code → Download ZIP** trên GitHub, hãy giải nén rồi mở dự án trong thư mục `SourceCode`.

## 6. Cấu hình cơ sở dữ liệu

### Bước 1: Tạo cấu hình kết nối SQL Server

Dự án đọc chuỗi kết nối tên **`DefaultConnection`** từ cấu hình ứng dụng. Nên dùng **User Secrets** khi chạy trên máy cá nhân để không đưa mật khẩu lên GitHub.

Trong Visual Studio, nhấp chuột phải vào project → **Manage User Secrets**, sau đó thêm nội dung ví dụ:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=DuyBarberShopDb;Trusted_Connection=True;TrustServerCertificate=True;"
  }
}
```

- Thay `localhost` bằng tên SQL Server instance của máy bạn (ví dụ `localhost\\SQLEXPRESS` nếu dùng SQL Server Express).
- Nếu sử dụng SQL Server Authentication, tự cấu hình `User Id` và `Password` trên máy cá nhân; **không commit thông tin đăng nhập**.
- Có thể cấu hình bằng `appsettings.Development.json` trên máy cá nhân, nhưng phải đảm bảo file chứa thông tin bí mật không được đẩy lên GitHub.

### Bước 2: Tạo cấu trúc bảng bằng Entity Framework Core Migrations

Trong Visual Studio, vào **Tools → NuGet Package Manager → Package Manager Console**, chọn project `DuyBarberShop` rồi chạy:

```powershell
Update-Database
```

Hoặc tại thư mục chứa `.csproj`, nếu đã cài công cụ `dotnet-ef`:

```bash
dotnet ef database update
```

Lệnh áp dụng các migration có sẵn trong thư mục `Migrations` để tạo/cập nhật cơ sở dữ liệu. Nếu báo lỗi kết nối, hãy kiểm tra SQL Server đã chạy và `DefaultConnection` đúng.

> **Lưu ý:** Migration tạo cấu trúc dữ liệu; không mặc định tạo sẵn tài khoản Admin hoặc dữ liệu mẫu. Việc cấp tài khoản quản trị cần thực hiện theo quy trình quản trị/cấp dữ liệu phù hợp của hệ thống.

## 7. Chạy hệ thống

**Cách 1 – Visual Studio**

1. Mở dự án từ thư mục `SourceCode`.
2. Chờ Visual Studio khôi phục các NuGet packages (Restore).
3. Kiểm tra kết nối SQL Server và áp dụng migration.
4. Chọn profile chạy **HTTPS** rồi nhấn **F5** (hoặc **Ctrl + F5**).
5. Mở địa chỉ `https://localhost:<port>` do Visual Studio hiển thị.

**Cách 2 – .NET CLI**

Tại thư mục chứa `DuyBarberShop.csproj`:

```bash
dotnet restore
dotnet run
```

Truy cập địa chỉ được in trong terminal. Nếu chạy HTTPS lần đầu và gặp cảnh báo chứng chỉ phát triển, có thể cần:

```bash
dotnet dev-certs https --trust
```

## 8. Hướng dẫn sử dụng

### Khách hàng

1. Mở website và xem danh sách dịch vụ.
2. Chọn **Đăng ký** để tạo tài khoản, sau đó **Đăng nhập**.
3. Vào chức năng **Đặt lịch**, chọn dịch vụ (có thể chọn nhiều dịch vụ trong một lịch), nhân viên và thời gian phù hợp theo giao diện.
4. Xác nhận đặt lịch và theo dõi tại phần **Lịch sử đặt lịch**.
5. Sử dụng **Hồ sơ cá nhân** để xem/cập nhật thông tin được hệ thống cho phép.

### Nhân viên

1. Đăng nhập bằng tài khoản được phân quyền nhân viên.
2. Mở phần **Lịch làm việc** để xem các lịch được phân công.

### Quản trị viên

1. Đăng nhập bằng tài khoản đã được cấp quyền **Admin**.
2. Truy cập các trang quản lý để cập nhật **dịch vụ**, **nhân viên**, **tài khoản** và **lịch đặt**.
3. Mở mục **Thống kê** để theo dõi tình hình hoạt động.

> Các nút và màn hình có thể thay đổi theo phiên bản mã nguồn; quyền truy cập phụ thuộc vai trò tài khoản.

## 9. Cấu trúc repository

```text
DuyBarberShop/
├── BaoCao/                   # Báo cáo tiến độ và tài liệu học phần
├── SourceCode/               # Mã nguồn ứng dụng
│   ├── Controllers/          # Xử lý yêu cầu MVC
│   ├── Data/                 # DbContext và cấu hình dữ liệu
│   ├── Migrations/           # Lịch sử migration EF Core
│   ├── Models/               # Các thực thể dữ liệu
│   ├── ViewModels/           # Mô hình dữ liệu cho giao diện
│   ├── Views/                # Razor Views
│   ├── wwwroot/              # CSS, JavaScript, hình ảnh, thư viện frontend
│   ├── Program.cs            # Cấu hình và khởi chạy ứng dụng
│   └── DuyBarberShop.csproj  # Cấu hình project .NET 8
└── README.md                 # Tài liệu này
```

## 10. Một số lỗi thường gặp

| Hiện tượng | Cách kiểm tra |
| --- | --- |
| Không kết nối được SQL Server | Kiểm tra SQL Server service, tên instance và chuỗi `DefaultConnection`. |
| Báo lỗi bảng chưa tồn tại | Kiểm tra đã chạy `Update-Database` và dùng đúng database. |
| Thiếu package khi build | Chạy `dotnet restore` hoặc Restore NuGet Packages trong Visual Studio. |
| Không mở được HTTPS localhost | Kiểm tra launch profile, chứng chỉ phát triển và thử `dotnet dev-certs https --trust`. |
| Không thấy chức năng quản trị | Kiểm tra tài khoản đã được phân quyền Admin hay chưa. |

## 11. Lưu ý bảo mật khi đăng mã nguồn

- **Không đưa lên GitHub** mật khẩu SQL Server, connection string có mật khẩu, API keys hoặc tài khoản thật.
- Không commit các thư mục build/tạm như `bin/`, `obj/`, `.vs/` và file `*.csproj.user`.
- Kiểm tra kỹ `appsettings.json` và các file cấu hình trước mỗi lần `git add` / `git push`.
- Với môi trường triển khai, nên dùng biến môi trường hoặc cơ chế quản lý bí mật của hosting.

## 12. Mục đích sử dụng

Dự án phục vụ học tập, thực hành phân tích – thiết kế – xây dựng – kiểm thử – triển khai hệ thống website đặt lịch và cung cấp dịch vụ trực tuyến.
