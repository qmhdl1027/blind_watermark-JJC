> [!TIP]
> **JJC 定制版** - 同步自 guofei9987/blind_watermark，qmhdl1027 维护

# blind_watermark

> [!TIP]
> 🚀 **JJC 定制版** — 同步自 blind_watermark/guofei9987，Python 盲水印工具中文版

**blind_watermark** 是一款 Python 实现的盲水印工具，能够在不访问原始图片的情况下嵌入水印，也无需原始图片即可提取水印，适用于版权保护、来源追溯等场景。

---

## 项目简介

盲水印（Blind Watermark）是一种数字水印技术，其独特之处在于：**嵌入水印时无需访问原始图片，提取水印时也无需原始图片**。这使得 blind_watermark 非常适合以下场景：

- 🛡️ **版权保护**：为图片嵌入不可见的版权信息
- 🔍 **来源追溯**：追踪图片被转载/盗用的路径
- 💬 **信息传递**：在图片中隐蔽传递文字信息

---

## 安装

\\\ash
pip install blind-watermark
\\\

---

## 快速开始

### 嵌入水印

\\\python
import blind_watermark as bw

# 嵌入水印
bwm = bw.Watermark('wm_text')
bwm.read_img('original.png')
bwm.embed('output.png')
\\\

### 提取水印

\\\python
import blind_watermark as bw

# 提取水印（无需原始图片）
bwm = bw.Watermark('wm_text')
bwm.read_img('output.png')
wm = bwm.extract()
print(f"提取的水印: {wm}")
\\\

---

## 支持的算法

- **DCT 域水印**：基于离散余弦变换的鲁棒水印
- **小波域水印**：基于离散小波变换的水印
- **LSB 水印**：最低有效位水印

---

## 资源链接

- GitHub：https://github.com/guofei9987/blind_watermark

---

## License

MIT License
