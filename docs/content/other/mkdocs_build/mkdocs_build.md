# mkdocs搭建

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