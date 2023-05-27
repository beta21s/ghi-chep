# Quản lý FILE

#### Cắt một FILE có dung lượng lớn thành N FILE có kích thước N

```
split -d -b 10GB file2_pc1 file

# Kết quả nhận được sẽ là các file nhỏ hơn được đánh tự động
file00,
file01
----
fileN N
```

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>
