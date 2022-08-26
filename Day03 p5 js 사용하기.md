# Day03 p5.js 사용하기

# 목차

# 복습

## p5.js

[https://editor.p5js.org/](https://editor.p5js.org/) 에서 sketch.js로 javascript를 통한 그래픽스를 그려줄 수 있다.

### index.html

p5.js를 사용하게 되면 sketch파일과 html파일이 생성된다.

### Example Code

```jsx
function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(220);
}
```

### createCanvas()

size()와 같은 함수이다.

## image 사용하기

```java
PImage img;
void setup() {
  size(400, 400);
  img = loadImage("C:\\Users\\admin\\Desktop\\tree.jpg");
  imageMode(CENTER);
}
float f=0;
void draw() {
  translate(mouseX,mouseY);
  rotate(f);
  //f=f+0.05;
  image(img,0,0,width/4,height>>2);
}
```

# 캐릭터 그리기

```java
void frog() {
  translate(50, 50);
  fill(0, 255, 0);
  ellipse(0, 0, 80, 60);
  fill(165, 50, 200);
  ellipse(-20, -30, 32, 32);
  ellipse(+20, -30, 32, 32);
  fill(200, 150, 30);
  ellipse(-20, -30, 5, 5);
  ellipse(+20, -30, 5, 5);
  fill(255, 0, 0);
  arc(0, 0, 60, 30, 0, PI);
}
void setup() {
  size(500, 500);
}
void draw() {
  frog();
}
```