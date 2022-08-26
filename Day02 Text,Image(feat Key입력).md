# Day02 Text,Image(feat.Key입력)

# 목차

# 들어가기 앞서..

processing 자체가 **절차지향**이므로 **속성들 혹은 설정들을 먼저 선언해놓고** 그려야 의도한 결과물을 출력할 수 있다고 하셨다.

( 물론 나중에 먼저 그려놓고 element들의 속성이나 위치들을 동적으로 조정할 수 있다고 하셨다 )

# Text

## Tutorial

```java
▷ 글자 출력
void draw(){
	text("andong", 0, 50); // 문자 출력
}
```

---

## text()의 원형

### Syntax

- `text(c, x, y)`
- `text(c, x, y, z)`
- `text(str, x, y)`
- `text(chars, start, stop, x, y)`
- `text(str, x, y, z)`
- `text(chars, start, stop, x, y, z)`
- `text(str, x1, y1, x2, y2)`
- `text(num, x, y)`
- `text(num, x, y, z)`

### Parameters

- **`c**(char)`the alphanumeric character to be displayed
- **`x**(float)`x-coordinate of text
- **`y**(float)`y-coordinate of text
- **`z**(float)`z-coordinate of text
- **`chars**(char[])`the alphanumeric symbols to be displayed
- **`start**(int)`array index at which to start writing characters
- **`stop**(int)`array index at which to stop writing characters
- **`x1**(float)`by default, the x-coordinate of text, see rectMode() for more info
- **`y1**(float)`by default, the y-coordinate of text, see rectMode() for more info
- **`x2**(float)`by default, the width of the text box, see rectMode() for more info
- **`y2**(float)`by default, the height of the text box, see rectMode() for more info
- **`num**(float, int)`the numeric value to be displayed

---

## Sizing

```java
▷ 크기 조절
void setup(){
	size(800, 300);
	textSize(200); // 폰트 크기
	text("andong", 0, height/2);
}
```

> 주의해야할 점
: 어떤 함수를 선언 혹은 사용할 때 draw()에서인지, setup()인지 헷갈린다.
꼭 외울 수 있다면 외우라 하셨다.
> 

---

## Move Text

```java
▷  이동하기

void setup() { // 한번 실행
	size(800, 300);
	textSize(200);
}

int i; // 정수 변수
void draw() { // 반복 됨
  text("andong", i++, 200);
}
```

draw()에서 text()이든 ellipse()이든 어떤 것을 화면에 그리고자 할 때, 그것의 위치값에 매개변수를 집어넣어 동적으로 움직이게 만들 수 있다.

> Called directly after setup() **,** the **draw() function continuously executes the lines of code contained inside its block until the program is stopped or noLoop()**  is called.

*- https://processing.org/reference*
> 

draw() 내부의 코드의 반복성을 제어하는 함수가 따로 정해져있는데 noLoop(), redraw(), loop()이다.

(이건 너무 tmi이므로 그만 알아보자..^^)

---

# Image

## PImage타잎

### 정의

Datatype for storing images. Processing can display **.gif**, **.jpg**, **.tga**, and **.png** images.

image를 사용하기 위해서는 `loadImage()`함수를 통해 load해야 한다.
`PImage 타잎`은 이미지의 width와 height 값을 필드값으로 가지며, 이미지의 모든 pixel값이 저장되어있는 `pixel[]` 배열 또한 저장되어있다.

### 예제코드

```java
PImage photo;

voidsetup() {
  size(400, 400);
  photo = loadImage("Toyokawa-city.jpg");
}

voiddraw() {
  image(photo, 0, 0);
}
```

---

## loadImage()

### 정의

Loads an image into a variable of type **PImage**. 

Four types of images ( **.gif**, **.jpg**, **.tga**, **.png**) images may be loaded. 

To load correctly, images must be located in the data directory of the current sketch.

대부분의 경우 setup() 안에서 load하기 때문에 **해당 함수는 setup() 안에서 사용해주는 것이 좋다.**

## imageMode()

이미지의 위치를 조정할 수 있게끔한다.

