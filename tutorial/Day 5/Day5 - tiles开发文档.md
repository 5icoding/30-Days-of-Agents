# 代码详解
## 代码演变步骤
* game_1.html 有1个小人，键盘控制，这是起点。
* game.html中的代码，这是参考
* all_movement.json，是互动的数据参考。
* game_2.html 有5个小人，自由游走，这是目标。


参考代码：
```javascript
player = this.physics.add.
            sprite(2400, 588, "atlas", "down").
            setSize(30, 40).
            setOffset(0, 0);
player.setDepth(-1);
// Setting up the camera. 
const camera = this.cameras.main;
camera.startFollow(player);
camera.setBounds(0, 0, map.widthInPixels, map.heightInPixels);
cursors = this.input.keyboard.createCursorKeys();


const anims = this.anims;
for (let i=0; i<Object.keys(persona_names).length; i++) { 
    let persona_name = Object.keys(persona_names)[i];
    let left_walk_name = persona_name + "-left-walk";
    let right_walk_name = persona_name + "-right-walk";
    let down_walk_name = persona_name + "-down-walk";
    let up_walk_name = persona_name + "-up-walk";

    console.log(persona_name, left_walk_name, "DEUBG")
    anims.create({
    key: left_walk_name,
    frames: anims.generateFrameNames(persona_name, { prefix: "left-walk.", start: 0, end: 3, zeroPad: 3 }),
    frameRate: 4,
    repeat: -1 });

    anims.create({
    key: right_walk_name,
    frames: anims.generateFrameNames(persona_name, { prefix: "right-walk.", start: 0, end: 3, zeroPad: 3 }),
    frameRate: 4,
    repeat: -1 });

    anims.create({
    key: down_walk_name,
    frames: anims.generateFrameNames(persona_name, { prefix: "down-walk.", start: 0, end: 3, zeroPad: 3 }),
    frameRate: 4,
    repeat: -1 });

    anims.create({
    key: up_walk_name,
    frames: anims.generateFrameNames(persona_name, { prefix: "up-walk.", start: 0, end: 3, zeroPad: 3 }),
    frameRate: 4,
    repeat: -1 });
}

// *** MOVING PERSONAS ***
for (let i=0; i<Object.keys(personas).length; i++) {
    let curr_persona_name = Object.keys(personas)[i];
let curr_persona = personas[curr_persona_name];
let curr_pronunciatio = pronunciatios[Object.keys(personas)[i]];
let curr_speech_bubble = speech_bubbles[Object.keys(personas)[i]];

if (curr_persona_name.replace("_", " ") in all_movement[step]) {
    if (execute_count == execute_count_max) { 
        let curr_x = all_movement[step][curr_persona_name.replace("_", " ")]["movement"][0]
    let curr_y = all_movement[step][curr_persona_name.replace("_", " ")]["movement"][1]
        movement_target[curr_persona_name] = [curr_x * tile_width, 
                                            curr_y * tile_width]; 
    let pronunciatio_content = all_movement[step][curr_persona_name.replace("_", " ")]["pronunciatio"];
    let description_content = all_movement[step][curr_persona_name.replace("_", " ")]["description"];
    let chat_content_raw = all_movement[step][curr_persona_name.replace("_", " ")]["chat"];

    let chat_content = "";
    if (chat_content_raw != null ){
        for (let j=0; j<chat_content_raw.length; j++) { 
            chat_content += chat_content_raw[j][0] + ": " + chat_content_raw[j][1] + "<br>"
        }
        } else {
        chat_content = "<em>None at the moment</em>"
        }

    // This is what gives the pronunciatio balloon the name initials. We 
    // use regex to extract the initials of the personas. 
    // E.g., "Dolores Murphy" -> "DM"
    let rgx = new RegExp(/(\p{L}{1})\p{L}+/, 'gu');
    let initials = [...curr_persona_name.matchAll(rgx)] || [];
    initials = (
        (initials.shift()?.[1] || '') + (initials.pop()?.[1] || '')
    ).toUpperCase();
    pronunciatios[curr_persona_name].setText(initials + ": " + pronunciatio_content);

    // Updating the status of each personas 
    document.getElementById("quick_emoji-"+curr_persona_name).innerHTML = pronunciatio_content;
    document.getElementById("current_action__"+curr_persona_name).innerHTML = description_content.split("@")[0];
    document.getElementById("target_address__"+curr_persona_name).innerHTML = description_content.split("@")[1];
    document.getElementById("chat__"+curr_persona_name).innerHTML = chat_content;
    }

    if (execute_count > 0) {
        if (curr_persona.body.x < movement_target[curr_persona_name][0]) {
        curr_persona.body.x += movement_speed; 
        anims_direction = "r";
        pre_anims_direction = "r"
        pre_anims_direction_dict[curr_persona_name] = "r"
    } else if (curr_persona.body.x > movement_target[curr_persona_name][0]) {
        curr_persona.body.x -= movement_speed; 
        anims_direction = "l";
        pre_anims_direction = "l"
        pre_anims_direction_dict[curr_persona_name] = "l"
    } else if (curr_persona.body.y < movement_target[curr_persona_name][1]) {
        curr_persona.body.y += movement_speed;
        anims_direction = "d";
        pre_anims_direction = "d"
        pre_anims_direction_dict[curr_persona_name] = "d"
    } else if (curr_persona.body.y > movement_target[curr_persona_name][1]) {
        curr_persona.body.y -= movement_speed;
        anims_direction = "u";
        pre_anims_direction = "u"
        pre_anims_direction_dict[curr_persona_name] = "u"
    } else {
        anims_direction = "";
    }

    curr_pronunciatio.x = curr_persona.body.x + 18; 
    curr_pronunciatio.y = curr_persona.body.y - 42 - 25; // DEBUG 1 --- I added 32 offset on Dec 29. 
    curr_speech_bubble.x = curr_persona.body.x + 80; 
    curr_speech_bubble.y = curr_persona.body.y - 39;

    let left_walk_name = curr_persona_name + "-left-walk";
            let right_walk_name = curr_persona_name + "-right-walk";
            let down_walk_name = curr_persona_name + "-down-walk";
            let up_walk_name = curr_persona_name + "-up-walk";
    if (anims_direction == "l") {
            curr_persona.anims.play(left_walk_name, true);
        } else if (anims_direction == "r") {
            curr_persona.anims.play(right_walk_name, true);
        } else if (anims_direction == "u") {
            curr_persona.anims.play(up_walk_name, true);
        } else if (anims_direction == "d") {
            curr_persona.anims.play(down_walk_name, true);
        };
    }
} else { 
    curr_persona.anims.stop();
    // If we were moving, pick an idle frame to use
    if (pre_anims_direction_dict[curr_persona_name] == "l") curr_persona.setTexture(curr_persona_name, "left"); else
    if (pre_anims_direction_dict[curr_persona_name] == "r") curr_persona.setTexture(curr_persona_name, "right"); else
    if (pre_anims_direction_dict[curr_persona_name] == "u") curr_persona.setTexture(curr_persona_name, "up"); else
    if (pre_anims_direction_dict[curr_persona_name] == "d") curr_persona.setTexture(curr_persona_name, "down");
}
}

if (execute_count == 0) {
    for (let i=0; i<Object.keys(personas).length; i++) {
    let curr_persona_name = Object.keys(personas)[i]
    let curr_persona = personas[curr_persona_name];
    curr_persona.body.x = movement_target[curr_persona_name][0];
    curr_persona.body.y = movement_target[curr_persona_name][1];
}
    execute_count = execute_count_max + 1;
step = step + 1;

start_datetime = new Date(start_datetime.getTime() + step_size);
    document.getElementById("game-time-content").innerHTML = start_datetime.toLocaleTimeString("en-US", datetime_options);
}

execute_count -= 1;
```

