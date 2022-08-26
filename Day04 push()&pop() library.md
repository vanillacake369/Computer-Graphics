# Day04 push()&pop() / library

# Contents

# push() & pop()

## 정의

그래픽스에서의 push() pop()은 자료를 넣고 꺼내는 것이 아니다.

- 그래픽에 대한 설정을 push()하여 저장해두었다가
- 해당 설정을 pop()하여 삭제함으로서 다음 그래픽에 대해 지장이 안 가게끔

하는 것이다.

예제를 살펴보자.

## Example Code

### Before push&pop

![Untitled](Day04%20push()&pop()%20library%20e7e4f6c88e1d41dd967c85b6460d32fc/Untitled.png)

After push&pop

![Untitled](Day04%20push()&pop()%20library%20e7e4f6c88e1d41dd967c85b6460d32fc/Untitled%201.png)

# p5.js에서 펜 만들기

<aside>
📖 1. 화면 가득
2. 푸른색
3. 굵기는 8
4. 마우스가 눌러 질때 펜이 되게 하시오.

</aside>

```jsx
function setup() {
		createCanvas(displayWidth, displayHeight);
		stroke(0, 0, 255);
		strokeWeight(8);
}

function draw() {
		if (mouseIsPressed) {
				line(pmouseX, pmouseY, mouseX, mouseY);}
}
```

p5.js에서 또한 setup()으로 화면을 구성하고 draw()를 통해 그릴 수 있다.

다만 이 때 주의해야할 점이 있다.

1. use `createCanvas(displayWidth, displayHeight);` instead of `size()`
2. use `mouseIsPressed` instead of `mousePressed`

기존 java를 쓰던 프로세싱과 **동일한 구현에 대해서 다른 명칭을 쓰니 주의하자**