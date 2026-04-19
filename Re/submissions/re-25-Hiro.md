第一部分：环境搭建记录  
1.IDA Pro 的安装与基本配置  
<img width="2078" height="1353" alt="image" src="https://github.com/user-attachments/assets/5799542e-a7a5-4665-93d4-b7a161878637" />
2.Linux 虚拟机（如 Ubuntu）的安装  
<img width="2105" height="1204" alt="image" src="https://github.com/user-attachments/assets/65f535f1-200b-4b17-9c26-d56d7d9fe16e" />
3.C 语言和 Python 开发环境的搭建  
<img width="899" height="512" alt="image" src="https://github.com/user-attachments/assets/225731a9-c4fd-4d0c-9b0e-44e54cfff8fe" />
<img width="1673" height="464" alt="image" src="https://github.com/user-attachments/assets/76291f3b-69a8-49f0-b183-9e4c6e9c8b7b" />
4.JEB 或 Jadx 的安装  
<img width="2075" height="1292" alt="image" src="https://github.com/user-attachments/assets/2ca9c070-51ed-4fee-b9fa-48d20cafb31f" />
第二部分：逆向实践与WriteUp  
1.begin
<img width="997" height="1058" alt="image" src="https://github.com/user-attachments/assets/930474af-ce78-426f-8de9-9f7dbb6a499f" />
先是一段汇编代码，按F5转成伪代码  
<img width="1485" height="475" alt="image" src="https://github.com/user-attachments/assets/e5bd9d22-2c71-4ab4-9684-10edbf9b6a35" />
根据提示，对16进制数按A，来转成ASCII 字符串并显示出来。  
<img width="694" height="42" alt="image" src="https://github.com/user-attachments/assets/e2ba6f49-46eb-4163-915f-58c33d5a085e" />
获得了第一部分。  
再根据提示，按shift 加 12.列出了程序中包含的所有可读字符串。  
<img width="1445" height="988" alt="image" src="https://github.com/user-attachments/assets/4471551b-4136-4c61-8749-50429fa014c6" />
找到了flag的第二部分。  
<img width="1484" height="22" alt="image" src="https://github.com/user-attachments/assets/04026a23-1d86-4e39-bd8f-31966ffeeae3" />
<img width="1229" height="38" alt="image" src="https://github.com/user-attachments/assets/02f63704-1f43-4fab-8537-6a11424c04a6" />
按要求对对函数名按X，发现还有一个函数交叉引用了这个函数。
<img width="835" height="520" alt="image" src="https://github.com/user-attachments/assets/c52508f9-a4b7-4a93-8584-efa84d285bb5" />
选中函数名 flag_part2 并按下 X 键，查询那个函数调用了 flag_part2  
<img width="1106" height="53" alt="image" src="https://github.com/user-attachments/assets/07bf5e44-5327-4379-8086-2bc0196f108e" />
找到了part 3  
拼接在一起，得到flag{Mak3_aN_3Ff0rt_tO_5eArcH_F0r_th3_f14g_C0Rpse}
<img width="1316" height="205" alt="image" src="https://github.com/user-attachments/assets/8fb5750a-efe8-440d-9664-40ee2b1cb2da" />
发现也可以直接在这一步按flag 3  
<img width="1202" height="77" alt="image" src="https://github.com/user-attachments/assets/7d71675b-6a12-4a36-9c89-d75c67cfb905" />
也找到了答案  
2.Simple_encryption  
依旧先按F5反编译  
<img width="818" height="815" alt="image" src="https://github.com/user-attachments/assets/87c2ffe3-266e-480d-be99-506d075790d0" />
<img width="411" height="281" alt="image" src="https://github.com/user-attachments/assets/e6e6ebc0-74f3-42c7-9a1b-47ec118882d9" />
逻辑大概是经过处理的input跟buffer完全相等，就成功。  
点开buffer，发现是一堆16进制数
<img width="1070" height="112" alt="image" src="https://github.com/user-attachments/assets/c9c07fff-f30c-4f9c-8397-8ddd9d63e935" />
71, 149, 52, 72, 164, 28, 53, 136, 100, 22, 136, 20, 106, 57, 18, 162, 10, 55, 92, 7, 90, 96, 18, 118, 37, 18, 142, 40, 0, 0
<img width="1556" height="730" alt="image" src="https://github.com/user-attachments/assets/4cd40f12-05e4-4d42-89e9-a4abe0208253" />
设计代码，反推出原数
<img width="1503" height="84" alt="image" src="https://github.com/user-attachments/assets/12349624-174c-45aa-9ad2-645c44352edf" />
<img width="852" height="412" alt="image" src="https://github.com/user-attachments/assets/de3dee78-8d6a-462f-aa3b-b52323cf3262" />
flag{IT_15_R3Al1y_V3RY-51Mp1e}