http-server 文件路径 -p 8888 -o

/browser-sync/browser-sync-client.js?v=2.27.5

# gulp browser-sync
npm install gulp browser-sync --save-dev

在项目根目录下创建 gulpfile.js 文件，并添加以下代码：
``` javascript
const gulp = require('gulp');
const browserSync = require('browser-sync').create();

// 静态服务器 + 监听 scss/html 文件
gulp.task('serve', function() {

    browserSync.init({
        server: "./"
    });

    gulp.watch("*.scss", ['sass']);
    gulp.watch("*.html").on('change', browserSync.reload);
});

// 编译 sass
gulp.task('sass', function() {
    return gulp.src("*.scss")
        .pipe(sass())
        .pipe(gulp.dest("./css"))
        .pipe(browserSync.stream());
});

gulp.task('default', ['serve']);
```

上面的代码中，我们定义了两个任务：

1. 使用 BrowserSync 启动一个静态服务器，并监听 scss 和 html 文件的变化。
2. 编译 sass 文件，将其输出到 css 目录，并使用 browserSync.stream() 实现实时刷新。
3. 在命令行中输入以下命令启动 Gulp：
```javascript
gulp
```
现在，你可以在浏览器中访问 http://localhost:3000 查看你的网站，并进行实时预览和自动刷新。

```javascript
var gulp = require('gulp');
var browserSync = require('browser-sync').create();

// 静态服务器
gulp.task('server', function() {
    browserSync.init({
        server: {
            baseDir: "./"
        }
    });
    
    // 监听 html 文件变化
    gulp.watch("*.html").on('change', browserSync.reload);
    
    // 监听 css 文件变化
    gulp.watch("css/*.css").on('change', browserSync.reload);
    
    // 监听 js 文件变化
    gulp.watch("js/*.js").on('change', browserSync.reload);
});
```

