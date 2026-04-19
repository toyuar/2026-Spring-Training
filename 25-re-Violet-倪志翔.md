# 配置环境

## IDA Pro
<img width="2558" height="1369" alt="image" src="https://github.com/user-attachments/assets/89577315-ae8c-4298-afe7-b8831d74417a" />

##ub虚拟机
<img width="1476" height="1064" alt="ad5456e0e8da874b5f579a1fc29da302" src="https://github.com/user-attachments/assets/8d9ce56f-d102-4f69-bbde-4e3449c40eb7" />

##虚拟机测试C语言和Python
<img width="919" height="674" alt="ae0440cb9e9baf3cffe1f79877b19020" src="https://github.com/user-attachments/assets/49916007-7232-455c-bd8a-87cea6ea54f8" />

##jadx
<img width="1762" height="994" alt="image" src="https://github.com/user-attachments/assets/6c6af612-9d3d-4d11-ad1e-2aad50924d36" />

#做题部分

## begin
1.main函数F5查看代码
<img width="1499" height="650" alt="9e3298c3cdd13f6c28c2011ab76daea7" src="https://github.com/user-attachments/assets/a841a844-7289-4334-a41a-5225666de19b" />

2.对变量名按X查看调用它的函数
<img width="1202" height="507" alt="33beeef892aebf67bdc728146fb01182" src="https://github.com/user-attachments/assets/97df733b-b546-4071-a38d-80f29d08caf6" />

<img width="706" height="533" alt="eb5070e997a47abaecaeef68e9910124" src="https://github.com/user-attachments/assets/3b2ef25e-76a1-4133-935c-774c6df3e4dd" />

3.根据提示，函数名为flag第三部分，故对函数名点击X查看调用它的函数
<img width="565" height="477" alt="ef01ed1a9a18ed35f0a579bf921e7022" src="https://github.com/user-attachments/assets/06daa9ce-8593-4c83-b2cb-f94bda08c394" />

4.三部分拼接起来得到flag{Mak3_aN_3Ff0rt_tO_5eArcH_F0r_th3_f14g_C0Rpse}
<img width="788" height="115" alt="image" src="https://github.com/user-attachments/assets/78e6d3ce-df8b-4ce0-a9e0-7687661537a5" />

## base64
1.通过字符串页面进入，发现切入点：26长度字符串
<img width="1229" height="490" alt="image" src="https://github.com/user-attachments/assets/8a328b1d-e9b3-4ed7-9529-cdb791f3deb9" />
2.再从correct flag页面看到g84Gg6m2ATtVeYqUZ9xRnaBpBvOVZYtj+Tc=和WHydo3sThiS7ABLEIO0k5trange+CZfVlGRvup81NKQbjmPzU4MDc9Y6q2XwFxJ/
<img width="1335" height="341" alt="image" src="https://github.com/user-attachments/assets/70c7567d-5668-4d1f-a6b8-6dee08d671e0" />
4.对转换后的码进行64base解码
<img width="874" height="453" alt="image" src="https://github.com/user-attachments/assets/90a83c95-7f1b-4cfe-b8f1-ad2591694488" />
5.得到flag{y0u_kn0w_base64_well}
<img width="619" height="203" alt="image" src="https://github.com/user-attachments/assets/d0e20459-891b-45d6-88b1-aa788fef1dc7" />

## Simple_encryption
1.查看main函数伪代码
<img width="1169" height="807" alt="image" src="https://github.com/user-attachments/assets/97af09b8-d308-4e19-b007-9bfda6ffebc6" />
2.分析得知，正确输入经过变换后，必须满足变换后的 input[j] = buffer[j]
3.然后就不会了
