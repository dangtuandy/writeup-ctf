# Write_up_picoCTF_forensics_Trivial-Flag-Transfer-Protocol.
# Đề:Figure out how they moved the flag.
# url: https://mercury.picoctf.net/static/ed308d382ae6bcc37a5ebc701a1cc4f4/tftp.pcapng
# Hints: What are some other ways to hide data?(Một số cách khác để ẩn dữ liệu là gì?)
# -----------let's go-----------------  #
~ Đầu tiên ta bấm vào *url thì được 1 file tftp.pcapng dễ dang thấy nó thì ta sẽ dùng wireshark để phân tích.
~ wireshark tftp.pcapng để mở :-1: thì như thường lệ bấm đúp vào để xem nội dung trao đổi và nháy chuột phải chọn follow thì thấy có sự trao đổi của 1 vài file txt, deb và file ảnh bmp.
~ Từ những manh mối trên thì ta vào phần file->export objects->tftp thì thấy có 6 file ta save về lần lượt xem từng file thì thấy:
* file: instructions.txt contents: GSGCQBRFAGRAPELCGBHEGENSSVPFBJRZHFGQVFTHVFRBHESYNTGENAFSRE.SVTHERBHGNJNLGBUVQRGURSYNTNAQVJVYYPURPXONPXSBEGURCYNA  có vẻ đoạn mã này đoạn mã hóa bởi rot13 khi giải mã-> TFTPDOESNTENCRYPTOURTRAFFICSOWEMUSTDISGUISEOURFLAGTRANSFER.FIGUREOUTAWAYT
* file : plan contents: VHFRQGURCEBTENZNAQUVQVGJVGU-QHRQVYVTRAPR.PURPXBHGGURCUBGBF-> IUSEDTHEPROGRAMANDHIDITWITH-DUEDILIGENCE.CHECKOUTTHEPHOTOS ta nhận thấy được đoạn giải mã trên là nhưng chữ đoạn được ->I USED THE PROGRAM AND HIDIT WITH - DUEDILIGENCE.CHECKOUT THE PHOTOS(TÔI ĐÃ SỬ DỤNG CHƯƠNG TRÌNH VÀ ẨN VỚI - DUEDILIGENCE.Kiểm tra các bức ảnh)
* file: program.deb -> chứa nội dung về phương thức và công cụ giấu info trong ảnh bằng phươn thức steghide thuộc streog 
~ sau khi đọc 1 hồi thì biết được flag được giấu 1 trong 3 hình trên nên ta sử dụng lệnh kiểm tra từ i:1->3 : steghide extract -sf picture(i).bmp -p  DUEDILIGENCE  mật khẩu chính là: DUEDILIGENCE được giải mã từ file plan ở trên và thử lần lượt qua 3 cái ảnh thì cuối cùng cũng tìm ra flag ở ảnh thứ 3 .

~ Kết quả flag : picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}

