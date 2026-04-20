# 一、CSS、HTML和JavaScript的进一步应用
---
## CSS

### 1.统一全局格式
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: "Microsoft YaHei";
}

### 2.导航栏固定在顶部
nav{
    background: linear-gradient(to right,#ff9d00,#ffb900);
    position: sticky;
    top: 0;
    box-shadow: 0 2px 8px rgba(0,0,0,0.15);
}

### 3.让元素整齐排列（flex弹性布局）
.nav-container{
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 80px;
}

### 4.鼠标悬浮效果（变色、晃动）
.nav-item:hover{
    background-color: rgba(255,255,255,0.25);
    transform: translateY(-2px);
}

### 5.下拉二维码（hover显示）
.dropdown-content{
    display: none;
}
.dropdown:hover .dropdown-content{
    display: block;
}

### 6.图片晃动动画
@keyframes floatShake{
    0%{transform: translateY(0);}
    25%{transform: translateY(-3px) rotate(-1deg);}
    50%{transform: translateY(0);}
}
.butterfly-decoration:hover{
    animation: floatShake 0.8s infinite;
}

### 7.岗位卡片不同颜色
.dev .quote{color: #4a90e2;}
.pm .quote{color:#e84393;}
.game .quote{color: #27ae60;}

### 8.Q&A默认隐藏
.qa-answer{
    display: none;
}
.qa-item.active .qa-answer{
    display: block;
}
## HTML

### 1. 搭建整个网页最基础的结构
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>Geek 招新官网</title>
</head>
<body>
    所有内容都放在这里
</body>
</html>

### 2.划分区域
<nav> 导航栏 </nav>
<section id="introduction"> 首页介绍 </section>
<section id="job-intro-section"> 岗位介绍 </section>
<section id="about-us"> 关于我们 </section>
<section id="contact-us"> 联系我们 </section>
<section id="q-a"> 问答区 </section>
<footer> 页脚版权 </footer>

### 3.放图片、按钮、链接等等
<img src="images/decoration_Geek.png" alt="Logo">
<h3>开发程序猿</h3>
<p>前端：HTML CSS JavaScript...</p>
<a href="#introduction">Introduction</a>
<button class="back-to-top">↑</button>

### 4.可以放装饰小图案
<svg width="300" height="200">
    <path d="M10 30 Q 30 10, 50 30" stroke="#333"/>
</svg>

### 5.可以做下拉二维码板面
<div class="dropdown">
    <a>Introduction</a>
    <div class="dropdown-content">
        <img src="二维码图片">
    </div>
</div>

## JavaScript

### 1.图片晃动
floatElements.forEach(el=>{
    el.addEventListener('mouseenter',()=>{
        el.style.animationPlayState='running';
    });
    el.addEventListener('mouseleave',()=>{
        el.style.animationPlayState='paused';
    });
});

### 2.点击导航栏平滑到对应的区域
document.querySelectorAll('a[href^="#"]').forEach(anchor=>{
    anchor.addEventListener('click',function(e){
        e.preventDefault();
        const targetId = this.getAttribute('href');
        document.querySelector(targetId).scrollIntoView({behavior:'smooth'});
    });
});

### 3.Q&A点击展开
qaItems.forEach(item => {
    item.addEventListener('click', () => {
        item.classList.toggle('active');
    });
});

### 4.回到顶部按钮
window.addEventListener('scroll', () => {
    if (window.pageYOffset > 200) {
        backToTopBtn.style.display = 'block';
    } else {
        backToTopBtn.style.display = 'none';
    }
});
backToTopBtn.addEventListener('click', () => {
    window.scrollTo({ top: 0, behavior: 'smooth' });
});

# 二、问题及其解决办法
---
## 1.问题：网页的图片显示不出来
     解决办法：
     需要把images的文件存放在和html文件都同一个文件中，最后在引用时用的语句“images/...”,还要注意图片是.png的形式还是.jpg的形式。
## 2.问题：文字有时候挤在一起，或者图片不在想要的位置
     解决办法：
     应把固定的值改为自适应的模式，例如：gap:80px（固定值）应该为gap:80px;flex-wrap:wrap;自适应模式会好一点。
## 3.问题：鼠标下拉时二维码不会现实出来
    解决办法：要给下拉的内容提高层次，代码中的z-index:数值，数值越大越在上面，越大的在上面就越容易看见，所以可点击。
## 4.问题：Q&A的答案不能展开
    解决办法：实际上是代码编译器的转换时的异常，三角符代码编译错误正确的应该是&#9660.
## 5.图片挡住下面的文字
    解决办法：实际上是z-index后面的数值占的太大，需要减小层次，或者装饰图后面添加pointer-events: none
# 三、Other Ideal
## 1.
作为一个招生官网是面向小白来进行宣传的，所以我觉得可以继续优化在手机上打开的便利性，因为刚开始我也是没有电脑的，所以我也想要在手机打开时不用滑动，直接就可以看网页的全内容，对一些值得提的点进行字体上的放大或者用例外不同的颜色，这样可以吸引小白对某一部分的注意力，接下来可以在这个注意词添加一个类似链接跳转的部分来解释来想表达的部分，我认为最好是生活中接触过的东西来进行描述，或者利用相关视频，我认为视频可能比文字更有吸引力。
## 2.
在对Q&A进行展开时可以添加一个动画的的展现，使得展开时不会那么生硬。
## 3.
在联系我们时可以添加一个一个表达进行在线报名，直接可以提送报名表。
## 4.
可以添加一个报名截止进度条，显示“距离报名截止还有多少天，请抓紧报名哦！”等等
## 5.
 学习一个新的模拟，把这个网页当成“模板”：以后做其他招新、社团介绍、个人展示的网页，都能套用这个结构（导航+首页+内容+联系+问答+页脚），改改文字和图片就能用
