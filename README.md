
https://github.com/Bistu-OSSDT-2023/22-enPacman/assets/114382806/492d5460-e140-402b-9e01-d33dd9ed8cc5

# Pacman 吃豆游戏

- 项目演示(DEMO)地址：[https://bistu-ossdt-2023.github.io/22-enPacman/]

## 目录

- [版权](#版权)
- [功能](#功能)
- [开发人员](#开发人员)

### 版权
本游戏开源代码来自于 [passer-by.com](https://passer-by.com/) ，本项目团队在其基础上进行开发

好的，以下是美化后的代码：

## 版权

本游戏开源代码来自于 [passer-by.com](https://passer-by.com/) ，本项目团队在其基础上进行开发。

## 功能

- [x] 地图绘制
- [x] 玩家控制
- [x] NPC根据玩家坐标实时自动寻径
- [x] 吃豆积分系统
- [x] 进攻豆功能
- [x] 减速豆功能
- [x] 回家豆功能  
- [x] 多关卡(共12关)
- [x] 特殊物品记分
- [x] 特殊音效设置
- [x] 地图美化设置

## 功能具体代码

### 减速豆功能

```js
// 如果有减速道具，进行绘制 
if (config['slow'][i + ',' + j]) {
  context.beginPath(); // 表示开始一个新的路径，用于定义绘制的形状
  context.arc(pos.x, pos.y, 5 + this.times % 2, 0, 2 * Math.PI, true);
  context.fillStyle = "#FFB6C1"; // 并进行颜色的填充
  context.fill(); // 圆形区域的颜色填充
  context.closePath(); // 结束当前路径
}
```

### 回家豆功能

```js
// 如果有回家道具，进行绘制
if (config['go home'][i + ',' + j]) {
  context.beginPath(); // 表示开始一个新的路径，用于定义绘制的形状
  context.arc(pos.x, pos.y, 5 + this.times % 2, 0, 2 * Math.PI, true);
  context.fillStyle = "#FFA500"; // 并进行颜色的填充
  context.fill(); // 圆形区域的颜色填充
  context.closePath(); // 结束当前路径
}
```

### 特殊音效功能

```js
//创建背景音乐
    var bgm = new Audio('bgm.mp3');
    //音乐播放器
    var bgms = [];
    var loop = function () {
      bgms.push(bgm.play());
      if (bgms.length === 5) bgms.shift();
      setTimeout(loop, 500);
    }
    loop();
 //游戏主程序
  (function () {
    _COIGIG.forEach(function (config, index) {
      var stage, map, beans, items, player;
      var winSound = new Audio("win.mp3"); //胜利音效
      stage = game.createStage({
        update: function () {
          var stage = this;
          if (stage.status == 1) {
            //场景正常运行
            items.forEach(function (item) {
              if (
                map &&
                !map.get(item.coord.x, item.coord.y) &&
                !map.get(player.coord.x, player.coord.y)
              ) {
                var dx = item.x - player.x;
                var dy = item.y - player.y;
                if (dx * dx + dy * dy < 750 && item.status != 4) {
                  //物体检测
                  if (item.status == 3) {
                    item.status = 4;
                    _SCORE += 10;
                  } else {
                    stage.status = 3;
                    stage.timeout = 30;
                  }
                }
              }
            });
            if (JSON.stringify(beans.data).indexOf(0) < 0) {
              //当没有物品的时候，进入下一关
              winSound.play(); //播放胜利音效
              game.nextStage();
            }
          } else if (stage.status == 3) {
            //场景临时状态
            if (!stage.timeout) {
              _LIFE--;
              if (_LIFE) {
                stage.resetItems();
              } else {
                var stages = game.getStages();
                game.setStage(stages.length - 1);
                return false;
              }
            }
          }
        },
      });
      //绘制地图
      map = stage.createMap({
        x: 60,
        y: 10,
        data: config["map"],
        cache: true,
        draw: function (context) {
          context.lineWidth = 2;
          for (var j = 0; j < this.y_length; j++) {
            for (var i = 0; i < this.x_length; i++) {
              var value = this.get(i, j);
              if (value) {
                var code = [0, 0, 0, 0];
                if (
                  this.get(i + 1, j) &&
                  !(
                    this.get(i + 1, j - 1) &&
                    this.get(i + 1, j + 1) &&
                    this.get(i, j - 1) &&
                    this.get(i, j + 1)
                  )
                ) {
                  code[0] = 1;
                }
                if (
                  this.get(i, j + 1) &&
                  !(
                    this.get(i - 1, j + 1) &&
                    this.get(i + 1, j + 1) &&
                    this.get(i - 1, j) &&
                    this.get(i + 1, j)
                  )
                ) {
                  code[1] = 1;
                }
                if (
                  this.get(i - 1, j) &&
                  !(
                    this.get(i - 1, j - 1) &&
                    this.get(i - 1, j + 1) &&
                    this.get(i, j - 1) &&
                    this.get(i, j + 1)
                  )
                ) {
                  code[2] = 1;
                }
                if (
                  this.get(i, j - 1) &&
                  !(
                    this.get(i - 1, j - 1) &&
                    this.get(i + 1, j - 1) &&
                    this.get(i - 1, j) &&
                    this.get(i + 1, j)
                  )
                ) {
                  code[3] = 1;
                }
                if (code.indexOf(1) > -1) {
                  context.strokeStyle =
                    value == 2 ? "#FFF" : config["wall_color"];
                  var pos = this.coord2position(i, j);
                  switch (code.join("")) {
                    case "1100":
                      context.beginPath();
                      context.arc(
                        pos.x + this.size / 2,
                        pos.y + this.size / 2,
                        this.size / 2,
                        Math.PI,
                        1.5 * Math.PI,
                        false
                      );
                      context.stroke();
                      context.closePath();
                      break;
                    case "0110":
                      context.beginPath();
                      context.arc(
                        pos.x - this.size / 2,
                        pos.y + this.size / 2,
                        this.size / 2,
                        1.5 * Math.PI,
                        2 * Math.PI,
                        false
                      );
                      context.stroke();
                      context.closePath();
                      break;
                    case "0011":
                      context.beginPath();
                      context.arc(
                        pos.x - this.size / 2,
                        pos.y - this.size / 2,
                        this.size / 2,
                        0,
                        0.5 * Math.PI,
                        false
                      );
                      context.stroke();
                      context.closePath();
                      break;
                    case "1001":
                      context.beginPath();
                      context.arc(
                        pos.x + this.size / 2,
                        pos.y - this.size / 2,
                        this.size / 2,
                        0.5 * Math.PI,
                        1 * Math.PI,
                        false
                      );
                      context.stroke();
                      context.closePath();
                      break;
                    default:
                      var dist = this.size / 2;
                      code.forEach(function (v, index) {
                        if (v) {
                          context.beginPath();
                          context.moveTo(pos.x, pos.y);
                          context.lineTo(
                            pos.x - _COS[index] * dist,
                            pos.y - _SIN[index] * dist
                          );
                          context.stroke();
                          context.closePath();
                        }
                      });
                  }
                }
              }
            }
          }
        },
      });
```

## 开发人员

| 用户名           | 邮箱                         |
| ---------------- | ---------------------------- |
| gongChengYuan-30 | 3216158096@qq.com            |
| kittleyu         | dxyu1117@126.com             |
| Gj1102           | 2022011167@bistu.edu.cn      |
| Yujia040528      | 2022011173@bistu.edu.cn      |
| W-71             | WangNinghahaha@Outlook.com   |
