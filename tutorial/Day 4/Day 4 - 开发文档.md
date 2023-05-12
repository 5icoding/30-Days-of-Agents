# phaser开发特点
## 游戏描述
1. 程序
* 程序有一个游戏(game)
* 游戏有一个dude(男人)。
* 游戏有许多star(星星)。
* 游戏有许多平台(platform)。
* 游戏有许多炸弹(bomb)。
* 游戏有背景(sky)。
* 游戏有一个记分牌(scoreText)
2. 男人
* 男人捡拾star(星星)。
* 男人碰到炸弹(bomb)死亡。
* 男人跳到平台(platform)。
* 男人左走。
* 男人右走。
3. 代码解析
``` javascript
var player; //有玩家
var stars;  //有星星
var bombs;  //有一个男人
var platforms; //有一个平台
var cursors;
var score = 0;
var gameOver = false;
var scoreText; //有一个记分牌

//有一个游戏
var game = new Phaser.Game(config); 
```
## 游戏代码细节
1. 游戏构建，由下面三个方法
``` javascript
    scene: {
        preload: preload, //提前装在资源
        create: create,   //创建画面
        update: update    //实时更新
    }
```
* create 将物理引擎和实体各自的逻辑关系对应好
* update 主要是对玩家的控制

2. this.physics.add.collider（对撞机）函数
``` javascript
//  Collide the player and the stars with the platforms
this.physics.add.collider(player, platforms);
this.physics.add.collider(stars, platforms);
this.physics.add.collider(bombs, platforms);

//  Checks to see if the player overlaps with any of the stars, if he does call the collectStar function
this.physics.add.overlap(player, stars, collectStar, null, this);

this.physics.add.collider(player, bombs, hitBomb, null, this);
```
* 第一组 来用物理引擎增加各个实体的关系
* 第二个 物理引擎 玩家和星星的重叠
* 第三个 玩家和炸弹的对撞 

## 说明
这里只是简单的说明一下代码，因为我们主要的是看模拟人生的代码
