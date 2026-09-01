# [blind-watermark](https://github.com/qmhdl1027/blind_watermark-JJC)

基于 DWT-DCT-SVD 的**盲水印**技术。

[![PyPI](https://img.shields.io/pypi/v/blind_watermark)](https://pypi.org/project/blind_watermark/)
[![Build Status](https://travis-ci.com/guofei9987/blind_watermark.svg?branch=master)](https://travis-ci.com/guofei9987/blind_watermark)
[![codecov](https://codecov.io/gh/guofei9987/blind_watermark/branch/master/graph/badge.svg)](https://codecov.io/gh/guofei9987/blind_watermark)
[![License](https://img.shields.io/pypi/l/blind_watermark.svg)](https://github.com/guofei9987/blind_watermark/blob/master/LICENSE)
![Python](https://img.shields.io/badge/python->=3.5-green.svg)
![Platform](https://img.shields.io/badge/platform-windows%20|%20linux%20|%20macos-green.svg)
[![stars](https://img.shields.io/github/stars/guofei9987/blind_watermark.svg?style=social)](https://github.com/guofei9987/blind_watermark/)
[![fork](https://img.shields.io/github/forks/guofei9987/blind_watermark?style=social)](https://github.com/guofei9987/blind_watermark/fork)
[![Downloads](https://pepy.tech/badge/blind-watermark)](https://pepy.tech/project/blind-watermark)
[![Discussions](https://img.shields.io/badge/discussions-green.svg)](https://github.com/guofei9987/blind_watermark/discussions)

> [!TIP]
> 🚀 **JJC 定制版** — 同步自 [guofei9987/blind_watermark](https://github.com/guofei9987/blind_watermark)，盲水印工具中文版

---

## 📖 项目简介

**盲水印**（Blind Watermark）是一种在图像中嵌入可见或不可见水印信息的技术，提取时**无需原图**，嵌入过程**不影响原图质量**。

本库基于 **DWT-DCT-SVD**（离散小波变换 + 离散余弦变换 + 奇异值分解）算法实现，支持：

- 文字水印嵌入与提取
- 图片水印嵌入与提取
- 抗常见攻击（旋转、缩放、裁剪、噪声等）

---

## 🔧 功能特性

### 水印嵌入

将水印信息嵌入图片，**嵌入后几乎不影响原图视觉效果**，肉眼难以察觉。

### 水印提取

从含水印图片中提取水印信息，**无需原始图片**，只需密码和原水印形状即可提取。

### 支持的攻击类型

| 攻击类型 | 说明 |
|----------|------|
| 旋转 45° | 抗旋转攻击 |
| 随机裁剪 | 抗裁剪攻击 |
| 遮挡/马赛克 | 抗局部遮挡 |
| 垂直/水平剪切 | 抗剪切攻击 |
| 缩放 | 抗缩放攻击 |
| 高斯噪声 | 抗噪声攻击 |
| 亮度调整 | 抗亮度变化 |

---

## 📥 安装

```bash
pip install blind-watermark
```

开发者版本（最新代码）：

```bash
git clone git@github.com:qmhdl1027/blind_watermark-JJC.git
cd blind_watermark-JJC
pip install .
```

---

## 🚀 快速上手

### 命令行使用

```bash
# 嵌入水印（将水印文字嵌入图片）
blind_watermark --embed --pwd 1234 examples/pic/ori_img.jpeg "水印文字" examples/output/embedded.png

# 提取水印（从含水印图片中提取）
blind_watermark --extract --pwd 1234 --wm_shape 111 examples/output/embedded.png
```

### Python 使用

原图 + 水印 = 打上水印的图

原图示例：

![原图](docs/原图.jpeg)

加上文字水印 `@guofei9987 开源万岁！` 后：

![打上水印的图](docs/打上水印的图.jpg)

> 💡 可以看出水印几乎不可见，肉眼无法分辨原图和水印图的差异

---

#### 嵌入文字水印

```python
from blind_watermark import WaterMark

# 步骤1：创建水印实例（设置密码）
bwm1 = WaterMark(password_img=1, password_wm=1)

# 步骤2：读取原始图片
bwm1.read_img('pic/ori_img.jpg')

# 步骤3：读取水印文字
wm = '@guofei9987 开源万岁！'
bwm1.read_wm(wm, mode='str')

# 步骤4：嵌入水印，保存为新图片
bwm1.embed('output/embedded.png')

# 获取水印比特长度（提取时需要用到）
len_wm = len(bwm1.wm_bit)
print('水印比特长度 len_wm:', len_wm)
```

#### 提取文字水印

```python
from blind_watermark import WaterMark

# 步骤1：创建水印实例（与嵌入时使用相同密码）
bwm1 = WaterMark(password_img=1, password_wm=1)

# 步骤2：提取水印（无需原图！）
wm_extract = bwm1.extract('output/embedded.png', wm_shape=len_wm, mode='str')

print('提取的水印:', wm_extract)
# 输出: @guofei9987 开源万岁！
```

---

### 嵌入图片水印

#### 嵌入图片水印

```python
from blind_watermark import WaterMark

bwm1 = WaterMark(password_img=1, password_wm=1)
# 读取原始图片
bwm1.read_img('pic/ori_img.jpg')
# 读取水印图片
bwm1.read_wm('pic/watermark.png')
# 嵌入水印
bwm1.embed('output/embedded.png')
```

#### 提取图片水印

```python
from blind_watermark import WaterMark

bwm1 = WaterMark(password_img=1, password_wm=1)
# 注意：需要知道水印图片的形状
bwm1.extract(filename='output/embedded.png', wm_shape=(128, 128),
             out_wm_name='output/extracted.png')
```

---

### 嵌入二进制水印

作为演示，以下嵌入 6 字节数据：

```python
wm = [True, False, True, True, True, False]
```

**嵌入：**

```python
from blind_watermark import WaterMark

bwm1 = WaterMark(password_img=1, password_wm=1)
bwm1.read_img('pic/ori_img.jpg')
bwm1.read_wm([True, False, True, True, True, False], mode='bit')
bwm1.embed('output/embedded.png')
```

**提取：**

```python
from blind_watermark import WaterMark

bwm1 = WaterMark(password_img=1, password_wm=1, wm_shape=6)
wm_extract = bwm1.extract('output/打上水印的图.png', mode='bit')
print('提取的水印:', wm_extract)
```

> ⚠️ 注意：`wm_shape`（水印形状/长度）是提取时的必要参数，嵌入时建议记录下来。

> 💡 提取出的 `wm_extract` 是一个浮点数数组，建议设置阈值为 0.5 进行二值化。

---

## ⚙️ 性能配置

```python
WaterMark(..., processes=None)
```

- `processes` 参数设置进程数，默认为 `None`（使用所有 CPU 核心）。
- 设置为 `1` 可禁用多进程。

---

## 📚 更多示例

- [examples/example_str.py](https://github.com/guofei9987/blind_watermark/blob/master/examples/example_str.py) — 文字水印完整示例
- [examples/example_bit.py](https://github.com/guofei9987/blind_watermark/blob/master/examples/example_bit.py) — 二进制水印完整示例
- [examples/example_pic.py](https://github.com/guofei9987/blind_watermark/blob/master/examples/example_pic.py) — 图片水印完整示例

---

## 📖 文档

- 英文文档：https://BlindWatermark.github.io/blind_watermark/#/en/
- 中文文档：https://BlindWatermark.github.io/blind_watermark/#/zh/

---

## 🔗 相关项目

- [text_blind_watermark](https://github.com/guofei9987/text_blind_watermark) — 将水印信息嵌入文本
- [HideInfo](https://github.com/guofei9987/HideInfo) — 图片中隐藏信息（图片隐写）、声音中隐藏信息（音频隐写）、文本中隐藏信息
