# FaceApi를 이용한 미용 프로그램의 구현

# Assignment

---

> [https://cafe.naver.com/newgraphics/91](https://cafe.naver.com/newgraphics/91) 를 참고하여 사람의 얼굴에 눈썹을 만들거나 변형하고, 입술에 화장을 할 수 있는 프로그램을 만드시오.
> 

# Result

---

![Untitled](FaceApi%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%86%E1%85%B5%E1%84%8B%E1%85%AD%E1%86%BC%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%80%E1%85%B3%E1%84%85%E1%85%A2%E1%86%B7%E1%84%8B%E1%85%B4%20%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%AB%206bef63ea013244b78160611af728632b/Untitled.png)

# Code

---

`index.html`

```html
<!DOCTYPE html>
<html lang="en">
		<head>
			<script src="[https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js](https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js)"></script>
			<script src="[https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/addons/p5.sound.min.js](https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/addons/p5.sound.min.js)"></script>
			<script src="[https://unpkg.com/ml5@latest/dist/ml5.min.js](https://unpkg.com/ml5@latest/dist/ml5.min.js)"></script>
			<link rel="stylesheet" type="text/css" href="style.css">
			<meta charset="utf-8" />
		</head>
		<script src="sketch.js"></script>
		<body>
			<h1>FaceApi Landmarks</h1>
			<h2>Draw Eyebrows and Lips</h2>
		</body>
</html>
```

`sketch.js`

```jsx
let img, parts,detections;
const options = {withLandmarks: true, withDescriptors: false};

function preload() {
  img = loadImage('face.png');
}

function setup() {
  //setting
  createCanvas(500, 500);
  img.resize(width*0.8, height*0.8);
  
  //create faceapi
  faceapi = ml5.faceApi(options, modelReady);
  
}

function modelReady() {
  // print face api's info to console log
  console.log('face recognition ready!')
  console.log(faceapi)
  // call faceapi's detectSingle() <- img,gotResults
  faceapi.detectSingle(img, gotResults);
}

function gotResults(err, results) {
  //error handling
  if (err) {
    console.error(err);
    return;
  }
  // print results to console
  console.log(results);
  // insert parts of face by given results
  detections = results;
  parts = results.parts;
  
  background(255);
  image(img, 0,0, width*0.8, height*0.8);
  if(detections){
    drawBox(detections)
    drawFace(detections)
  }
  
  
  strokeWeight(2);
  drawFace();
}

function drawBox(detections){
    const alignedRect = detections.alignedRect;
    const {_x, _y, _width, _height} = alignedRect._box;
    noFill();
    stroke(161, 95, 251);
    strokeWeight(2)
    rect(_x, _y, _width, _height)
}

function drawFace() {
  // black stroke
  stroke(0);
  
	push()

		  //입술그리기
		  //fill lips with red
		  fill(255,0,0);
		  beginShape();
		  for(let i=0; i<parts.mouth.length; i++){
		    vertex(parts.mouth[i]._x, parts.mouth[i]._y);
		  }
		  vertex(parts.mouth[0]._x, parts.mouth[0]._y);
		  endShape();
		  
		  //white
		  fill(255);
		  beginShape();
		  for(let i=parts.mouth.length/2; i<parts.mouth.length; i++){
		    vertex(parts.mouth[i]._x, parts.mouth[i]._y);
		  }
		  vertex(parts.mouth[0]._x, parts.mouth[0]._y);
		  endShape();
		
		  
		
		  //코 그리기    
		  noFill();
		  beginShape();
		  for(let i=0; i<parts.nose.length; i++){
		    vertex(parts.nose[i]._x, parts.nose[i]._y);
		  }
		  endShape();
		
		  //눈 그리기
		  noFill();
		  beginShape();
		  for(let i=0; i<parts.leftEye.length; i++){
		    vertex(parts.leftEye[i]._x, parts.leftEye[i]._y);
		  }
		  vertex(parts.leftEye[0]._x, parts.leftEye[0]._y);
		  endShape();
		  // yesFill()
		  beginShape();
		  for(let i=0; i<parts.rightEye.length; i++){
		    vertex(parts.rightEye[i]._x, parts.rightEye[i]._y);
		  }
		  vertex(parts.rightEye[0]._x, parts.rightEye[0]._y);
		  endShape();
		
		  //눈썹 그리기  
		  //grey stroke
		  stroke(102,51,0);
		  //fill with white
		  fill(153,76,0);
		  beginShape();
		  for(let i=0; i<parts.leftEyeBrow.length; i++){
		    vertex(parts.leftEyeBrow[i]._x, parts.leftEyeBrow[i]._y);
		  }
		  endShape();
		
		  beginShape();
		  for(let i=0; i<parts.rightEyeBrow.length; i++){
		    vertex(parts.rightEyeBrow[i]._x, parts.rightEyeBrow[i]._y);
		  }
		  endShape();

	pop();
}
```

`style.css`

```css
html, body {
  margin: 0;
  padding: 0;
}
canvas {
  display: block;
}
```

# Analysis,Description of faceApi

---

## faceApi 객체

---

```jsx
let img, parts,detections;
const options = {withLandmarks: true, withDescriptors: false};

function preload() {
  img = loadImage('face.png');
}

function setup() {
  //setting
  createCanvas(500, 500);
  img.resize(width*0.8, height*0.8);
  
  //create faceapi
  const faceapi = ml5.faceApi(options, modelReady); 
}
```

ml5js에서 지원하는 faceApi객체를 생성한다. 아래와 같은 매개변수를 가진다.

- option : detect option이다.  option은 아래와 같이 withLandmarks,withDescriptors가 있다.

```jsx
const options = {withLandmarks: true, withDescriptors: false};
```

- modelReady

> 대상모델이 로드되거나 준비되었다면 true를, 아니라면 false를 리턴
> 

## modelReady()

---

```jsx
function modelReady() {
  // print face api's info to console log
  console.log('face recognition ready!')
  console.log(faceapi)
  // call faceapi's detectSingle() <- img,gotResults
  faceapi.detectSingle(img, gotResults);
}
```

- console에 faceapi에 객체 정보를 출력
- faceapi.detectSingle(img, gotResults);
    
    **Inputs**
    
    - img
    
    : If given an image, this is the image that the face detection will be applied
    
    - gotResults
    
    Function. A function to handle the results of `.makeSparkles()`
    . Likely a function to do something with the results of makeSparkles.
    
    **Outputs**
    
    - **Array**
    
    : Returns an array of objects. Each object contains `{alignedRect, detection, landmarks, unshiftedLandmarks}`
    

## gotResults(err, results)

---

```jsx
function gotResults(err, results) {
  //error handling
  if (err) {
    console.error(err);
    return;
  }
  // print results to console
  console.log(results);
  // insert parts of face by given results
  detections = results;
  parts = results.parts;
  
  background(255);
  image(img, 0,0, width*0.8, height*0.8);
  if(detections){
    drawBox(detections)
    drawFace(detections)
  }  
  strokeWeight(2);
  drawFace();
}
```

- error 발생 시 콘솔에 error 출력
- 콘솔에 결과값 출력
- image 그리고, 결과값이 존재한다면
    - drawBox(detections) 호출
    - drawFace(detections) 호출

## drawBox(detections)

---

```jsx
function drawBox(detections){
    const alignedRect = detections.alignedRect;
    const {_x, _y, _width, _height} = alignedRect._box;
    noFill();
    stroke(161, 95, 251);
    strokeWeight(2)
    rect(_x, _y, _width, _height)
}
```

## drawFace(detections)

---

```jsx
function drawFace() {
  // black stroke
  stroke(0);
  
	push()

		  //입술그리기
		  //fill lips with red
		  fill(255,0,0);
		  beginShape();
		  for(let i=0; i<parts.mouth.length; i++){
		    vertex(parts.mouth[i]._x, parts.mouth[i]._y);
		  }
		  vertex(parts.mouth[0]._x, parts.mouth[0]._y);
		  endShape();
		  
		  //white
		  fill(255);
		  beginShape();
		  for(let i=parts.mouth.length/2; i<parts.mouth.length; i++){
		    vertex(parts.mouth[i]._x, parts.mouth[i]._y);
		  }
		  vertex(parts.mouth[0]._x, parts.mouth[0]._y);
		  endShape();
		
		  
		
		  //코 그리기    
		  noFill();
		  beginShape();
		  for(let i=0; i<parts.nose.length; i++){
		    vertex(parts.nose[i]._x, parts.nose[i]._y);
		  }
		  endShape();
		
		  //눈 그리기
		  noFill();
		  beginShape();
		  for(let i=0; i<parts.leftEye.length; i++){
		    vertex(parts.leftEye[i]._x, parts.leftEye[i]._y);
		  }
		  vertex(parts.leftEye[0]._x, parts.leftEye[0]._y);
		  endShape();
		  // yesFill()
		  beginShape();
		  for(let i=0; i<parts.rightEye.length; i++){
		    vertex(parts.rightEye[i]._x, parts.rightEye[i]._y);
		  }
		  vertex(parts.rightEye[0]._x, parts.rightEye[0]._y);
		  endShape();
		
		  //눈썹 그리기  
		  //grey stroke
		  stroke(102,51,0);
		  //fill with white
		  fill(153,76,0);
		  beginShape();
		  for(let i=0; i<parts.leftEyeBrow.length; i++){
		    vertex(parts.leftEyeBrow[i]._x, parts.leftEyeBrow[i]._y);
		  }
		  endShape();
		
		  beginShape();
		  for(let i=0; i<parts.rightEyeBrow.length; i++){
		    vertex(parts.rightEyeBrow[i]._x, parts.rightEyeBrow[i]._y);
		  }
		  endShape();

	pop();
}
```

# Reference

---

- ml5js ****Face-Api Document****

[https://learn.ml5js.org/#/reference/face-api?id=tutorials](https://learn.ml5js.org/#/reference/face-api?id=tutorials)

- **FaceApi Landmarks Demo**

[https://editor.p5js.org/ml5/sketches/FaceApi_Image_Landmarks](https://editor.p5js.org/ml5/sketches/FaceApi_Image_Landmarks)

- face-api.js github

[https://github.com/justadudewhohacks/face-api.js/blob/master/README.md#models-face-recognition](https://github.com/justadudewhohacks/face-api.js/blob/master/README.md#models-face-recognition)

- **Will you release facial expression detection for face-api?**

https://github.com/ml5js/ml5-library/issues/1039