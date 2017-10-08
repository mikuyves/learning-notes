Flask / Jinja2 / jQuery
===

## 折叠 / 展开 网页内容
可以通过 bootstrap 的 collapse 组件进行操作。

```html
<button id="{{ item.spu.asin }}" class="btn btn-primary btn-sm" type="button" data-toggle="collapse" data-target=".{{ item.spu.asin }}" aria-expanded="false">More</button>
```
* 必须指定 data-target，可以用 jinja2 生成。


```javascript
$('button').click(function () {
    $(this).text(function (i, old) {
        return old == "More" ? "Less" : "More";
    })
    $(this).toggleClass("btn-warning")
});
$('.collapse').on('shown.bs.collapse', function () {
    $('.row').masonry();
});
$('.collapse').on('hidden.bs.collapse', function () {
    $('.row').masonry();
});
```
* 点击 button 后，改变文字和样式。
* 折叠或展开之后，需要 masonry 重新排列。