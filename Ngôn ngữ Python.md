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

Hoặc sử dụng glob

```
import glob
glob.glob(folder + "/*.jpg")
```

### Xử lý đường dẫn file

Đường dẫn tuyệt đối 
```
os.path.abspath(os.path.join(dirpath, f))
```

Tách thư mục từ đường dẫn đến 1 file
```
folder = os.path.dirname(full_path)
```

Tách tên file từ đường dẫn
```
filename = os.path.basename(full_path)
```

### Gán bản quyền (watermark) vào file PDF

Cài đặt thư viện
```
pip install Pillow
```

```
from PIL import Image
def addImages(inFile, outFile, filWatermark, position):
    base_image = Image.open(inFile)
    watermark = Image.open(filWatermark)
    
    # (350, 350) là sai ảnh watermark sẽ chèn vào
    watermark = watermark.resize((350, 350))
    width, height = base_image.size
    transparent = Image.new('RGB', (width, height), (0,0,0,0))
    transparent.paste(base_image, (0,0))
    transparent.paste(watermark, position, mask=watermark)
    transparent.save(outFile)
```

### Chuyển đổi  ảnh màu sang ảnh trắng đen

```
import cv2
list_pages = glob.glob(folder + "/*.jpg")
for p in list_pages:
    originalImage = cv2.imread(p)
    grayImage = cv2.cvtColor(originalImage, cv2.COLOR_BGR2GRAY)
    (thresh, blackAndWhiteImage) = cv2.threshold(grayImage, 170, 255, cv2.THRESH_BINARY)
    cv2.imwrite(p, blackAndWhiteImage)
```

### Chuyển đổi PDF sang các file ảnh

Cài đặt thư viện
```
pip install pdf2image
```

```
from pdf2image import convert_from_path
images = convert_from_path(path, grayscale=True)
for i in range(len(images)):
    images[i].save( folder + '/page'+ str(i) +'.jpg', 'JPEG')
```

### Chuyển đổi file ảnh sang PDF

Cài đặt thư viện

```
pip install img2pdf
```

```
import img2pdf
with open("output/" + filename,"wb") as f:
    f.write(img2pdf.convert(glob.glob(folder + "/*.jpg")))
```

### Save data sang một file json và ngược lại

Ghi file
```
import json
with open('error.json', 'w', encoding='utf8') as outfile:
    json.dump(list_file_errror, outfile, ensure_ascii=False)
```

Đọc file
```
import json
dsfile = []
with open('data.json', encoding='utf8') as json_file:
    dsfile = json.load(json_file)
```
