# mkdocs搭建

## 显示视频
使用`video`标签即可引入视频资源，可以在外层包裹`div`标签来修改视频显示的长宽属性。其中`width`属性无法超出文档正文部分的原有最大宽度
（或者`admonition`部分的最大宽度），`height`属性可以任意调节

> `height`属性一般设置为`300px`，大约是`2:1`的宽高比，不会太占用页面空间

!!! tip ""
    ``` html linenums="1"
    <div style="width:auto; height:300px; display:flex; justify-content:center; align-items:end;">
        <video class="light-mode-video" autoplay muted loop style="width:100%; height:100%; object-fit:cover;">
            <source src="./img/bgvideo.mp4" type="video/mp4">
        </video>
    </div>
    ```