import ipywidgets as widgets
from IPython.display import display, clear_output
from datetime import datetime

# Danh sách học sinh
danh_sach_hs = {"DH3564H": "Sùng A Chiu", "HUE567B": "Lờ A Cáng", "UHU789B": "Giàng A Sinh"}

def luu_thong_tin(noi_dung):
    with open("nhat_ky_truong.txt", "a", encoding="utf-8") as f:
        f.write(f"{datetime.now().strftime('%d/%m %H:%M')} - {noi_dung}\n")

out = widgets.Output()

def trang_chu(ten_hs):
    with out:
        clear_output()
        tab = widgets.Tab()
        # Mục Điểm danh
        btn_dd = widgets.Button(description=f"Điểm danh: {ten_hs}", button_style='success')
        btn_dd.on_click(lambda b: [luu_thong_tin(f"{ten_hs} điểm danh"), print(f"✅ Đã lưu điểm danh cho {ten_hs}")])
        # Mục TKB
        tkb = widgets.HTML(f"<b>Học sinh: {ten_hs}</b><br><table border='1' style='width:100%'><tr><th>T2</th><th>T3</th><th>T4</th><th>T5</th><th>T6</th></tr><tr><td>Toán</td><td>Văn</td><td>Anh</td><td>Lý</td><td>Hóa</td></tr></table>")
        # Mục Viết đơn
        ly_do = widgets.Textarea(placeholder='Nhập lý do nghỉ học tại đây...')
        btn_gui = widgets.Button(description="Gửi đơn xin nghỉ", button_style='warning')
        btn_gui.on_click(lambda b: [luu_thong_tin(f"ĐƠN {ten_hs}: {ly_do.value}"), print(f"✅ Đã gửi đơn của {ten_hs}"), setattr(ly_do, 'value', '')])
        
        tab.children = [btn_dd, tkb, widgets.VBox([widgets.Label(f"Người viết đơn: {ten_hs}"), ly_do, btn_gui])]
        tab.set_title(0, 'Điểm danh'); tab.set_title(1, 'TKB'); tab.set_title(2, 'Viết đơn')
        display(tab)

ma_hs = widgets.Text(description='Mã HS:', placeholder='Nhập mã của bạn...')
btn_in = widgets.Button(description="Đăng nhập", button_style='info')

def login(b):
    ma = ma_hs.value.upper().strip()
    if ma in danh_sach_hs:
        trang_chu(danh_sach_hs[ma])
    else:
        with out: clear_output(); print("❌ Sai mã rồi!")

btn_in.on_click(login)
display(widgets.HTML("<h2>THPT MÙ CANG CHẢI</h2>"), ma_hs, btn_in, out)