예제코드를 살펴보자.

```java
PImage img;

void setup() {
  size(400,400);
  img = loadImage("Toyokawa.jpg");
}

void draw() {
  imageMode(CORNER);
  image(img, 40, 40, 200, 200);  // Draw image using CORNER mode
}
```

## translate()이용하기

Specifies an amount to displace objects within the display window.

** displace :

take over the place, position, or role of (someone or something).

- cause (something) to move from its proper or usual
- force (someone) to leave their home, typically because of war, persecution, or natural disaster.
    
    "thousands of people have been displaced by the civil war"
    

** translate :

1. express the sense of (words or text) in another 
2. move from one place or condition to another

즉, 선언한 객체(도형)들을 x,y,z축으로 움직이게끔 하는 함수이다. 

주의해야할 점이 존재한다.

위치정보를 먼저 줘야지만 원하는 위치에 도형을 그릴 수 있다. 따라서 **도형을 그리기 전에 앞서 선언해준다.**

<aside>
💡 Tip
: x,y 인자에 pmouseX,pmouseY를 사용하면 마우스를 따라 도형을 그리게 할 수 있다.

</aside>

```java
size(400, 400);
translate(pmouseX,pmouseY);
rect(0, 0, 220, 220);
```

# 예제코드

## #1 Text 벽에 부딪히면 왔다갔다 하게 만들기

```java
void setup() {
  size(800, 300);
  textSize(200);
}

int i, dir=1;
void draw() {
  fill(0);
  background(255,255,0);
  text("andong", i, 200);
  if(i>width) dir=-1;
  if(i<0) dir=1;
  i = i+dir;
}
```

- `text(str, x, y)` 을 통해 그린다.
- 오른쪽 벽에 닿은 경우
: width보다 x값이 커지면 x+dir연산 시의 dir값을 -1로 하여 왼쪽으로 움직이게 만든다.
- 왼쪽 벽에 닿은 경우
: 0보다 x값이 작아지게 되면 x+dir연산 시의 dir값을 +1로 하여 오른쪽으로 움직이게 만든다.

## #2 Text 움직이는 속도 키보드 입력으로 조작하기

```java
PFont f;
void setup() {
  size(800,400);
  textSize(100);
}

int i,direction=1,speed=1;

void draw() {
  fill(0);
  background(0,255,0);
  text("keypress",i,200);
  i=i+direction*speed;
  if(i>width) direction=-1;
  if(i<0) direction=+1;
}
void keyPressed(){
 speed= key-'0';  
 /* 
 when key pressed, the value of key input is ASCII code, 
 which contains digit value + 48.
 48(digit) in binary is 0.
 therefore, key - '0' intends to specify the digit value through binary key input.
 */
}
```

- text를 그린다.
- text의 x값인 i에 i+direction*speed을 삽입한다.
- direction 세팅
: width보다 x값이 커지면 x+dir연산 시의 dir값을 -1로 하여 왼쪽으로 움직이게 만든다.
    
    : 0보다 x값이 작아지게 되면 x+dir연산 시의 dir값을 +1로 하여 오른쪽으로 움직이게 만든다.
    
- speed 세팅
    
    : keyPressed함수를 사용한다.
    
    <aside>
    💡 주의!
    : Processing에서 key를 눌렀을 때 입력되는 값 모두 char값으로 ASCII코드값이다. 
    따라서 우리가 의도한 0~9값은 실제로 char값이다. 즉, 실제 digit값과 48차이가 난다. 
    우리가 입력한 키값에서 ‘0’(char타잎)을 빼주게 되면 우리가 의도한 digit 숫자가 나오게 된다.
    
    Ex)
    5 입력 
    → ‘5’ == 53(dec) 
    → ‘5’ - ‘0’ == 53(dec) - 48(dec) == 5
    → 의도대로 5 입력됨.
    
    </aside>
    
    ![Untitled](Day02%20Text,Image(feat%20Key%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%85%E1%85%A7%E1%86%A8)%20cb26b58c2f3e4231bdaf87ff81b66b82/Untitled.png)