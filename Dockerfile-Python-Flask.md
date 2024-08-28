## Dockerfile with Project Python & Flask

```
FROM python:3.6-alpine

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

CMD ["python", "app.py"]
```

Giải thích:

  - Dòng đầu tiên FROM thực hiện quá trình build image với 1 image base đã được cài đặt sẵn Python phiên bản 3.6

  - Tiếp theo trong Dockerfile khởi tạo thư mục làm việc và chuyển đến thư mục làm việc /app

  - Tiếp theo COPY toàn bộ file từ folder ở môi trường gốc (bên ngoài) vào bên trong đường dẫn /app bên trong image

  - Tiếp theo thực hiện cài đặt các dependencies - cần cài đặt các packet nào thì thực hiện define ở file requirement.

  - Cuối cùng sử dụng lệnh CMD để chỉ định command line mặc định khi 1 container được khởi tạo từ image này: Khởi động file app.py

### Generate requirements.txt for your Python project on the fly

#### Install

```
pip install pipreqs
```

#### Example

```
mkdir project
cd project
vi word_cloud_generator.py
```

```
import matplotlib.pyplot as plt
from wordcloud import WordCloud
from collections import Counter
import requests
from PIL import Image
import numpy as np
import argparse
from io import BytesIO  # Import BytesIO

# Ensure you have these modules installed
# pip install matplotlib wordcloud pillow numpy requests

def get_mask_image(url):
    response = requests.get(url)
    img = Image.open(BytesIO(response.content))
    return np.array(img)

def create_word_cloud(text, mask_url=None):
    if mask_url:
        mask = get_mask_image(mask_url)
    else:
        mask = None

    wordcloud = WordCloud(width=800, height=400, background_color='white', mask=mask, contour_width=1, contour_color='black').generate(text)

    plt.figure(figsize=(10, 5))
    plt.imshow(wordcloud, interpolation='bilinear')
    plt.axis('off')
    plt.show()

def parse_arguments():
    parser = argparse.ArgumentParser(description="Generate a word cloud from input text")
    parser.add_argument('--text', '-t', required=True, help="Input text to generate the word cloud")
    parser.add_argument('--mask_url', '-m', help="URL of the image to use as mask for the word cloud")
    return parser.parse_args()

if __name__ == "__main__":
    args = parse_arguments()
    create_word_cloud(args.text, args.mask_url)
```

Generate requirements.txt file

```
pipreqs /path/to/project/location

```
