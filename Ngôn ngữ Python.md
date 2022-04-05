### Kiểm tra định dạng file

Link tài liệu https://pypi.org/project/filetype/

```
Cài đặt thư viện 
# pip install filetype
```
```
def isPdf(path_to_file):
    try:
        return filetype.guess(path_to_file).mime == 'application/pdf'
    except:
        return False
        pass
```

### Duyệt tất cả file có trong thư mục và các thư mục con

```
import os
path ='/home/Module descriptions'
for dirpath, _, filenames in os.walk(path):
    for f in filenames:
        full_path = os.path.abspath(os.path.join(dirpath, f))
```

### Xử lý đường dẫn file

Đường dẫn tuyệt đối 
```
os.path.abspath(os.path.join(dirpath, f))
```
