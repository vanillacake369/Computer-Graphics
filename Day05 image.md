# Day05 image

# Contents

---

# Image 선언&호출

```jsx
let img;
function preload() {
  img = loadImage('https://pbs.twimg.com/profile_images/1150112534/jcshim640_480l_400x400.jpg');
}
function setup() {
  createCanvas(400,400);
  image(img, 0, 0);
}
```

*교수님 : 외우세요~~ ^^*

## 📑 Method 설명

### translate(x좌표,y좌표)

화면 내에서 객체를 이동시킬 정도를 지정

→ 좌표에 mouseX,mouseY를 지정하여 마우스를 따라 움직이게 끔 할 수 있음

### function preload() { .... }

setup() 함수 직전에 호출되며, 외부 파일의 비동기 불러오기를 차단하기 위해 사용됩니다.  preload() 함수로 외부 파일 사전 불러오기가 설정되면, setup() 함수는 불러오기 호출이 완료될 때까지 대기합니다.

## ⚙️ 프로그램 작동순서

- preload를 통해 image를 불러오고
- setup을 통해 화면을 구성하고
- draw를 통해 그래픽을 그린다.

# Example Code : 마우스를 따라가며 회전하는 이미지

```jsx
let img;
function preload() {
  img = loadImage('https://pbs.twimg.com/profile_images/1150112534/jcshim640_480l_400x400.jpg');
}
function setup() {
  createCanvas(400,400);
}
lef f = 0;
function draw(){
	translate(mouseX,mouseY);
	rotate(f); f = f + 0.05;
	image(img,-40,-40,80,80);
	// -40,-40을 통해 이미지 중앙이 마우스를 따라가게끔 
	//   cf)0,0을 지정하면 이미지 좌측상단이 마우스를 따라감
}
```

---

# 도형 그리기

## vertax() : 꼭짓점 좌표 지정

모든 도형들은 꼭짓점 연결을 통해 구축됩니다. vertex() 함수를 사용하여 점, 선, 삼각형, 사각형, 그리고 다각형의 꼭짓점 좌표를 지정할 수 있습니다.는 데에 쓰입니다. 이 때, vertex() 함수는 반드시 
beginShape()//Shape">endShape() 함수 사이에 작성되어야합니다.

```jsx
strokeWeight(3);
beginShape(POINTS);
vertex(30, 20);
vertex(85, 20);
vertex(85, 75);
vertex(30, 75);
endShape();
```

## beginShape() : 꼭짓점 좌표 연결하여 도형그리기

beingShape()은 도형의 꼭짓점을 기록하기 시작하고, endShape() 은 그 기록을 중지합니다.

함수의 매개변수를 통해 꼭짓점으로 어떤 도형을 그릴지 결정할 수 있습니다. 

LINES - 여려 개의 분리 된 선들을 그립니다.

TRIANGLES - 여러 개의 분리 된 삼각형들을 그립니다.

TRIANGLE_FAN - 여러 개의 연결 된 삼각형들을 그립니다. 이 삼각형들은 첫 꼭짓점을 공통적으로 하며 부채 모양으로 그려집니다.

TRIANGLE_STRIP - 여러 개의 연결 된 삼각형들을 그립니다. 이 삼각형들은 한 줄로 그려집니다.

QUADS - 여러 개의 분리 된 사각형들을 그립니다.

QUAD_STRIP - 여러 개의 연결 된 사각형들을 한 줄로 그립니다.

TESS (WebGl만 가능) - 모자이크 세공 (tessellation)을 위한 불규칙적 도형을 그립니다.

+) endshape() 안에 CLOSE를 넣어주면 모든 점에 대해 선을 다 그어준다.

![Untitled](Day05%20image%205663b67ad8e84c9581550bb9db5caf30/Untitled.png)

```jsx
beginShape();
vertex(30, 20);
vertex(85, 20);
vertex(85, 75);
vertex(30, 75);
endShape(CLOSE);
```

![Untitled](Day05%20image%205663b67ad8e84c9581550bb9db5caf30/Untitled%201.png)

beginShape(TRIANGLE_STRIP);
vertex(30, 75);
vertex(40, 20);
vertex(50, 75);
vertex(60, 20);
vertex(70, 75);
vertex(80, 20);
vertex(90, 75);
endShape();

---

# Noise

지정된 좌표에서의 펄린 노이즈(Perlin noise)값을 반환

그래픽 응용 프로그램상 절차적 텍스처, 자연스러운 모션, 형상, 지형 등을 생성하는 데에 사용됩니다.

![Untitled](Day05%20image%205663b67ad8e84c9581550bb9db5caf30/Untitled%202.png)

```jsx
vertical line moves left to right with updating noise values.
noise example 0
let xoff = 0.0;

function draw() {
  background(204);
  xoff = xoff + 0.01;
  let n = noise(xoff) * width;
  line(n, 0, n, height);
}
```

---

# Reference

- Preload

[https://p5js.org/ko/reference/#/p5/preload](https://p5js.org/ko/reference/#/p5/preload)

- translate()

[https://p5js.org/ko/reference/#/p5/translate](https://p5js.org/ko/reference/#/p5/translate)

- Image

[https://p5js.org/ko/reference/#/p5/loadImage](https://p5js.org/ko/reference/#/p5/loadImage)

- beginShape

[https://p5js.org/ko/reference/#/p5/beginShape](https://p5js.org/ko/reference/#/p5/beginShape)

- vertax

[https://p5js.org/ko/reference/#/p5/vertex](https://p5js.org/ko/reference/#/p5/vertex)

- noise

[https://p5js.org/ko/reference/#/p5/noise](https://p5js.org/ko/reference/#/p5/noise)