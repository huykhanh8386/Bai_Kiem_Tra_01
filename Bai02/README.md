<img width="1535" height="862" alt="Screenshot 2026-10-02 135931" src="https://github.com/user-attachments/assets/54ea394c-afd8-4562-945a-a90dcc9ac724" />
<img width="1535" height="862" alt="Screenshot 2026-10-02 140308" src="https://github.com/user-attachments/assets/a12c38c5-e6e7-49c4-bc33-b8ead457e847" />
<img width="1535" height="862" alt="Screenshot 2026-10-02 140257" src="https://github.com/user-attachments/assets/19f1d646-c6c4-465a-8e2a-dce8dfcc0209" />
<img width="1535" height="862" alt="Screenshot 2026-10-02 140242" src="https://github.com/user-attachments/assets/effa6234-5f7c-4efb-9f0b-757423c06ea9" />
<img width="1535" height="862" alt="Screenshot 2026-10-02 140235" src="https://github.com/user-attachments/assets/09e67b71-137a-4ffd-829c-bfd4a18a23da" />


//Phương tiện
using System;

namespace AutoSpeedOOP
{
    public abstract class PhuongTien
    {
        private string _maPT;
        private string _tenHang;
        private int _namSanXuat;
        private decimal _giaGoc;
        public string MaPT
        {
            get { return _maPT; }
            set
            {
                if (string.IsNullOrWhiteSpace(value))
                    _maPT = "PT000";
                else
                    _maPT = value.Trim();
            }
        }
        public string TenHang
        {
            get { return _tenHang; }
            set
            {
                if (string.IsNullOrWhiteSpace(value))
                    throw new ArgumentException("Tên hãng không được để trống!");
                _tenHang = value.Trim();
            }
        }
        public int NamSanXuat
        {
            get { return _namSanXuat; }
            set
            {
                if (value < 1900 || value > DateTime.Now.Year)
                    throw new ArgumentException("Năm sản xuất không hợp lệ!");
                _namSanXuat = value;
            }
        }
        public decimal GiaGoc
        {
            get { return _giaGoc; }
            set
            {
                if (value <= 0)
                    throw new ArgumentException("Giá gốc phải lớn hơn 0!");
                _giaGoc = value;
            }
        }
        public PhuongTien(string maPT,string tenHang, int namSanXuat, decimal giaGoc)
        {
            MaPT = maPT;
            TenHang = tenHang;
            NamSanXuat = namSanXuat;
            GiaGoc = giaGoc;
        }
        public abstract decimal TinhGiaLanBanh();
        public virtual string GetInfo()
        {
            return $"Mã PT: {MaPT} | " + $"Hãng: {TenHang} | " + $"Năm SX: {NamSanXuat} | " + $"Giá gốc: {GiaGoc:N0} VNĐ";
        }
    }
}

// Ô tô
using System;
namespace AutoSpeedOOP
{
    public class OTo : PhuongTien
    {
        private int _soChoNgoi;
        private double _dungTichDongCo;
        public int SoChoNgoi
        {
            get { return _soChoNgoi; }
            set
            {
                if (value <= 0)
                    throw new ArgumentException("Số chỗ ngồi phải lớn hơn 0!");
                _soChoNgoi = value;
            }
        }
        public double DungTichDongCo
        {
            get { return _dungTichDongCo; }
            set
            {
                if (value <= 0)
                    throw new ArgumentException("Dung tích động cơ phải lớn hơn 0!");
                _dungTichDongCo = value;
            }
        }
        public OTo(string maPT, string tenHang, int namSanXuat, decimal giaGoc, int soChoNgoi, double dungTichDongCo): base(maPT, tenHang, namSanXuat, giaGoc)
        {
            SoChoNgoi = soChoNgoi;
            DungTichDongCo = dungTichDongCo;
        }
        public override decimal TinhGiaLanBanh()
        {
            if (SoChoNgoi <= 9)
            {
                decimal lePhiTruocBa = GiaGoc * 0.12m;
                decimal thueTieuThuDacBiet = GiaGoc * 0.30m;
                return GiaGoc + lePhiTruocBa+ thueTieuThuDacBiet;
            }
            else
            {
                decimal lePhiTruocBa = GiaGoc * 0.10m;

                return GiaGoc + lePhiTruocBa;
            }
        }

        public override string GetInfo()
        {
            return base.GetInfo() + $" | Số chỗ: {SoChoNgoi}" + $" | Động cơ: {DungTichDongCo}L";
        }
    }
}

// xe máy
using System;

