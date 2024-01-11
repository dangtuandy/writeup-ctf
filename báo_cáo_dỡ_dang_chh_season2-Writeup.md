**Báo cáo dang dỡ cookiehanhoan season2**

* Em tải 1 file thuộc loại `MEMORY.DMP: MS Windows 64bit crash dump, full dump, 524158 pages` nên em sẽ tiến hành dùng tool Volatility (phiên bản 3) để phân tích, trước tiên thì em dùng plugin `pstree` để xem các process trong file đó:
* ![Screenshot 2024-01-11 210603](https://hackmd.io/_uploads/BkqxAOp_T.png)
, dựa vào đề cho Hòa làm một bài báo mà chưa kịp lưu và em cũng tìm hiểu sơ qua bản chất của các `ImageFileName`(ở hình dưới) nên đã biết được nó liên quan đến process WINWORD.exe.
* Tiếp theo em dùng plugin `handles` để in ra danh sách các đường dẫn có trong process này: `python3/home/kali/task/volatility3/vol.py -f MEMORY.DMP -o /home/kali/task/bo_loc windows.handles --pid=1736`
*![Screenshot 2024-01-11 214938](https://hackmd.io/_uploads/HyWKeKad6.png)
, nhưng có rất nhiều đường dẫn file bên trong process này nên em dựa vào đề cũng như suy đoán của em thì em sẽ lọc ra những cái `key thuộc là file`:
* ![Screenshot 2024-01-11 215416](https://hackmd.io/_uploads/Bk2m-KpOT.png)
,tới đây em thấy có 1 đường dẫn rất khả nghi đó là `1736    WINWORD.EXE     0xfa80041e2070  0x720   File    0x12019f        \Device\HarddiskVolume2\Users\admin\AppData\Roaming\Microsoft\Word\AutoRecovery save of Document1.asd
` , nên tiến hành dump file về và quan sát bên trong chứa rất nhiều file:
* ![Screenshot 2024-01-11 213129](https://hackmd.io/_uploads/HkNh-KTuT.png), nếu mà dùng tool xem phần hex thì sẽ thấy dễ hơn
* Nên từ đó em sẽ tiến hành dùng lệnh `binwalk --dd='.*' 'file.0xfa80041e2070.0xfa8003d7b6d0.DataSectionObject.AutoRecovery save of Document1.asd.dat' ` và lục trong đống file đó là một file zip bên trong chứa 2 ảnh thì 1 ảnh chứa flag:
* ![Screenshot 2024-01-11 213403](https://hackmd.io/_uploads/HyW5MK6OT.png)
**FLAG:** *CHH{4ut0R3c0v3r_s4v3_my_l1f3}*