# 1.图片作为背景图片

要使图片作为背景图片充满整个屏幕，可以使用 CSS 的 background-image 和 background-size 属性。

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>weiki</title>
    <style>
        /* 设置body的样式，使背景图覆盖整个屏幕 */
        body, html {
            height: 100%;
            margin: 0;
        }
        /* 设置背景图，并使其覆盖整个屏幕 */
        body {
            background-image: url('https://take-saas.oss-cn-hangzhou.aliyuncs.com/wechat_applets/wxshare/coach_daily_background/bg_1.png'); /* 替换为自己的图片路径 */
            background-position: center center; /* 图片居中 */
            background-repeat: no-repeat; /* 不重复 */
            background-size: cover; /* 覆盖整个屏幕 */
        }
    </style>
</head>
<body>
    <!-- 页面内容 -->
</body>
</html>
```

**效果图**
![在这里插入图片描述](/public/web/htmlAndCss/7020cd333ec642cc982e20f94fef4c04.png)
![在这里插入图片描述](/public/web/htmlAndCss/33e0423a3da34fbcadc005958269d7be.png)

- background-image 属性用于指定背景图的路径
- background-size: cover; 用于确保背景图覆盖整个屏幕，而不会失真或重复
- background-position: center center; 用于确保图片在屏幕中居中显示
- background-repeat: no-repeat; 确保图片不会重复

使用时将 url('') 中的 '' 替换为实际的背景图片路径。确保图片的尺寸足够大，以适应不同分辨率的屏幕。

# 图片非背景图

要使图片作为前景图片充满整个屏幕，可以使用 CSS 的 width 和 height 属性，并将图片的 position 设置为 absolute 或 fixed。

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>weiki</title>
    <style>
        /* 设置body的样式，使前景图覆盖整个屏幕 */
        body, html {
            height: 100%;
            margin: 0;
            overflow: hidden; /* 防止出现滚动条 */
        }
        /* 设置前景图，并使其覆盖整个屏幕 */
        .full-screen-image {
            width: 100%;
            height: 100%;
            position: absolute;
            top: 0;
            left: 0;
            object-fit: cover; /* 保持图片的纵横比 */
        }
    </style>
</head>
<body>
    <!-- 前景图片 -->
    <img src="https://take-saas.oss-cn-hangzhou.aliyuncs.com/wechat_applets/wxshare/coach_daily_background/bg_1.png" class="full-screen-image" alt="Full Screen Image">
</body>
</html>
```

**效果图**
![在这里插入图片描述](/public/web/htmlAndCss/7020cd333ec642cc982e20f94fef4c04.png)
![在这里插入图片描述](/public/web/htmlAndCss/33e0423a3da34fbcadc005958269d7be.png)
