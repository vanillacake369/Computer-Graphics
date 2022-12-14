# Code Analysis : Terrain using PerlinNoise

# Contents

# πCode

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

# βοΈμ£Όμ λμ κ΅¬ν μ€λͺ

## πCode

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

## πμ΄λ»κ² λμνλ κ²μΈκ°?

mapκ³Ό noise λ©μλλ₯Ό ν΅ν΄ terrain λ°°μ΄μ κ°μ μ ν¨

## πμ¬μ©λ λ©μλμ λ³μ

- map(value, start1, stop1, start2, stop2, [withinBounds])
    
    : mapping 'value' from low bound to high bound
    

- noise
    
    : returns the Perlin noise value at specified coordinates(x,y)
       
    
- xoff,yoff
: noiseμ κ°μ μ‘°μ 

---

# π¨λ³κ²½λ λΆλΆ

- `background(0)`; μ ν΅ν΄ λ°°κ²½μ κ²μ μΌλ‘
- `stroke(0,255,0);` μ ν΅ν΄ μ΄λ‘μ μ μΌλ‘
- `noFill();` μ ν΅ν΄ μ±μμμ΄ μκ²
- `beginShape(QUADS)`λ₯Ό ν΅ν΄ μ¬κ°ν κ°λ₯λ€λ‘
- `xoff`μ `yoff`, κ·Έλ¦¬κ³  `flying`μ λ³΄λ€ μ μ μλ‘ νμ¬ λλ¦Ώλλ¦Ώνκ²

---

# π¨οΈμκ°

λ³΄λ΄μ£Όμ  μ νλΈ μμμ ν΅ν΄ νλνλ κ°μ΄ μ½λ©μ ν΄λκ°λ©° λͺ¨λ₯΄λ λΆλΆμ p5js λ νΌλ°μ€λ₯Ό λ€μ Έλ΄€λ€. 

μκ°λ³΄λ€ λ§μ κ³΅λΆκ° λμλ€.

1. ν λ²μ λͺ¨λ  κ²μ μ½λ©νλ κ²μ μ§μν΄μΌκ² λ€.

λ¬Όλ‘  ν΄λΉ νλ‘μ νΈμ λ΄μ©μ μ€λͺνκΈ° μν΄ λ¨κ³λ³λ‘ λ§λ€μ΄λκ°μΌλ¦¬λΌ μκ°λμ§λ§, μ€μ  λ°λΌ μΉκ³  νΌμ λ§λ€μ΄ λ³Ό λμλ μμμ²λΌ μ΄λ€ κ²μ μ΄λ»κ² κ΅¬νν μ§λ₯Ό μ λ¦½ν΄μΌνλ€.

λ¬Όλ‘  μ μμλ μ΄λ―Έ ν΄λ΄μ νλνλ μ΄λ»κ² ν΄λκ°λμ§ μμ§λ§, μ€κ³ κ³Όμ  μμμ μ΄λ€ κ²μ κ΅¬νν  κ²μΈμ§ μΈμ§,μΈμμ νκ³  κ·Έκ²μ λΌλ¦¬μ μΌλ‘ νννλ κ²μ΄ μ²« λ²μ§Έ λ¨μΆμ΄λ€. 

μΌκ΄μ  μ½λ©λ³΄λ€ λΌλ¦¬μ  μ κ·Όμ ν΅ν΄ λΆλΆμ  μ½λ©μ΄ κ΅μ₯ν νΈλ¦¬νκ³  μ€μν κ²μμ μμμ λ³΄λ©° λ°°μ λ€. 

1. λͺ¨λ₯΄λ κ²μ docsμμ

μκ°λ³΄λ€ APIκ° μ§μν΄μ£Όλ λ©μλλ€μ΄ λ§μκ³  μμνλ€. p5js docsμ κ΅μ₯ν μ₯μ μ΄ λ°λ‘ νκΈλ‘ λ²μ­μ΄ λμ΄μλ€λ κ²μ΄λ€. λ¬Όλ‘  APIμ μκΈ°λ₯μ νκΈλ‘ λ€ λ΄μλ΄κΈ°λ λΆκ°λ₯ν μμμ΄μ§λ§ νκΈμ΄λ μμν μΈμ΄λ₯Ό μ§μν΄μ£Όλ κ²μ κ΅μ₯ν λλΌμ΄ μΌμ΄λ€.

λλΆμ `beginShape`λ `rotateX`μ κ°μ μ μ΄ν΄κ° λμ§ μλ λ©μλλ€μ μ½κ² μ°Ύκ³  μ΄ν΄ν  μ μμλ€.

1. μ΄μ λ μ½λ©λ μ νλΈλ‘..

λ³΄ν΅μ μ±μ μ¬κ±°λ κ°μλ₯Ό μ¬μ κ³΅λΆλ₯Ό νκ³€ νλλ° μ μμκ° λμμ μ€λͺνλ μμμ κ΅μ₯ν λλκ³  μλ‘μ λ€. μ΅κ·Όλ€μ΄μ λλμ± μ½λ©μ μ§μμ₯λ²½μ΄ λ§μ΄ λ?μμ§κ³  μμμ λͺΈμ λλΌκ³  μλ€.

---

# Reference

[https://www.youtube.com/watch?v=IKB1hWWedMk](https://www.youtube.com/watch?v=IKB1hWWedMk)

[https://p5js.org/ko/reference](https://p5js.org/ko/reference)