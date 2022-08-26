# Code Analysis : Face Mask

# Source Code

`index.html`

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
<script src="[https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js](https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js)"></script>
<script src="[https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/addons/p5.sound.min.js](https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/addons/p5.sound.min.js)"></script>
<script src="[https://unpkg.com/ml5@latest/dist/ml5.min.js](https://unpkg.com/ml5@latest/dist/ml5.min.js)"></script>
<link rel="stylesheet" type="text/css" href="style.css">
<meta charset="utf-8" />
</head>
<body>
<main>
</main>
<script src="sketch.js"></script>
</body>
</html>
```

`sketch.js`

```jsx
let img, parts;
let options = {withLandmarks: true, withDescriptors: false};

function preload() {
  img = loadImage('face.png');
}

function setup() {
  // create Canvas
  createCanvas(img.width*2, img.height);
  // call faceApi() <- options, modelReady
  // role of options? modelReady?
  // where is modelReady ???
  faceapi = ml5.faceApi(options, modelReady);
  background(255);
  image(img,0,0);
  image(img, img.width, 0);    
}

function modelReady() {
  faceapi.detectSingle(img, gotResults);
}

function gotResults(err, results) {
  if (err) {
    console.error(err);
    return;
  }
  console.log(results);
  parts = results.parts;
  noFill();
  stroke(0, 0, 255);
  strokeWeight(3);
  drawFace();
}

function drawFace() {
  stroke(255, 0, 255);

  //입술그리기
  beginShape();
  for(let i=0; i<parts.mouth.length; i++){
    vertex(parts.mouth[i]._x, parts.mouth[i]._y);
  }
  vertex(parts.mouth[0]._x, parts.mouth[0]._y);
  endShape();

  //코 그리기    
  beginShape();
  for(let i=0; i<parts.nose.length; i++){
    vertex(parts.nose[i]._x, parts.nose[i]._y);
  }
  endShape();

  //눈 그리기
  fill(100);
  beginShape();
  for(let i=0; i<parts.leftEye.length; i++){
    vertex(parts.leftEye[i]._x, parts.leftEye[i]._y);
  }
  vertex(parts.leftEye[0]._x, parts.leftEye[0]._y);
  endShape();

  beginShape();
  for(let i=0; i<parts.rightEye.length; i++){
    vertex(parts.rightEye[i]._x, parts.rightEye[i]._y);
  }
  vertex(parts.rightEye[0]._x, parts.rightEye[0]._y);
  endShape();

  //눈썹 그리기    
  noFill();
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
}
```

## Notation

---

```jsx
function setup() {
  // create Canvas
  createCanvas(img.width*2, img.height);
  // call faceApi() <- options, modelReady
  // role of options? modelReady?
  // where is modelReady ???
  faceapi = ml5.faceApi(options, modelReady);
  background(255);
  image(img,0,0);
  image(img, img.width, 0);    
}
```

# Console 결과창

---

1. *{detection: e, landmarks: e, unshiftedLandmarks: e, alignedRect: e, parts: Object}*
    1. ▶detection: e
    2. ▶landmarks: e
    3. ▶unshiftedLandmarks: e
    4. ▶alignedRect: e
    5. ▶parts: Object

# Reference

---

****11.9 페이스 마스크 만들기 : Face-Api****

[https://wikidocs.net/103515](https://wikidocs.net/103515)