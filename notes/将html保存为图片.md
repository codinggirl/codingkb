---
title: 将html保存为图片
created: '2019-09-04T10:37:24.908Z'
modified: '2019-09-04T10:37:33.663Z'
---

# 将html保存为图片

在《[将SVG保存为图片\[译\]](https://www.tangshuang.net/3595.html)》一文中介绍了一种将svg保存为图片保存到本地的一种方案。但是这篇文章里面，还存在一个问题，它使用了a标签的download属性。这个属性并不能支持版本较低（9）的IE。本文更进一层，不仅导出svg，连html也可以导出保存的方案。

import html2canvas from 'html2canvas'
import download from 'downloadjs'
export default class Html2Image {
    static canvas(el, options, factory) {
        if(typeof el === 'string') {
            el = document.querySelector(el)
        }
        let type = options.type || 'png'
        let filename = (options.name || 'download') + '.' + type
        let filetype = 'image/' + type
        options.width = options.width || el.offsetWidth
        options.height = options.height || el.offsetHeight
        options.onrendered = canvas => {
            factory(canvas, filename, filetype)
        }
        html2canvas(el, options)
        return this
    }
    static base64(el, options, factory) {
        this.canvas(el, options, (canvas, filename, filetype) => {
            let dataurl = canvas.toDataURL(filetype)
            factory(dataurl, filename, filetype)
        })
    }
    static blob(el, options, factory) {
        this.canvas(el, options, (canvas, filename, filetype) => {
            canvas.toBlob(blob => factory(blob, filename, filetype), 'image/' + filetype, 1)
        })
    }
    static save(el, options) {
        this.base64(el, options, download)
        return this
    }
}

使用方法：

import Html2Image from './html2image'
Html2Image.save('#container', {
  name: 'my\-image',
  type: 'jpg',
})

就是这么简单。我们使用到了两个包html2canvas和downloadjs，你需要用npm去安装它们。使用componer对它进行预览，直接在我的[github](https://github.com/tangshuang/html2image)上拿源码。

Html2Image还提供了blob, base64等方法，你可以根据你自己的需要来选择使用哪一个方法。


[将html保存为图片_唐霜的博客](https://www.tangshuang.net/3607.html)

