## 改变图片颜色

imageWithRenderingMode

UIImageRenderingModelAlwaysOriginal 按图片文件原来的样子渲染
UIImageRenderingModelAlwaysTemplate 扫描图片，从图片中所有不透明的像素创建一个模版，同时忽略图片的颜色信息。可以用 tintColor 填充。
UIImageRenderingModelAutomatic 根据图片的使用环境决定

从 Xcode 6 开始，可以在 Assets Catalog 中选择。

## @3x 图

使用矢量 PDF。

实际上，是编译时，根据矢量图自动生成 @1x、@2x、@3x 的 png 图片。

设计 PDF 图片时，以 1x 为准。

## 设备启动图

使用 LanchScreen.xib

## 拉长图片

slicing

