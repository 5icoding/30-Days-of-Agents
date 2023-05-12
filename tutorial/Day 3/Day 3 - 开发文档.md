## 参考案例资料
* 《生成式智能体：人类行为的交互式模拟》(Generative Agents: Interactive Simulacra of Human Behavior)
### 生成式智能体架构
* 我们需要一种机制来将智能体的推理与沙盒世界联系起来。为了实现这一点，我们将沙盒环境——区域和对象——表示为一个树状数据结构，树中的边表示沙盒世界中的包含关系。我们将这棵树转换为自然语言，传递给生成型智能体。例如，“炉子”是“厨房”的子对象，被渲染成“厨房里有一个炉子”。

* 感知 存储器流 检索 检索到的存储器 行为 计划 反思的架构图

### 智能体构造
* 环境
* 智能体
1. 感知器
2. 主体
* 当前内部状态（历史状态、规律、历史动作、目标）
* 规划
* 目标设定（目标）
3. 执行器

### 代码分析
 ```javascript
let persona_names = {"Isabella_Rodriguez": [72, 14], "Tom_Moreno": [73, 74], "Ayesha_Khan": [118, 61], "Ryan_Park": [66, 19], "Jane_Moreno": [72, 74], "John_Lin": [90, 74], "Klaus_Mueller": [127, 46], "Arthur_Burton": [54, 14], "Yuriko_Yamamoto": [29, 65], "Eddy_Lin": [94, 74], "Wolfgang_Schulz": [111, 60], "Giorgio_Rossi": [87, 18], "Tamara_Taylor": [54, 74], "Francisco_Lopez": [16, 32], "Sam_Moore": [39, 62], "Carmen_Ortiz": [58, 74], "Carlos_Gomez": [93, 18], "Abigail_Chen": [36, 18], "Latoya_Williams": [16, 18], "Jennifer_Moore": [37, 68], "Hailey_Johnson": [26, 32], "Maria_Lopez": [123, 57], "Rajiv_Patel": [25, 18], "Mei_Lin": [90, 74], "Adam_Smith": [22, 65]};
	var spawn_tile_loc = {};
	for (var key in persona_names){
		spawn_tile_loc[key] = persona_names[key] ;
	}

	var personas = {};
	var pronunciatios = {};
	var speech_bubbles = {};
	let anims_direction;
	let pre_anims_direction;
	let pre_anims_direction_dict = {};

    //用于存储从后端服务器发送的移动的变量。

    let execute_count_max = tile_width/movement_speed;
	let execute_count = execute_count_max;
	let movement_target = {};
	let all_movement = {}

    function preload() {

    }
    function create() {

    }
    function update(time, delta) {
        //resume:恢复场景(启动key场景的update)，无数据处理
        play_context.scene.resume()
    }
    
```

## 开始学习phaser3 
* 参考代码：https://github.com/photonstorm/phaser3-examples/blob/master/public/src/games/firstgame/part1.html

## 注意事项
* 对于phaser3的网页，图片都是基于服务器的定位，这个需要注意
* npm install -g http-server
* http-server 文件路径 -p 8888 -o