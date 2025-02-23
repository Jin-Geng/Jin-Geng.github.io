# mkdocs搭建心得
!!! info "前言"
    本文档使用`mkdocs-material`搭建，本文记录了一些搭建过程中遇到的问题以及相应解决方案，以供参考。

## 链接在新标签页打开
使用`[]()`的链接形式默认为当前标签页跳转，在其后加上`{:target="_blank"}`即可在新标签页跳转

> `[官网的教程](https://squidfunk.github.io/mkdocs-material/reference/images/#light-and-dark-mode){:target="_blank"}`

## 显示视频
使用`video`标签即可引入视频资源，可以在外层包裹`div`标签来修改视频显示的长宽属性。其中`width`属性无法超出文档正文部分的原有最大宽度
（或者`admonition`部分的最大宽度），`height`属性可以任意调节

> `height`属性设置在`300px`左右即可，大约是`2:1`的宽高比，不会太占用页面空间

!!! tip ""
    ``` html linenums="1"
    <div style="width:auto; height:300px; display:flex; justify-content:center; align-items:end;">
        <video class="light-mode-video" autoplay muted loop style="width:100%; height:100%; object-fit:cover;">
            <source src="./img/bgvideo.mp4" type="video/mp4">
        </video>
    </div>
    ```

## 在暗色和亮色主题下显示不同组件
!!! tip "显示不同图片可以参考[官网的教程](https://squidfunk.github.io/mkdocs-material/reference/images/#light-and-dark-mode){:target="_blank"}"

参考官网对展示不同图片的写法，我们也可以实现在不同主题下展示不同视频（其他组件同理）
!!! info ""
    === "HTML"
        在要展示的部分添加对应的组件，此处为`video`，将其分为两个部分，分别添加对应主题的视频
        并设置不同的`class`以控制在不同主题下的`display`属性值
    
        ``` html 
        <div id="video-container" style="width:auto; height:320px; display:flex; justify-content:center; align-items:end;">
            <video class="light-mode-video" autoplay muted loop style="width:100%; height:100%; object-fit:cover;">
                <source src="./img/bgvideo.mp4" type="video/mp4">
            </video>
            <video class="dark-mode-video" autoplay muted loop style="width:100%; height:100%; object-fit:cover;">
                <source src="./img/wallpaper.mp4" type="video/mp4">
            </video>
        </div>
        ```

    === "CSS"
        根据`data-md-color-scheme`的值来决定`light-mode-class`和`dark-mode-class`中`display`属性对应的值。
        注意`default`与`light`相同，`slate`对应`dark`。这部分添加在`docs/stylesheets/extra.css`中，同时`extra.css`需在`mkdocs.yml`
        中`extra_css`中挂载，这部分可以参考[官网](https://squidfunk.github.io/mkdocs-material/customization/#additional-css){:target="_blank"}。

        ``` css 
        [data-md-color-scheme="light"] .light-mode-video {
            display: block;
        }
        [data-md-color-scheme="light"] .dark-mode-video {
            display: none;
        }
        [data-md-color-scheme="default"] .light-mode-video {
            display: block;
        }
        [data-md-color-scheme="default"] .dark-mode-video {
            display: none;
        }
        [data-md-color-scheme="slate"] .light-mode-video {
            display: none;
        }
        [data-md-color-scheme="slate"] .dark-mode-video {
            display: block;
        }
        ```

## 修改`inline/inline-end`块的宽度
在`extra.css`中添加如下`css`代码，注意`inline-end`和`inline`是一起修改的。
``` css 
.md-typeset .admonition.inline {
    width: 280px; /* 设置为你需要的宽度 */
}
```
## 添加代码注释
注意根据代码类型来选择注释符。例如`Java`、`C++`等需要使用`//`，而`html`则需要使用`<!---->`

## 嵌入B站视频
点击网页端B站视频的分享按钮，可以看到如下图嵌入代码选项，点击即可一键复制。
!!! tip ""
    ![b站截图](../static/mkdoc_bilibili.png)

嵌入代码使用`iframe`标签，示例如下：
``` html
<iframe src="//player.bilibili.com/player.html?isOutside=true&aid=113792172163702&bvid=BV12krYYmEn7&cid=27763017100&p=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>
```

代码中可以添加属性值来控制视频样式，例如添加`style`属性来控制视频的宽高等。

``` html
<iframe ··· style="width:100%; height:380px; object-fit:cover;"/> <!-- (1)! -->
```

1. `height`在`400px`左右可以使视频基本填满`admonition`

其他属性值，例如在`src`属性值后添加`&autoplay=0`字段可以控制视频加载时暂停（默认自动播放），可以参考下图：
!!! tip ""
    ![播放器参数](../static/mkdoc_bili_player.png)

以及几个网站：[站外(外链)播放器使用文档](https://player.bilibili.com/)、[接入B站iframe视频](https://blog.csdn.net/xinshou_caizhu/article/details/94028606)、[使用更干净的哔哩哔哩iframe播放器](https://cloud.tencent.com/developer/article/2266871?areaSource=102001.12&traceId=_Wck1ahshaLqcfjbzXTy2)

## 图片放大功能
参考[官网教程](https://squidfunk.github.io/mkdocs-material/reference/images/)，运行`pip install mkdocs-glightbox`安装`glightbox`插件，并在
`mkdoc.yml`内添加`plugins: - glightbox`即可。