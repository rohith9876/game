# game
HTML
<html lang="en-GB">
<head>
  <meta charset="utf-8">
  <body>
    <div id="gradient"></div>
    <div id="page">
      <div id="Message-Container">
        <div id="message">
          <h1>Congratulations!</h1>
          <p>You are done.</p>
          <p id="moves"></p>
          <input id="okBtn" type="button" onclick="toggleVisablity('Message-Container')" value="Cool!" />
        </div>
      </div>
      <div id="menu">
        <div class="custom-select">
          <select id="diffSelect">
                    <option value="10">Easy</option>
                    <option value="15">Medium</option>
                    <option value="25">Hard</option>
                    <option value="38">Extreme</option>                                      
                </select>
        </div>
        <input id="startMazeBtn" type="button" onclick="makeMaze()" value="Start" />
      </div>
      <div id="view">
        <div id="mazeContainer">
          <canvas id="mazeCanvas" class="border" height="1100" width="1100"></canvas>
        </div>
      </div>
    </div>
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery.touchswipe/1.6.18/jquery.touchSwipe.min.js"></script>
  </body>
</html>
CSS
$menuHeight: 65px+10px;
@mixin transition {
    transition-property: background-color opacity;
    transition-duration: 0.2s;
    transition-timing-function: ease-in-out;
}

html,
body {
    width: 100vw;
    height: 100vh;
    position: fixed;
    padding: 0;
    margin: 0;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
}

#mazeContainer {
    transition-property: opacity;
    transition-duration: 1s;
    transition-timing-function: linear;
    top: $menuHeight;
    opacity: 0;
    display: inline-block;
    background-color: rgba(0, 0, 0, 0.30);
    margin: auto;

    #mazeCanvas {
        margin: 0;
        display: block;
        border: solid 1px black;
    }
}

input,
select {
    @include transition;
    cursor: pointer;
    background-color: rgba(0, 0, 0, 0.30);
    height: 45px;
    width: 150px;
    padding: 10px;
    border: none;
    border-radius: 5px;
    color: white;
    display: inline-block;
    font-size: 15px;
    text-align: center;
    text-decoration: none;
    appearance: none;
    &:hover {
        background-color: rgba(0, 0, 0, 0.70);
    }
    &:active {
        background-color: black;
    }
    &:focus {
        outline: none;
    }
}


.custom-select {
    display: inline-block;
    select {
        -webkit-appearance: none;
        -moz-appearance: none;
        appearance: none;
        background-image: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAh0lEQVQ4T93TMQrCUAzG8V9x8QziiYSuXdzFC7h4AcELOPQAdXYovZCHEATlgQV5GFTe1ozJlz/kS1IpjKqw3wQBVyy++JI0y1GTe7DCBbMAckeNIQKk/BanALBB+16LtnDELoMcsM/BESDlz2heDR3WePwKSLo5eoxz3z6NNcFD+vu3ij14Aqz/DxGbKB7CAAAAAElFTkSuQmCC');
        background-repeat: no-repeat;
        background-position: 125px center;
    }
}

#Message-Container {
    visibility: hidden;
    color: white;
    display: block;
    width: 100vw;
    height: 100vh;
    position: fixed;
    top: 0;
    left: 1;
    bottom: 0;
    right: 1;
    background-color: rgba(0, 0, 0, 0.30);
    z-index: 1;
    #message {
        width: 300px;
        height: 300px;
        position: fixed;
        top: 50%;
        left: 50%;
        margin-left: -150px;
        margin-top: -150px;
    }
}

#page {
    font-family: "Segoe UI", Arial, sans-serif;
    text-align: center;
    height: auto;
    width: auto;
    margin: auto;
    #menu {
        margin: auto;
        padding: 10px;
        height: 65px;
        box-sizing: border-box;
        h1 {
            margin: 0;
            margin-bottom: 10px;
            font-weight: 600;
            font-size: 3.2rem;
        }
    }
    #view {
        position: absolute;
        top:65px;
        bottom: 0;
        left: 0;
        right: 0;
        width: 100%;
        height: auto;
           
    }
}

.border {
    border: 1px black solid;
    border-radius: 5px;
}



#gradient {
    z-index: -1;
    position: fixed;
    top: 0;
    bottom: 0;
    width: 100vw;
    height: 100vh;
    color: #fff;
    background: linear-gradient(-45deg, #EE7752, #E73C7E, #23A6D5, #23D5AB);
    background-size: 400% 400%;
    animation: Gradient 15s ease infinite;
}

@keyframes Gradient {
    0% {
        background-position: 0% 50%
    }
    50% {
        background-position: 100% 50%
    }
    100% {
        background-position: 0% 50%
    }
}

 /* Extra small devices (phones, 600px and down) */
 @media only screen and (max-width: 400px) {
     input, select{
         width: 120px;
     }
 }
JS
function rand(max) {
  return Math.floor(Math.random() * max);
}

function shuffle(a) {
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}

function changeBrightness(factor, sprite) {
  var virtCanvas = document.createElement("canvas");
  virtCanvas.width = 500;
  virtCanvas.height = 500;
  var context = virtCanvas.getContext("2d");
  context.drawImage(sprite, 0, 0, 500, 500);

  var imgData = context.getImageData(0, 0, 500, 500);

  for (let i = 0; i < imgData.data.length; i += 4) {
    imgData.data[i] = imgData.data[i] * factor;
    imgData.data[i + 1] = imgData.data[i + 1] * factor;
    imgData.data[i + 2] = imgData.data[i + 2] * factor;
  }
  context.putImageData(imgData, 0, 0);

  var spriteOutput = new Image();
  spriteOutput.src = virtCanvas.toDataURL();
  virtCanvas.remove();
  return spriteOutput;
}

function displayVictoryMess(moves) {
  document.getElementById("moves").innerHTML = "You Moved " + moves + " Steps.";
  toggleVisablity("Message-Container");  
}

function toggleVisablity(id) {
  if (document.getElementById(id).style.visibility == "visible") {
    document.getElementById(id).style.visibility = "hidden";
  } else {
    document.getElementById(id).style.visibility = "visible";
  }
}

function Maze(Width, Height) {
  var mazeMap;
  var width = Width;
  var height = Height;
  var startCoord, endCoord;
  var dirs = ["n", "s", "e", "w"];
  var modDir = {
    n: {
      y: -1,
      x: 0,
      o: "s"
    },
    s: {
      y: 1,
      x: 0,
      o: "n"
    },
    e: {
      y: 0,
      x: 1,
      o: "w"
    },
    w: {
      y: 0,
      x: -1,
      o: "e"
    }
  };

  this.map = function() {
    return mazeMap;
  };
  this.startCoord = function() {
    return startCoord;
  };
  this.endCoord = function() {
    return endCoord;
  };

  function genMap() {
    mazeMap = new Array(height);
    for (y = 0; y < height; y++) {
      mazeMap[y] = new Array(width);
      for (x = 0; x < width; ++x) {
        mazeMap[y][x] = {
          n: false,
          s: false,
          e: false,
          w: false,
          visited: false,
          priorPos: null
        };
      }
    }
  }
