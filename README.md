# Printer Demo

[![License](https://img.shields.io/badge/license-MIT-4EB1BA.svg)](https://www.opensource.org/licenses/mit-license.php)

零墨云旗下打印机Demo汇总

| Demo                | Bluetooth | TCP/IP | USB | 备注                  |
|---------------------|:---------:|:------:|:---:|---------------------|
| [Android](#Android) |     ✓     |   ✓    |  ✓  | Bluetooth BLE & SPP |
| [uni-app](#uni-app) |     ✓     |   ×    |  ×  | Bluetooth BLE       |
| [微信小程序](#微信小程序)     |     ✓     |   ×    |  ×  | Bluetooth BLE       |
| [企业微信H5](#企业微信H5)   |     ✓     |   ×    |  ×  | Bluetooth BLE       |

## 对接注意事项

所有零墨云旗下A4机器(包括但不限于：喜鹊系列、节纸系列、危废系列)由于字体种类繁多、版权等因素不支持字库打印，
无法使用文本相关指令。【内置部分字库只用于显示，无法满足打印需求】

A4打印机均采用**图形打印**方式，所以需要把要打印的内容转换或生成一张**黑白图片**，使用图形打印。

### 打印流程

* 把要打印的文件(Word、Excel、PDF等)转为转换**黑白图片**，或直接按照模板绘制一张**黑白图片**。【各DEMO中有绘制图形打印示例】
* 使用`CPCL`SDK把`图片`转换为`CPCL`指令
* 通过蓝牙(推荐BLE）、TCP/IP、USB扫描并连接打印机
* 把`CPCL`指令发送到打印机。
* 打印完成。

### 打印机DPI和图片大小的关系

打印机有`203DPI`、`300DPI`两种规格，其中`203DPI`也常被称为`200DPI`。

#### DPI、毫米和dots(px)的关系

**转换方式：**

* 使用公式计算：dots(px)=mm*dpi/25.4
* 简化算法：
    - 203DPI：1mm=8dots(px)
    - 300DPI：1mm=12dots(px)
    - 只使用整数部分。例如 2 mm = 23.6 dots 则将使用 23 dots。

**常用示例：**

* 100mm在不同dpi下的分辨率
    - 203DPI：100mm * 8 = 800px
    - 300DPI：100mm * 11.8 = 1180px

* 150mm在不同dpi下的分辨率
    - 203DPI：150mm * 8 = 1200px
    - 300DPI：150mm * 11.8 = 1770px

* 200mm在不同dpi下的分辨率
    - 203DPI：200mm * 8 = 1600px
    - 300DPI：200mm * 11.8 = 2360px

* A4: 210mm * 297mm
    - 203dpi: A4 = 1680px * 2376px
    - 300dpi: A4 = 2478px * 3504px

* A5: 210mm * 148mm
    - 203dpi: A5 = 1680px * 1184px
    - 300dpi: A5 = 2478px * 1746px

#### 如何确定图片大小？

图片实际打印出来的尺寸取决于图片本身的大小，缩放图片大小可以控制打印在纸张上的尺寸。

> 等比例缩放图片算法伪代码

```javascript
// 等比缩放(缩小、放大)规则，防止变形
function scale(image, targetWidth, targetHeight) {
    originalWidth = image.width;
    originalHeight = image.height;
    widthRatio = originalWidth / originalWidth;
    heightRatio = originalHeight / originalHeight;
    if (widthRatio > heightRatio) {
        resize(image, targetWidth, originalHeight * widthRatio);
    } else {
        resize(image, originalWidth * heightRatio, targetHeight);
    }
}
```

比如使用`203DPI`的打印机把一张`500px*600px`的图片铺满打印在`A4(210mm*297mm)`张上：

* `A4(210mm*297mm)`在`203DPI`下对应的dots(px)为：`1680px*2376px`
* 按照等比缩放图片`scale(image, 1680, 2376)`
* `500*600`的图片填充到A4后就是`1680*2016`

## Q&A

* 纸张定位错误解决办法
    1. 纸张漏出的部分不要超过2cm合上盖子。
    2. 长按大键3秒后松开，打印机自动学习纸张。
    3. 然后开盖把纸张卷回去，然后露出部分不要超过2cm合上盖子使用。

## Demos

### Android

- [printer-demo-android][android]

### uni-app

- [printer-demo-uni-app(HBuilderX版本)][uni-app]
- [printer-demo-uni-app(NodeJS版本)][uni-app-nodejs]

### 微信小程序

- [printer-demo-wx-miniprogram][wx-miniprogram]

### 企业微信H5

- [printer-demo-wx-cp-h5][wx-cp-h5]

[android]: https://github.com/lingmoyun/printer-demo-android

[uni-app]: https://github.com/lingmoyun/printer-demo-uni-app

[uni-app-nodejs]: https://github.com/lingmoyun/printer-demo-uni-app/tree/nodejs

[wx-miniprogram]: https://github.com/lingmoyun/printer-demo-wx-miniprogram

[wx-cp-h5]: https://github.com/lingmoyun/printer-demo-wx-cp-h5
