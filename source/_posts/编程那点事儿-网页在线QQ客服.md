---
title: 为自己的网页添加在线QQ客服💁
date: 2017-05-29 0:56:21
categories: 编程那点事儿
tags:
  - QQ
---
<blockquote class="blockquote-center">QQ有时候就像水流一样，看上去并不强大，但无孔不入。</blockquote>

<!--more-->

## 写在前面

一个强大完整的网络产品包括很多元素，很久之前就留意过国外诸多产品有个<a href="https://www.intercom.com/">优秀的在线客服系统</a>（你可以打开本链接看下它右下角，就是它自己提供的在线客服服务），想着闲来无事时可以探索一下这玩意。今天，时候到了。

上面这个服务简洁不落俗套，操作起来也很简单（<a href="https://forum.qzy.camp/t/gem-intercom-rails/800">戳我看配置教程</a>），可以先试用。不过对我来说并无实质的功能需求，等哪时创业了再说吧（意淫ing），先搞个免费的把玩把玩足够。国人代码向来搬运自海外，有时候青出于蓝胜于蓝，为了接地气，我又翻了几家国内的服务（<a href="http://qiyukf.com/">网易七鱼</a>，<a href="http://www.ewei.com/">易维帮助台</a>），然而丑到爆，尾大不掉...

经过一番折腾，自己的需求逐渐清晰：

- 可以自定义客服窗口UI
- 免费
- 功能性暂无要求，有个意思即可

于是，就有了下面的动作。

## 添加网页在线QQ客服

搜索一下“在线QQ客服代码”即可出现各种资源。再一次不得不佩服马化腾的产品路线之广，方方面面无处不在。

以下只记录我相册这个效果实现。（<a href="http://51world.win/gallery/index.html">戳我看效果，没错，就是那个调皮的等你哔哔的爱因斯坦</a>）

### 引用 CSS

``` bash
#cs_box {
    width: 120px;
    height: 220px;
    color: #FFF;
    background: #333;
    position: fixed;
    right: 10px;
    top: 100px;
    border-radius: 10px;
    border: 1px solid #777;
    z-index: 1000
}

#cs_box span {
    height: 20px;
    line-height: 20px;
    display: block;
}

.cs_close {
    color: #FFF;
    position: absolute;
    right: 10px;
    top: 8px;
    cursor: pointer;
    font-size: 20px;
    font-family: Verdana, Geneva, sans-serif
}

.cs_title {
    font-size: 14px;
    margin: 10px;
    font-weight: bold;
}

.cs_img {
    width: 100px;
    height: 100px;
    background: #FFF;
    margin: 10px;
    background-image: url(http://ogudt6aal.bkt.clouddn.com/image/zaixianqq.png)<!-- 这是我七牛上的爱因斯坦图片的路径 -->
}

.cs_info {
    font-size: 12px;
    margin: 0px 10px;
    overflow: hidden;
    text-align: center;
}

.cs_btn {
    width: 100px;
    height: 25px;
    background: #666;
    margin: 5px 10px;
    border-radius: 5px;
    font-size: 12px;
    line-height: 25px;
    color: #FFF;
    text-align: center;
    cursor: pointer;
}
```

### 引用JS

``` bash
function myEvent(obj,ev,fn){
	if (obj.attachEvent){
		obj.attachEvent('on'+ev,fn);
	}else{
		obj.addEventListener(ev,fn,false);
	};
};

function getByClass(obj,sClass){
	var array = [];
	var elements = obj.getElementsByTagName('*');
	for (var i=0; i<elements.length; i++){
		if (elements[i].className == sClass){
			array.push (elements[i]);
		};
	};
	return array;
};

var cs_box = {
	set : function(json){
		this.box = document.getElementById('cs_box');
		//this.setimg(json);
		this.qqfn(json);
		this.cs_close();
	},
	qqfn : function(json){
		this.btn = getByClass(this.box,'cs_btn')[0];
		var link = 'http://wpa.qq.com/msgrd?v=3&uin='+json.qq+'&site=qq&menu=yes';
		this.btn.onclick = function(){
			window.open(link,'_blank');
		};
	},
	cs_close : function(){
		this.btn = getByClass(this.box,'cs_close')[0];
		var _this = this;
		var speed = 0;
		var timer = null;
		var sh = document.documentElement.clientHeight || document.body.clientHeight;
		this.btn.onclick = function(){
			clearInterval(timer);
			timer = setInterval(function(){
				speed += 4;
				var t = _this.box.offsetTop + speed;
				if (t >= sh-_this.box.offsetHeight){
					speed *= -0.8;
					t = sh-_this.box.offsetHeight;
				};
				if (Math.abs(speed)<2)speed = 0;
				if (speed == 0  && sh-_this.box.offsetHeight == t){
					clearInterval(timer);
					_this.fn();
				};
				_this.box.style.top = t + 'px';
			}, 30);
		};
	},
	fn : function(){
		var _this = this;
		var timer = setTimeout(function(){
			_this.box.style.display = 'none';
		}, 1000);
	},
};

```


### 添加对应的 html

body 结束前添加以下代码：

``` bash
<div id='cs_box'>
  <span class='cs_title'>-在线吐槽-</span>
  <span class='cs_close'>x</span>
  <div class='cs_img'></div>
  <span class='cs_info'>有什么想哔哔？</span>
  <div class='cs_btn'>点击开撩</div>
</div>
<script src="js/xxx.js"></script><!-- 这里写刚才js的路径 -->
<script>
  myEvent(window,'load',function(){
    cs_box.set({
      qq : 'yyy',<!-- 这里填写你客服的企鹅🐧号 -->
    });
  });
</script>

```

### 测试

第一次玩这玩意，你需要开通QQ的某种功能（这功能在时刻更新，你按图索骥即可）。

结束。