namespace AutoSpeedOOP
{
    public class XeMay : PhuongTien
    {
        private int _dungTichXylanh;
        public int DungTichXylanh
        {
            get { return _dungTichXylanh; }
            set
            {
                if (value <= 0)
                    throw new ArgumentException("Dung tích xy lanh phải lớn hơn 0!");
                _dungTichXylanh = value;
            }
        }
        public XeMay(string maPT,string tenHang, int namSanXuat, decimal giaGoc, int dungTichXylanh): base(maPT, tenHang, namSanXuat, giaGoc)
        {
            DungTichXylanh = dungTichXylanh;
        }
        public override decimal TinhGiaLanBanh()
        {
            if (DungTichXylanh < 175)
            {
                return GiaGoc + GiaGoc * 0.02m;
            }
            else
            {
                return GiaGoc + GiaGoc * 0.05m;
            }
        }
        public override string GetInfo()
        {
            return base.GetInfo() + $" | Dung tích xy lanh: {DungTichXylanh}cc";
        }
    }
}
// Quản lý phương tiện
using System;
using System.Collections.Generic;
using System.Linq;

namespace AutoSpeedOOP
{
    public class QuanLyPhuongTien
    {
        private List<PhuongTien> danhSach;
        public QuanLyPhuongTien()
        {
            danhSach = new List<PhuongTien>();
        }
        public void AddPhuongTien(PhuongTien pt)
        {
            danhSach.Add(pt);
        }
        public void DisplayAll()
        {
            if (danhSach.Count == 0)
            {
                Console.WriteLine("Danh sách phương tiện đang trống!");
                return;
            }
            Console.WriteLine("\n================ DANH SÁCH PHƯƠNG TIỆN ================");
            foreach (PhuongTien pt in danhSach)
            {
                Console.WriteLine(pt.GetInfo());
                Console.WriteLine($"Giá lăn bánh: {pt.TinhGiaLanBanh():N0} VNĐ");
            }
        }
        public PhuongTien? FindMaxGiaLanBanh()
        {
            if (danhSach.Count == 0)
                return null;
            PhuongTien max = danhSach[0];
            foreach (PhuongTien pt in danhSach)
            {
                if (pt.TinhGiaLanBanh() > max.TinhGiaLanBanh())
                {
                    max = pt;
                }
            }
            return max;
        }
        public List<PhuongTien> SearchByName(string keyword)
        {
            return danhSach.Where(pt => pt.TenHang.Contains(keyword,StringComparison.OrdinalIgnoreCase)).ToList();
        }
    }
}

//main
using System;
using System.Collections.Generic;
using System.Text;

