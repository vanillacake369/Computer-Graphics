# Code Modification : Terrain using PerlinNoise

# Contents

# 서론

---

  기존 강좌의 terrain 모델은 사용자의 추가 입력의 제한으로 단순 사용자로 하여금 지루함과 따분함을 느낄 수 있다. 또 시각적인 효과가 떨어져 사용자의 눈을 사로잡지 못 한다.

이를 보완하기 위해 사용자가 화면을 확대 혹은 축소할 수 있게 하였고, 시야각을 위에서 볼 지 아래에서 볼 수 있게 하였다. 또한 마우스를 움직이며 terrain 지형 위를 밝힐 수 있게 하였다. 추가로 사각형으로 이루어진 망 구조인 terrain에 다양한 시각효과를 입혀주었다.

# 기획

---

## 분할작업

---

 총 4명의 구성원으로서 두 팀으로 분할하여 작업하였다. 안정흠,임지훈이 책임자가 되어 각 팀에 따른 구현부분을 나누고 Git을 통해 통합 및 수정하는 작업과정을 거쳤다.

## Team

---

- Team A 
: 안정흠*, 전성민, 최혜령
- Team B
    
    : 임지훈*, 이영훈
    

## Work

---

Team A

→  **지형 위를 밝히기**

→  **terrain 시각효과 입히기**

Team B

→ 사**용자의 입력값을 통한 terrain모델 확대/축소,시야각 변경**

# Code

---

```jsx
var cols, rows;
var scl = 20;
var w = 1400;
var h = 1000;

var flying = 0;

var terrain = [];

var zoom = 0;
var angle = 0;

function setup() {
  createCanvas(600, 600, WEBGL);
  cols = w / scl;
  rows = h / scl;

  zoom = createSlider(470,950,470);
  angle = createSlider(-400,200,-100);

  for (var x = 0; x < cols; x++) {
    terrain[x] = [];
    for (var y = 0; y < rows; y++) {
      terrain[x][y] = 0; //specify a default value for now
    }
  }
}

function draw() {
  flying -= 0.1;
  var yoff = flying;
  for (var y = 0; y < rows; y++) {
    var xoff = 0;
    for (var x = 0; x < cols; x++) {
      terrain[x][y] = map(noise(xoff, yoff), 0, 1, -100, 100);
      xoff += 0.2;
    }
    yoff += 0.2;
  }
  camera(0, angle.value(), zoom.value(), 0, 0, 0, 0, 1, 0);

  ambientLight(50); 
  pointLight(255, 255, 255, mouseX-350, -150, mouseY-350); //mouseX, Y mapping
  specularMaterial(250); //reflection Color
  shininess(10); //Sets the amount of gloss in the surface of shapes.
	// specularMaterial()과 shininess()는 같이 쓰인다.

  background(50, 50, 80);
  translate(0, 50);
  rotateX(PI / 3);
  fill(200, 200, 200, 150);
  noStroke();
  translate(-w / 2, -h / 2);
  
  for (var y = 0; y < rows - 1; y++) {
    beginShape(TRIANGLE_STRIP);
    for (var x = 0; x < cols; x++) {
      ambientMaterial(100, 200, 100);
      vertex(x * scl, y * scl, terrain[x][y]);
      vertex(x * scl, (y + 1) * scl, terrain[x][y + 1]);
    }
    endShape();
  }
}
```

# 소감

---

> **이영훈**
카메라값을 슬라이더를 통해 조절하면서 value를 필요로 하는 다른 코드(색상, 크기 등등) 들도 창의적이게 응용하면 다른 멋진 작품들이 나올 것 같아서 흥미로웠고 기회가 된다면 다른 기능들도 추가해 보고 싶었습니다.
> 

> **안정흠**
원하는 기능을 만들기 위해 배웠던 것도 다시보고 레퍼런스를 찾아보며 공부를 하니 더욱 머릿속에 깊이 남게 되었습니다.
앞으로도 수업이 끝나고 기능을 추가하는 식의 스스로 학습을 통해 완벽히 숙지를 해야겠단 생각이 들었고
기능을 만들어서 정상적으로 동작하는 모습을 보니 정말 뿌듯했습니다
> 

> **임지훈**
개인적으로 이번 프로젝트를 통해 여러 방면으로 다양한 경험과 교훈을 배웠습니다.
첫 째, terrain모델에 대한 이해도와 map(),noise()에 대해 더욱 잘 알게되었습니다.
두 번째, git을 통해 여러 학우들과 협업을 하며 의견조율을 하는 과정을 배울 수 있었습니다.
세 번째, camera(),ambientLight(),specularMaterial()과 같은 더욱 다양한 함수들을 익힐 수 있었습니다.
네 번쨰, p5js 공식 레퍼런스 홈페이지를 통해 공부하는 방법을 익혔습니다.
다섯 번째, 보고서를 작성하는 방법을 선배들과 주변 학우들의 조언을 통해 익혔습니다.
위와 같은 경험을 만들게 해주신 심재창교수님께 감사드립니다.
> 

> **최혜령**
3차원 지형을 여러방면으로 활용하는 과제를 처음 혼자하면서도 어려움을 많이 겪어 교수님께서 알려주신 방향으로 밖에 수정하지 못하였었는데, 아직 모자람이 많아 팀플 과제를 보고 정말 막막했는데 좋은 조원들을 만나 가르침을 많이 받으면서 코드를 더 세세하게 이해할 수 있는 시간이 된 것 같아 좋았습니다.
> 

> **전성민**
이번 학기에서 처음으로 팀플을 진행하였는데 제가 부족함이많아 팀에 도움이될까라는 생각을 많이했습니다. 하지만 저희 팀장인 안정흠학우가 많이 도와주고 알려주어서 최선을다해서 팀플에 임하였습니다. 팀플을진행하면서 같이 공부도하고 의견도 내고 도움이 많이되었습니다. 이런기회를 만들어 주셔서 감사합니다.
> 

# Reference

---

- terrain model using p5js by Daniel Shiffman

[https://editor.p5js.org/codingtrain/sketches/OPYPc4ueq](https://editor.p5js.org/codingtrain/sketches/OPYPc4ueq)

- createSlider()

[https://p5js.org/reference/#/p5/createSlider](https://p5js.org/reference/#/p5/createSlider)

- map()

[https://p5js.org/reference/#/p5/map](https://p5js.org/reference/#/p5/map)

- noise()

[https://p5js.org/reference/#/p5/noise](https://p5js.org/reference/#/p5/noise)

- camera()

[https://p5js.org/reference/#/p5/camera](https://p5js.org/reference/#/p5/camera)

- ambientLight()

[https://p5js.org/reference/#/p5/ambientLight](https://p5js.org/reference/#/p5/ambientLight)

- pointLight()

[https://p5js.org/reference/#/p5/pointLight](https://p5js.org/reference/#/p5/pointLight)

- specularMaterial()

[https://p5js.org/reference/#/p5/specularMaterial](https://p5js.org/reference/#/p5/specularMaterial)

- shineiness

[https://p5js.org/reference/#/p5/shininess](https://p5js.org/reference/#/p5/shininess)

- ambientModel()

[https://p5js.org/reference/#/p5/ambientMaterial](https://p5js.org/reference/#/p5/ambientMaterial)