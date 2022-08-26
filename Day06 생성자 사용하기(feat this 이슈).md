# Day06 생성자 사용하기(feat. this 이슈)

# 랜덤위치,랜덤색상 Ball 생성

```jsx
/*
문제 ::
	공을 만드는 Object를 통해 화면에 random한 색상과 좌표를 가진 Ball들을 출력하여라
*/

let ball; // array of Jitter objects
let balls=[];
let i;
function setup() {
  createCanvas(710, 400);
  for(i=0;i<100;i++){
    balls[i] = new Ball();
    
  }
}

function draw() {
  background(50, 89, 100);
  for(i=0;i<100;i++){
    balls[i].display();
  }
}

// Jitter class
class Ball {
  constructor() {
    this.x = random(width);
    this.y = random(height);
    this.diameter = random(20, 150);
    //this.speed = 1;
    
    this.r = random(0,255);
    this.g = random(0,255);
    this.b = random(0,255);
  }
  display() {
    
    /*fill(random(0,125),random(0,125),random(0,125));
        -> draw()의 for-loop를 돌면서 
           display()를 호출할 때마다 random()이 계속 호출되어
           계속 색상이 바뀌게 된다.
    */
  
```

위 코드에서 생성자에 대해서 알아보자.

```jsx
class Ball {
  constructor() {
    this.x = random(width);
    this.y = random(height);
    this.diameter = random(20, 150);
    //this.speed = 1;
    
    this.r = random(0,255);
    this.g = random(0,255);
    this.b = random(0,255);
  }
  display() {
    
    /*fill(random(0,125),random(0,125),random(0,125));
        -> draw()의 for-loop를 돌면서 
           display()를 호출할 때마다 random()이 계속 호출되어
           계속 색상이 바뀌게 된다.
    */
    fill(this.r,this.g,this.b);
    ellipse(this.x, this.y, this.diameter, this.diameter);
  }
}
```

위 생성자에서 this가 아닌 let 혹은 var를 쓰게 되면 setup(),draw()에서 선언한 객체와 바인딩을 하지 못 한다...

아래를 참조해보자 ([여기를](https://wormwlrm.github.io/2019/03/04/You-should-know-JavaScript-this.html.html) 참조했다.)

### **생성자**

함수 앞에 `new` 키워드가 붙이고 선언할 때, `this`를 해당 객체에 바인딩합니다.

```jsx
// 1. 클래스 역할을 할 함수 선언
function Man () {
    this.name = 'John';
}

// 2. 생성자로 객체 선언
var john = new Man();

// 3. this가 Man 객체를 가리키고 있어 이름이 정상적으로 출력된다
john.name; // => 'John'
```