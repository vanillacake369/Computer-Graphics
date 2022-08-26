# Code Analysis : Terrain using PerlinNoise

# Contents

# 📑Code

```jsx
int cols=0, rows=0;
// col,row : value of vertex
// for drawing mash
int scl=10;
// scale of terrain
int w =2000;
int h =1600;
// width and height

float flying = 0;
float [][] terrain;

void setup() {
  size(600, 600, P3D);
  cols=w/scl;
  rows=h/scl;
  terrain = new float[cols][rows];
  float yoff = 0;

  for (int y=0; y<rows; y++) {
    float xoff=0;
    for (int x=0; x<cols; x++) {
      terrain[x][y]=map(noise(xoff, yoff), 0, 1, -50, 50);
      xoff+=0.2;
    }
    yoff+=0.2;
  }
}

void draw() {

  flying -= 0.01;
  float yoff = flying;
  for (int y=0; y<rows; y++) {
    float xoff = 0;
    for (int x=0; x<cols; x++) {
      terrain[x][y]=map(noise(xoff, yoff), 0, 1, -50, 50);
      xoff+=0.2;
    }
    yoff+=0.2;
  }

  background(0);
  stroke(255);
  noFill();

  translate(width/2, height/2);
  rotateX(PI/3);
  translate(-w/2, -h/2);

  /*
    draw vertex using x,y,z
   x : x*scl -> x*scl
   y : y*scl -> (y+1)*scl
   z : terrain[x][y] -> terrain[x][y+1]
   */
  for (int y=0; y<rows-1; y++) {
    beginShape(QUADS);
    for (int x=0; x<cols; x++) {
      vertex(x*scl, y*scl, terrain[x][y]);
      vertex(x*scl, (y+1)*scl, terrain[x][y+1]);
      //rect(x*scl,y*scl,scl,scl);
    }
    endShape();
  }
}
```

---

# ⚙️주요 동작 구현 설명

## 📑Code

```jsx
void setup(){
...
	for (int y=0; y<rows; y++) {
	  float xoff=0;
	  for (int x=0; x<cols; x++) {
	    terrain[x][y]=map(noise(xoff, yoff), 0, 1, -50, 50);
	    xoff+=0.2;
	  }
	  yoff+=0.2;
	}
...
}
```

```jsx
void draw() {
...
  for (int y=0; y<rows-1; y++) {
    beginShape(QUADS);
    for (int x=0; x<cols; x++) {
      vertex(x*scl, y*scl, terrain[x][y]);
      vertex(x*scl, (y+1)*scl, terrain[x][y+1]);
      //rect(x*scl,y*scl,scl,scl);
    }
    endShape();
  }
...
}
```

## 💁어떻게 동작하는 것인가?

map과 noise 메서드를 통해 terrain 배열의 값을 정함

## 📖사용된 메서드와 변수

- map(value, start1, stop1, start2, stop2, [withinBounds])
    
    : mapping 'value' from low bound to high bound
    

- noise
    
    : returns the Perlin noise value at specified coordinates(x,y)
       
    
- xoff,yoff
: noise의 값을 조정

---

# 🔨변경된 부분

- `background(0)`; 을 통해 배경을 검정으로
- `stroke(0,255,0);` 을 통해 초록색 선으로
- `noFill();` 을 통해 채움색이 없게
- `beginShape(QUADS)`를 통해 사각형 가닥들로
- `xoff`와 `yoff`, 그리고 `flying`을 보다 적은 수로 하여 느릿느릿하게

---

# 🗨️소감

보내주신 유튜브 영상을 통해 하나하나 같이 코딩을 해나가며 모르는 부분은 p5js 레퍼런스를 뒤져봤다. 

생각보다 많은 공부가 되었다.

1. 한 번에 모든 것을 코딩하는 것을 지양해야겠다.

물론 해당 프로젝트의 내용을 설명하기 위해 단계별로 만들어나갔으리라 생각되지만, 실제 따라 치고 혼자 만들어 볼 때에는 영상처럼 어떤 것을 어떻게 구현할지를 정립해야했다.

물론 제작자는 이미 해봐서 하나하나 어떻게 해나가는지 알지만, 설계 과정 속에서 어떤 것을 구현할 것인지 인지,인식을 하고 그것을 논리적으로 표현하는 것이 첫 번째 단추이다. 

일괄적 코딩보다 논리적 접근을 통해 부분적 코딩이 굉장히 편리하고 중요한 것임을 영상을 보며 배웠다. 

1. 모르는 것은 docs에서

생각보다 API가 지원해주는 메서드들이 많았고 생소했다. p5js docs의 굉장한 장점이 바로 한글로 번역이 되어있다는 것이다. 물론 API의 순기능을 한글로 다 담아내기란 불가능한 작업이지만 한글이란 생소한 언어를 지원해주는 것은 굉장히 놀라운 일이다.

덕분에 `beginShape`나 `rotateX`와 같은 잘 이해가 되지 않는 메소드들을 쉽게 찾고 이해할 수 있었다.

1. 이제는 코딩도 유튜브로..

보통은 책을 사거나 강의를 사서 공부를 하곤 했는데 제작자가 나와서 설명하는 영상은 굉장히 놀랍고 새로웠다. 최근들어서 더더욱 코딩의 진입장벽이 많이 낮아지고 있음을 몸소 느끼고 있다.

---

# Reference

[https://www.youtube.com/watch?v=IKB1hWWedMk](https://www.youtube.com/watch?v=IKB1hWWedMk)

[https://p5js.org/ko/reference](https://p5js.org/ko/reference)