# write_up_picoCTF_forensics_advanced-potion-making.
# Đề: Ron just found his own copy of advanced potion making, but its been corrupted by some kind of spell. Help him recover it!(Ron vừa tìm thấy bản sao chế tạo thuốc nâng cao của riêng mình, nhưng nó đã bị hỏng bởi một loại bùa chú nào đó. Giúp anh ta phục hồi nó!)
# url:https://artifacts.picoctf.net/picoMini+by+redpwn/Forensics/advanced-potion-making/advanced-potion-making
# hint:(none).
# --------let's go----------------- #
~ Đầu tiên ta bấm vào *url và được một file: advanced-potion-making.Ta dùng lệnh file kiểm tra advanced-potion-making cho ra info là data, như thói quen ta dùng lệnh strings advanced-potion-making, thì không thu được manh mối gì về flag.
~ Tiếp đến ta dùng xxd advanced-potion-making | head để kiểm tra 10 đoạn mã hex đầu tiên của file thì ta được:" 
00000000: 8950 4211 0d0a 1a0a 0012 1314 4948 4452  .PB.........IHDR
00000010: 0000 0990 0000 04d8 0802 0000 0004 2de7  ..............-.
00000020: 7800 0000 0173 5247 4200 aece 1ce9 0000  x....sRGB.......
00000030: 0004 6741 4d41 0000 b18f 0bfc 6105 0000  ..gAMA......a...
00000040: 0009 7048 5973 0000 1625 0000 1625 0149  ..pHYs...%...%.I
00000050: 5224 f000 0076 3949 4441 5478 5eec fd61  R$...v9IDATx^..a
00000060: 72e3 4c94 a659 ce16 6afe 76cd fe57 d7dd  r.L..Y..j.v..W..
00000070: 5b18 45e9 4b8a 7a28 d19d 2048 07a9 6376  [.E.K.z(.. H..cv
00000080: ac2d 2b3e bfaf 5f07 1801 82d7 b2f3 fff3  .-+>.._.........
00000090: fffc 7fff 7f00 0000 0000 0000 4b18 5802  ............K.X.
"
nhìn vào 4 byte đầu tiên và đối chiếu qua đoạn mã Ascii thì ta nghi ngờ đến đoạn mã hex này đã bị thay được dẫn đến nó bị hỏng (search:89 50 42 11 trên google thì tìm thấy được được mã như sau tương tự 89 50 4E 47 0D 0A 1A 0A -> đây là mã hex của phần mở rộng hình ảnh png->ta sẽ đổi mã hex lại và thêm đuôi mở rộng là png)-> khi downloads về thì ảnh vẫn báo lỗi , nên ta search xem các đoạn mã hex ở phần "IHDR Image header" đẫ đúng chưa và ta thay "12 1314 49" => lại thành "00 000D 49"-> lúc này đã sửa thành công và cho ra 1 ảnh như này:
![Screenshot 2023-11-20 092510](https://hackmd.io/_uploads/BkRqXHuVa.png)

, khi nhìn vào thì ta chẳng thấy gì lúc này ta dùng tool streog để kiểm tra nhưng không có , thì ta sẽ nghĩ đến là sẽ có dòng flag ẩn bên trong cái ảnh màu đỏ này nên ta lên " paint online đổi màu lại thì thấy được dòng flag:![Screenshot 2023-11-20 092604](https://hackmd.io/_uploads/BkI0Xr_ET.png)
 
~ kết quả flag là : picoCTF{ư1z4rdry}.