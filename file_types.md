# write_up_picoCTF_forensics_file_types.
# Đề: This file was found among some files marked confidential but my pdf reader cannot read it, maybe yours can. You can download the file from here.(Tệp này được tìm thấy trong số một số tệp được đánh dấu bí mật nhưng trình đọc pdf của tôi không thể đọc được, có thể tệp của bạn thì có thể. Bạn có thể tải tập tin từ đây.)
# url : https://artifacts.picoctf.net/c/82/Flag.pdf
# hint:Remember that some file types can contain and nest other files.(Hãy nhớ rằng một số loại tệp có thể chứa và lồng các tệp khác.)
# -------let's go---------------#
~ Đầu tiên ta bấm vào *url và có được một file : Flag.pdf, như thường lệ thì ta dùng lệnh file Flag.pdf để xem info thì kết quả " Flag.pdf: shell archive text"
~ Tiếp theo ta dùng strings/cat Flag.pdf thì thu được một đoạn mã sau: " !/bin/sh
# This is a shell archive (produced by GNU sharutils 4.15.2).
# To extract the files from this archive, save it to some FILE, remov
e
# everything before the '#!/bin/sh' line above, then type 'sh FILE'.
lock_dir=_sh00046
# Made on 2023-03-16 01:40 UTC by <root@e076735df429>.
# Source directory was '/app'.
# Existing files will *not* be overwritten, unless '-c' is specified.
# This shar contains:
# length mode       name
# ------ ---------- ------------------------------------------
#   1092 -rw-r--r-- flag
MD5SUM=${MD5SUM-md5sum}
f=`${MD5SUM} --version | egrep '^md5sum .*(core|text)utils'`
test -n "${f}" && md5check=true || md5check=false
${md5check} || \
  echo 'Note: not verifying md5sums.  Consider installing GNU coreuti
ls.'
if test "X$1" = "X-c"
then keep_file=''
else keep_file=true
echo=echo
save_IFS="${IFS}"
IFS="${IFS}:"
gettext_dir=
locale_dir=
set_echo=false
for dir in $PATH
  if test -f $dir/gettext \
     && ($dir/gettext --version >/dev/null 2>&1)
  then
    case `$dir/gettext --version 2>&1 | sed 1q` in
      *GNU*) gettext_dir=$dir"
      ", nhìn chung qua đoạn mã trên thì chẳng có được manh mối gì về flag cả . Bây giờ thì ta phải quay lại hint để xem thì có lẻ có những file khác nằm lồng trong nhau .
~ Bây giờ ta sẽ cấp quyền thực thi cho Flag.pdf , dùng lệnh chmod +x Flag.pdf -> ./Flag.pdf, thu được:" x - created lock directory _sh00046.
x - extracting flag (text)
x - removed lock directory _sh00046."
 , ta thu được một file tên flag và ta sẽ tiếp tục dùng chmod +x flag -> ./flag thu được:"./flag: 2: Syntax error: newline unexpected"=> lỗi , vậy ta dùng binwalk -e flag (để trích xuất ra xem có nhưng tệp ẩn nào nằm trong file flag hay không)=>có được một file tên '64', đây là một loại file gzip nên ta chuyển lại thành 64.gz và gzip -d 64.gz thu được file '64' ta tiếp tục binwalk -e 64 thì thu đc file 64 nhưng thuộc loại file lzip.
 ~ và tiếp tục thì lặp lại như bước trên 7749 lần thì thu được 1 file 0 : ASCII text , ta dùng lệnh cat 0 thì thu được được mã sau "069636f4354467b66316c656e406d335f6d406e3170756c407431306e5f
6630725f3062326375723137795f39353063346665657d0a"
nhìn sơ qua thì biết được đây là 1 đoạn flag được mã hóa bởi mã hex 
~ khi giải mã thì ta thu được flag sau:"  picoCTF{f1len@m3_m@n1pul@t10n_f0r_0b2cur17y_950c4fee}  " 

