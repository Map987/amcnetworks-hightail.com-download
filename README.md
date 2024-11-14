# amcnetworks-hightail.com-download
下载 https://spaces.hightail.com/space/4BjKr2dPHl/


```python
import requests
import time

# 创建一个会话
session = requests.Session()

# 获取当前时间戳
current_timestamp = int(time.time())

# 从sessionInfo API获取sessionId
session_info_url = f"https://api.spaces.hightail.com/api/v1/auth/sessionInfo?cacheBuster={current_timestamp}"
response = session.get(session_info_url)
session_info = response.json()

# 将sessionId添加到会话的cookies中
session.cookies['sessionId'] = session_info['sessionId']

# 使用记录的session来访问API

import re

# 主链接
main_url = "https://spaces.hightail.com/space/4BjKr2dPHl"

# 使用正则表达式提取空间ID
match = re.search(r'space/([a-zA-Z0-9]+)', main_url)
if match:
    space_id = match.group(1)
    # 构造API链接
    api_url = f"https://api.spaces.hightail.com/api/v1/spaces/url/{space_id}?status=ACTIVE"
    print(api_url)
else:
    print("无法从主链接中提取空间ID")

response = session.get(api_url)
data = response.json()

# 提取所需参数
id_param = data["id"]

download_url = f"https://download.spaces.hightail.com/api/v1/download/{id_param}?downloadFile=true"
response = session.get(download_url, stream=True)

# 检查请求是否成功
if response.status_code == 200:
    # 获取文件名
    content_disposition = response.headers.get('Content-Disposition')
    if content_disposition:
        filename = content_disposition.split('filename=')[-1].replace('"', '')
    else:
        filename = 'downloaded_file.txt'  # 如果没有文件名，使用默认文件名

    # 打开一个本地文件以写入模式
    with open(filename, 'wb') as local_file:
        for chunk in response.iter_content(chunk_size=8192):
            # 写入文件内容
            local_file.write(chunk)

    print(f"文件已保存为：{filename}")
else:
    print(f"下载失败，状态码：{response.status_code}")
name_param = data["name"]
description_param = data["description"]
updateTime_param = data["updateTime"]
creationTime_param = data["creationTime"]

# 构建新的API URL
new_api_url = f"https://api.spaces.hightail.com/api/v1/files/{id_param}/untagged?cacheBuster={current_timestamp}&depth=1&dir=ASC&limit=20&offset=0&sort=custom&spaceUrl="

# 使用同一个session来访问新的API URL
response_files = session.get(new_api_url)
files_data = response_files.json()

# 提取文件名和文件ID
children_data = files_data.get("children", [])
names_and_fileIds = [{"name": child["name"], "fileId": child["fileId"]} for child in children_data]
"""
下载图片，network中没有任何响应以及api
直接跳转到
https://hightailspaces-us-east-1.s3.amazonaws.com/aa33b6bd-ccd5-4908-ab97-7ebae90615f4?response-content-disposition=attachment%3B%20filename%2A%3DUTF-8%27%272.5%20Dimensional%20Seduction_KeyArt_A_ENG_2000x3000.jpg&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJHMEUCIQDwNkmqtkGFVsLc1AhnulE7F%2F6M%2BUfoq6a9dDkmLsIxoQIgM5CH1lBOsl38NmawYCf8HYotxkFElUHJdFX7HpEgYP8qvAUI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw3MDc5NDIxNTczMjAiDFcvbTe7mAik0s3ZdCqQBVjlS198DLnkysOJ1jLiIGi0peSSsMj5sp2d0Db%2FtY5bi65EJwVxBLSVfJGAsiR2RSMemNRvq8d1E%2Bp1UksI3qYDgsh9MyjfT5JCzGaYAiGZ%2Bk%2FVdwu0efPN3CtX%2BTAv%2B0pPCUKP9O8Iv5i7E%2Fo0UVgTLS9aO2rwnv5N%2FyERtEmaPs0DQUrIF56keckxnoc7G7dathxqzEvgHPRWa%2BNuhdYxS%2FCBk%2FkAzdsJW4i5VI%2FcbRcaCIq6KwFsNWQpFiOix34g59axqqPKK17eta2axO1Q%2B6ny5L%2FW7udI46bjorr8IvUHi7331p94YTTT%2B1B9UjgI0HQnYiZ9DoMIbuhZEd5Qln4hYvRoNrpIYGOuRGMc%2BLnLs8aD1L0cTP6%2BqMA59QrKAXwNh0zCzqpas1319oSzDePcUTjiywSyYGxtRC57e9hWU5hxYSKg4V9i6d2LxFCPKZ0zkmei6oFXgsUEWe1FzDvxyS0hdeDrbtKpyIT2WBZX%2BRBbkCGhbgF51XQorm%2F1n%2FIGn0n6mFN2qXXkasSsKdUmmYM1HnrHmUiZEA7FsbP78vX5liGU287NKLpS0NFYhM%2BduS%2BreUkD%2BoTGaiBCinPnt4SvDBKzg0FH%2BC4h7E6slwtcCNYT2%2FM3W7eP2f%2Fodig0bfAmnI0KziGrCnxHEUER0%2Fl%2BDAtSiK5L9wJAdpGWfK772%2Fc5z5p1KlgDiVyvbjKY4Tk76yxBGpu%2Bz3Kr6y%2FfwTmjnw8tXL311ORnJ6jDnun8YcYLRzAlPwy3D85Yz4BibH9UhXOjVa%2BWxS0pzqGu0wfnrUytRnO7ttLcQVzIv52ak82STACcbcZvnFLn1FfFaxG7za2VICpSOYVw%2BO3vM2RQcriEJBR2ekn8MMSO1rkGOrEBZF8WFjOvDxww%2BcOEMDU2Un%2F%2FBPvZZ3%2F89VpDpAbSOZ868xv4JJk2IVaWjptG3eTCyeSFw8hyyPe2U0a8RigmDiqjCm3CxVNi4DJPfrQlLTOC0lSYdjVjbfTG4uedHCDoZXKaSI5mFTFue8KlkVyoEafX4WwvC%2BcOeefwYLykQkkqp0i1p6WBX6Jnfb5Y9NiHW%2F9PSlqZ0mX4EYq3T5RsnN671D2tsRD37n8EGjKqMJVM&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20241114T052902Z&X-Amz-SignedHeaders=host&X-Amz-Credential=ASIA2JVFEBQEP4Y6M2FH%2F20241114%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Expires=28800&X-Amz-Signature=2679b1f8f84f6aa3024a39aaf1087d4eede47268e324ce84b5e3be0c72d5f267

电脑版倒是有压缩包下载链接
https://download.spaces.hightail.com/api/v1/download/sp-3069f26f-8ea9-4918-8df6-7fd063b50d43?downloadFile=true
"""
# 显示结果
print("ID:", id_param)
print("Name:", name_param)
print("Description:", description_param)
print("Update Time:", updateTime_param)
print("Creation Time:", creationTime_param)
print("Files:")
for file in names_and_fileIds:
    print("Name:", file["name"], "File ID:", file["fileId"])

```