namespace AutoSpeedOOP
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.OutputEncoding = Encoding.UTF8;
            Console.InputEncoding = Encoding.UTF8;
            QuanLyPhuongTien ql = new QuanLyPhuongTien();
            int chon;
            do
            {
                Console.WriteLine();
                Console.WriteLine("==============================================");
                Console.WriteLine("       QUẢN LÝ PHƯƠNG TIỆN AUTOSPEED");
                Console.WriteLine("==============================================");
                Console.WriteLine("1. Thêm ô tô");
                Console.WriteLine("2. Thêm xe máy");
                Console.WriteLine("3. Hiển thị danh sách phương tiện");
                Console.WriteLine("4. Tìm phương tiện có giá lăn bánh cao nhất");
                Console.WriteLine("5. Tìm kiếm phương tiện theo tên hãng");
                Console.WriteLine("0. Thoát");
                Console.WriteLine("==============================================");
                chon = NhapSoNguyen("Nhập lựa chọn: ");
                switch (chon)
                {
                    case 1:
                        NhapOTo(ql);
                        break;
                    case 2:
                        NhapXeMay(ql);
                        break;
                    case 3:
                        ql.DisplayAll();
                        break;
                    case 4:
                        TimGiaLanBanhMax(ql);
                        break;
                    case 5:
                        TimTheoTenHang(ql);
                        break;

                    case 0:
                        Console.WriteLine("\nĐã thoát chương trình!");
                        break;
                    default:
                        Console.WriteLine("\nLựa chọn không hợp lệ!");
                        break;
                }
            } while (chon != 0);
        }
        static void NhapOTo(QuanLyPhuongTien ql)
        {
            Console.WriteLine();
            Console.WriteLine("============= NHẬP Ô TÔ =============");
            try
            {
                Console.Write("Mã phương tiện: ");
                string maPT = Console.ReadLine() ?? "";
                Console.Write("Tên hãng: ");
                string tenHang = Console.ReadLine() ?? "";
                int namSanXuat = NhapSoNguyen("Năm sản xuất: ");
                decimal giaGoc = NhapDecimal("Giá gốc: ");
                int soChoNgoi = NhapSoNguyen("Số chỗ ngồi: ");
                double dungTichDongCo = NhapDouble("Dung tích động cơ (L): ");
                OTo oto = new OTo(maPT, tenHang, namSanXuat, giaGoc, soChoNgoi, dungTichDongCo);
                ql.AddPhuongTien(oto);
                Console.WriteLine("\nThêm ô tô thành công!");
                Console.WriteLine(oto.GetInfo());
                Console.WriteLine($"Giá lăn bánh: {oto.TinhGiaLanBanh():N0} VNĐ");
            }
            catch (ArgumentException ex)
            {
                Console.WriteLine("\nLỗi: " + ex.Message);
            }
        }
        static void NhapXeMay(QuanLyPhuongTien ql)
        {
            Console.WriteLine();
            Console.WriteLine("============= NHẬP XE MÁY =============");
            try
            {
                Console.Write("Mã phương tiện: ");
                string maPT = Console.ReadLine() ?? "";
                Console.Write("Tên hãng: ");
                string tenHang = Console.ReadLine() ?? "";
                int namSanXuat = NhapSoNguyen("Năm sản xuất: ");
                decimal giaGoc =  NhapDecimal("Giá gốc: ");
                int dungTichXylanh = NhapSoNguyen("Dung tích xy lanh (cc): ");
                XeMay xeMay = new XeMay( maPT, tenHang, namSanXuat, giaGoc,dungTichXylanh);
                ql.AddPhuongTien(xeMay);
                Console.WriteLine("\nThêm xe máy thành công!");
                Console.WriteLine(xeMay.GetInfo());
                Console.WriteLine($"Giá lăn bánh: {xeMay.TinhGiaLanBanh():N0} VNĐ");
            }
            catch (ArgumentException ex)
            {
                Console.WriteLine("\nLỗi: " + ex.Message);
            }
        }
        static void TimGiaLanBanhMax(
            QuanLyPhuongTien ql)
        {
            Console.WriteLine();
            Console.WriteLine("===== PHƯƠNG TIỆN GIÁ LĂN BÁNH CAO NHẤT =====");
            PhuongTien? max = ql.FindMaxGiaLanBanh();
            if (max == null)
            {
                Console.WriteLine("Danh sách phương tiện đang trống!");
                return;
            }
            Console.WriteLine(max.GetInfo());
            Console.WriteLine($"Giá lăn bánh: {max.TinhGiaLanBanh():N0} VNĐ");
        }
        static void TimTheoTenHang(QuanLyPhuongTien ql)
        {
            Console.WriteLine();
            Console.Write("Nhập tên hãng cần tìm: ");
            string keyword =Console.ReadLine() ?? "";
            List<PhuongTien> ketQua =ql.SearchByName(keyword);
            if (ketQua.Count == 0)
            {
                Console.WriteLine("Không tìm thấy phương tiện!");
                return;
            }
            Console.WriteLine();
            Console.WriteLine("============= KẾT QUẢ TÌM KIẾM =============");
            foreach (PhuongTien pt in ketQua)
            {
                Console.WriteLine(pt.GetInfo());
                Console.WriteLine( $"Giá lăn bánh: {pt.TinhGiaLanBanh():N0} VNĐ");
            }
        }
        static int NhapSoNguyen(string thongBao)
        {
            int n;
            while (true)
            {
                Console.Write(thongBao);
                if (int.TryParse(Console.ReadLine(), out n))
                {
                    return n;
                }
                Console.WriteLine(
                    "Dữ liệu không hợp lệ, hãy nhập số nguyên!");
            }
        }
        static decimal NhapDecimal(
            string thongBao)
        {
            decimal n;
            while (true)
            {
                Console.Write(thongBao);
                if (decimal.TryParse( Console.ReadLine(), out n))
                {
                    return n;
                }
                Console.WriteLine( "Dữ liệu không hợp lệ, hãy nhập lại!");
            }
        }
        static double NhapDouble(
            string thongBao)
        {
            double n;
            while (true)
            {
                Console.Write(thongBao);
                if (double.TryParse(
                    Console.ReadLine(), out n))
                {
                    return n;
                }
                Console.WriteLine("Dữ liệu không hợp lệ, hãy nhập lại!");
            }
        }
    }
}
