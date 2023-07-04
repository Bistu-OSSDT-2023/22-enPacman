
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

## 开发人员

| 用户名           | 邮箱                         |
| ---------------- | ---------------------------- |
| gongChengYuan-30 | 3216158096@qq.com            |
| kittleyu         | dxyu1117@126.com             |
| Gj1102           | 2022011167@bistu.edu.cn      |
| Yujia040528      | 2022011173@bistu.edu.cn      |
| W-71             | WangNinghahaha@Outlook.com   |
