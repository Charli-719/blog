---
title: react实现抽奖大转盘
date: 2019-06-22 15:00:48
tags: react
categories: react
---

先看一下实现的效果（这样才有动力）
![最后实现效果图](react实现抽奖大转盘/01.gif)

<!-- more -->

1、html代码
```
   <canvas className="item" id="wheelcanvas" height={422} width={422}/>
```
2、绘制圆盘
```
	let canvas = document.getElementById("wheelcanvas");
	// 获取canvas的上下文,context含有各种api用来操作canvas
	let context = canvas.getContext('2d');
	this.setState({canvas: canvas, context: context});
	context.save();
	// 新建一个路径,画笔的位置回到默认的坐标(0,0)的位置
	// 保证了当前的绘制不会影响到之前的绘制
	context.beginPath();
	// 设置填充转盘用的颜色,fill是填充而不是绘制
	context.fillStyle = '#fff';
	// 绘制一个圆,有六个参数,分别表示:圆心的x坐标,圆心的y坐标,圆的半径,开始绘制的角度,结束的角度,绘制方向(false表示顺时针)
	context.arc(211, 211, 211, startRadian, Math.PI * 2 + startRadian, false);
	// 将设置的颜色填充到圆中,这里不用closePath是因为closePath对fill无效.
	context.fill();
	// 将画布的状态恢复到上一次save()时的状态
	context.restore();
```
![](react实现抽奖大转盘/02.jpg)
2、绘制奖品
canvas绘制都是从水平出开始绘制，所以这里我调整了初始弧度
```
	let awards = [
		{id: 1, name: '一等奖', level: '1', color:"#FFC200"},
	   {id: 2, name: '二等奖', level: '2', color:"#FFE122"},
	   {id: 3, name: '三等奖', level: '3', color:"#FFC200"},
	    {id: 4, name: '四等奖', level: '4', color:"#FFE122"},
	    {id: 5, name: '五等奖', level: '5', color:"#FFC200"},
	    {id: 6, name: '六等奖', level: '6', color:"#FFE122"},
	],//大转盘要绘制的奖品
	let startRadian = -90 * Math.PI / 180,//大转盘的开始弧度(canvas绘制圆从水平方向开始，所以这里调整为垂直方向) 弧度计算公式：角度*Math.PI/180
	// 第一个奖品色块开始绘制时开始的弧度及结束的弧度
    let RadianGap = Math.PI * 2 / awards.length, endRadian = startRadian + RadianGap;
    for (let i = 0; i < awards.length; i++) {
      context.save();
      context.beginPath();
      // 为了区分不同的色块,使用随机生成的颜色作为色块的填充色
      context.fillStyle = awards[i].color;
      // 这里需要使用moveTo方法将初始位置定位在圆点处,这样绘制的圆弧都会以圆点作为闭合点
      context.moveTo(211, 211);
      // 画圆弧时,每次都会自动调用moveTo,将画笔移动到圆弧的起点,半径设置的比转盘稍小一点
      context.arc(211, 211, 201, startRadian, endRadian, false);
      context.fill();
      context.restore();
      // 开始绘制文字
      context.save();
      //设置文字颜色
      context.fillStyle = '#f00';
      //设置文字样式
      context.font = "18px Arial";
      // 改变canvas原点的位置,简单来说,translate到哪个坐标点,那么那个坐标点就将变为坐标(0, 0)
      context.translate(
        211 + Math.cos(startRadian + RadianGap / 2) * 201,
        211 + Math.sin(startRadian + RadianGap / 2) * 201
      );
      // 旋转角度,这个旋转是相对于原点进行旋转的.
      context.rotate(startRadian + RadianGap / 2 + Math.PI / 2);
      // 这里就是根据获取的各行的文字进行绘制,maxLineWidth取70,相当与一行最多展示5个文字
      this.getLineTextList(context, awards[i].name, 70).forEach((line, index) => {
        // 绘制文字的方法,三个参数分别带:要绘制的文字,开始绘制的x坐标,开始绘制的y坐标
        context.fillText(line, -context.measureText(line).width / 2, ++index * 25)
      });
      context.restore();
      // 每个奖品色块绘制完后,下个奖品的弧度会递增
      startRadian += RadianGap;
      endRadian += RadianGap;
    }
```
```
//绘制文字，文字过长进行换行，防止文字溢出
  getLineTextList(context, text, maxLineWidth) {
    let wordList = text.split(''), tempLine = '', lineList = [];
    for (let i = 0; i < wordList.length; i++) {
      if (context.measureText(tempLine).width >= maxLineWidth) {
        lineList.push(tempLine);
        maxLineWidth -= context.measureText(text[0]).width;
        tempLine = ''
      }
      tempLine += wordList[i]
    }
    lineList.push(tempLine);
    return lineList
  }
```
![](react实现抽奖大转盘/03.jpg)
3、绘制中间的小圆
```
	//下面是画中间的小圆
    context.save();
    // 新建一个路径,画笔的位置回到默认的坐标(0,0)的位置
    // 保证了当前的绘制不会影响到之前的绘制
    context.beginPath();
    // 设置填充转盘用的颜色,fill是填充而不是绘制
    context.fillStyle = '#fff';
    // 绘制一个圆,有六个参数,分别表示:圆心的x坐标,圆心的y坐标,圆的半径,开始绘制的角度,结束的角度,绘制方向(false表示顺时针)
    context.arc(211, 211, 70, startRadian, Math.PI * 2 + startRadian, false);
    // 将设置的颜色填充到圆中,这里不用closePath是因为closePath对fill无效.
    context.fill();
    // 将画布的状态恢复到上一次save()时的状态
    context.restore();
```
![](react实现抽奖大转盘/04.jpg)
4、添加中间的按钮和转盘边框
```
//html
<img className="wheel-circle" src="https://wp.touty.io/api/file/5d0c51ff2ab79c000897ecac.image"/>
          <canvas className="item" id="wheelcanvas" height={422} width={422}/>
          <img onClick={this.draw.bind(this)} className="pointer" src="https://wp.touty.io/api/file/5d0c51c02ab79c000897ecaa.image"/>
```
这样一个静态的大转盘就完成了，下面是让转盘转动起来
![](react实现抽奖大转盘/05.jpg)
5、点击抽奖让转盘转起来
```
  //点击抽奖让转盘转起来
  draw(e) {
    const {canvas, context} = this.state;
    // 只要抽奖没有结束，就不让再次抽奖
    if (!this.state.canBeClick) return;
    this.state.canBeClick = false;
    // 每次点击抽奖，都将初始化角度重置
    this.state.startRadian = 0;
    const distance = this.distanceToStop();
    this.rotatePanel(distance);//调用处理旋转的方法
  }
```
得出最后停留的地方
```
distanceToStop() {
    // middleDegrees为奖品块的中间角度（最终停留都是以中间角度进行计算的）距离初始的startRadian的距离，distance就是当前奖品跑到指针位置要转动的距离。
    let middleDegrees = 0, distance = 0;
    // 映射出每个奖品的middleDegrees
    let awardsToDegreesList = this.state.awards.map((data, index) => {
      let awardRadian = (Math.PI * 2) / this.state.awards.length;
      return awardRadian * index + (awardRadian * (index + 1) - awardRadian * index) / 2
    });
    // 随机生成一个索引值，来表示此次抽奖应该中的奖品
    const currentPrizeIndex = Math.floor(Math.random() * this.state.awards.length);
    console.log('当前奖品应该中的奖品是：' + this.state.awards[currentPrizeIndex].name);
    middleDegrees = awardsToDegreesList[currentPrizeIndex];
    // 因为指针是垂直向上的，相当坐标系的Math.PI/2,所以这里要进行判断来移动角度
    distance = Math.PI * 3 / 2 - middleDegrees;
    distance = distance > 0 ? distance : Math.PI * 2 + distance;
    // 这里额外加上后面的值，是为了让转盘多转动几圈，看上去更像是在抽奖
    return distance + Math.PI * 10;
  }
```
让转盘转动
```
// 处理旋转的关键方法
  rotatePanel(distance) {
    // 这里用一个很简单的缓动函数来计算每次绘制需要改变的角度，这样可以达到一个转盘从块到慢的渐变的过程
    let changeRadian = (distance - this.state.startRadian) / 20;
    this.state.startRadian += changeRadian;
    // 当最后的目标距离与startRadian之间的差距低于0.0001时，就默认奖品抽完了，可以继续抽下一个了。
    if (distance - this.state.startRadian <= 0.001) {
      this.state.canBeClick = true;
      return
    } ;
    // 初始角度改变后，需要重新绘制
    this.onLoadPage(this.state.awards);
    // 循环调用rotatePanel函数，使得转盘的绘制连续，造成旋转的视觉效果
    window.requestAnimationFrame(this.rotatePanel.bind(this, distance));
  }
```
这样我们就得到了最终的效果图
![](react实现抽奖大转盘/01.gif)
完整代码查看：[codePen](https://codepen.io/fuzhen/project/editor/XEyRdE)
