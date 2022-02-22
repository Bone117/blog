# 清理特殊文本

## 清理特殊文本

> - unicodedata.normalize(form, unistr)
>
> 把一串UNICODE字符串转换为普通格式的字符串，具体格式支持NFC、NFKC、NFD和NFKD格式。
>
> Unicode标准定义了四种规范化形式： Normalization Form D (NFD)，Normalization Form KD (NFKD)，Normalization Form C (NFC)，和Normalization Form KC (NFKC)。大约来说，NFD和NFKD将可能的字符进行分解，而NFC和NFKC将可能的字符进行组合。

```python
import unicodedata
a = 'pýtĥöñ is awesome\n'
b = unicodedata.normalize('NFD',a)
b.encode('ascii', 'ignore').decode('ascii')
```
