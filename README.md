
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
      //主角
      player = stage.createItem({
        width: 30,
        height: 30,
        type: 1,
        location: map,
        eatSound: new Audio("eat.mp3"),
        dieSound: new Audio("death.mp3"),
        coord: { x: 13.5, y: 23 },
        orientation: 2,
        speed: 2,
        frames: 10,
        update: function () {
          var coord = this.coord;
          if (!coord.offset) {
            if (typeof this.control.orientation != "undefined") {
              if (
                !map.get(
                  coord.x + _COS[this.control.orientation],
                  coord.y + _SIN[this.control.orientation]
                )
              ) {
                this.orientation = this.control.orientation;
              }
            }
            this.control = {};
            var value = map.get(
              coord.x + _COS[this.orientation],
              coord.y + _SIN[this.orientation]
            );
            if (value == 0) {
              this.x += this.speed * _COS[this.orientation];
              this.y += this.speed * _SIN[this.orientation];
            } else if (value < 0) {
              this.x -= map.size * (map.x_length - 1) * _COS[this.orientation];
              this.y -= map.size * (map.y_length - 1) * _SIN[this.orientation];
            }
          } else {
            if (!beans.get(this.coord.x, this.coord.y)) {
              //吃豆
              _SCORE++;
              beans.set(this.coord.x, this.coord.y, 1);
              this.eatSound.play(); // 添加音效
              if (config["goods"][this.coord.x + "," + this.coord.y]) {
                //吃到能量豆
                this.eatSound.play(); // 添加音效
                items.forEach(function (item) {
                  if (item.status == 1 || item.status == 3) {
                    //如果NPC为正常状态，则置为临时状态
                    item.timeout = 450;
                    item.status = 3;
                  }
                });
              }
              //吃到减速道具
							if(config['slow'][this.coord.x+','+this.coord.y]){	
								items.forEach(function(item){

									if(item.status==1||item.status==3){	//如果NPC为正常状态，则置为临时状态

										item.timeout = 450;
										item.status = 5;

									}
								});
							}
              //吃到一路顺风道具
							if(config['go home'][this.coord.x+','+this.coord.y]){	
								items.forEach(function(item){

									if(item.status==1||item.status==3){	//如果NPC为正常状态，则置为临时状态

										item.timeout = 450;
										item.status = 4;

									}
								});
							}
            }
            this.x += this.speed * _COS[this.orientation];
            this.y += this.speed * _SIN[this.orientation];
          }
        },
```

## 开发人员

| 用户名           | 邮箱                         |
| ---------------- | ---------------------------- |
| gongChengYuan-30 | 3216158096@qq.com            |
| kittleyu         | dxyu1117@126.com             |
| Gj1102           | 2022011167@bistu.edu.cn      |
| Yujia040528      | 2022011173@bistu.edu.cn      |
| W-71             | WangNinghahaha@Outlook.com   |
