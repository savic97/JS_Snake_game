# JS_Snake_game
snake game 
<!DOCTYPE html>
<html lang="srb">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake</title>
    <script>
        window.onload = function() {
            canvas = document.getElementById("canvas");
            ctx = canvas.getContext("2d");
            setInterval(draw, 1000 / 5);
        }

        function drawRect(topLeftX, topLeftY, width, height, color) {
            ctx.fillStyle = color;
            ctx.fillRect(topLeftX, topLeftY, width, height);
        }
        
        var mrezica = 20;
        var tileCount = 15;
        var xosa = 0;
        var yosa = 0;
        var niz = [];
        var tailLength = 3;
        var hranaX = Math.floor(Math.random() * tileCount); 
        var hranaY = Math.floor(Math.random() * tileCount);
        var playerX = Math.floor(Math.random() * tileCount);
        var playerY = Math.floor(Math.random() * tileCount);
        var rez = 0;
        var pomeraj = 1; //ako je vise od jedan razdvoji se

        var showGrid = true;

        window / alert("GAMEPLAY:\n \n upravljas strelicama\n ako se  zmija ugrize to je kraj igre!\n");

        document.addEventListener("keydown", keyDown);

        function keyDown(e) {
            if (e.keyCode == "37") {
                moveLeft();
                e.preventDefault();
            }
            if (e.keyCode == "38") {
                moveUp();
                e.preventDefault();
            }
            if (e.keyCode == "39") {
                moveRight();
                e.preventDefault();
            }
            if (e.keyCode == "40") {
                moveDown();
                e.preventDefault();
            }
        }

        function moveLeft() {
            if (xosa != pomeraj) {
                xosa = -pomeraj; 
                yosa = 0;
            }
        }

        function moveRight() {
            if (xosa != -pomeraj) {
                xosa = pomeraj;
                yosa = 0;
            }
        }

        function moveUp() {
            if (yosa != pomeraj) {
                xosa = 0; 
                yosa = -pomeraj;
            }
        }

        function moveDown() {
            if (yosa != -pomeraj) {
                xosa = 0;
                yosa = pomeraj; 
            }
        }

        var showTitleScreen = true;

        document.addEventListener("click", NewGame);

        function NewGame() {
            if (showTitleScreen) {
                showTitleScreen = false;
            }
        }

        var indexNumber = 0;
        var index = [true, false, true, false];

        function toggleGrid() {
            indexNumber += 1;
        }

        function draw() {
            drawRect(0, 0, canvas.width, canvas.height, "white"); 

            if (indexNumber == 2) {
                indexNumber = 0;
            }

            showGrid = index[indexNumber];
            //pocetak nove ige i njegva boja
            if (showTitleScreen) {
                ctx.fillStyle = "blue";
                ctx.font = "30px Arial";
                ctx.fillText("JS Zmijica", canvas.width / 2 - 100, canvas.height / 3);
                /*
                ctx.font = "15px Arial";
                ctx.fillStyle = "green";
                ctx.fillText("Game By :DS", 5, canvas.height - 15);*/

                ctx.font = "20px Arial";
                ctx.fillStyle = "red";
                ctx.fillText("New Game", canvas.width / 2 - 60, canvas.height / 2 + 80);
                return;
            }


            playerX = playerX + xosa;
            playerY = playerY + yosa;

            //Move To Other Side Of Screen
            if (playerX > tileCount - 1) {
                playerX = 0;
            }
            if (playerX < 0) {
                playerX = tileCount - 1;
            }
            if (playerY < 0) {
                playerY = tileCount - 1;
            }
            if (playerY > tileCount - 1) {
                playerY = 0;
            }
            if (showGrid) {
                for (var r = 0; r < 20; r++) {
                    for (var c = 0; c < 20; c++) {
                        ctx.beginPath();
                        ctx.strokeStyle = "#ccffff"; //boja mrezice
                        ctx.rect(c * 20, r * 20, 20 - 2, 20 - 2);
                        ctx.stroke();
                        ctx.closePath();
                    }
                }
            }

            for (var i = 0; i < niz.length; i++) {

                drawRect(niz[i].x * mrezica, niz[i].y * mrezica, mrezica - 2, mrezica - 2, "green"); //boja zmijce

                drawRect(niz[niz.length - 1].x * mrezica, niz[niz.length - 1].y * mrezica, mrezica - 2, mrezica - 2, "blue"); //glava zmije

                if (niz[i].x == playerX &&
                    niz[i].y == playerY) {
                    rez = 0;
                    tailLength = 5;
                    playerX = Math.floor(Math.random() * tileCount);
                    playerY = Math.floor(Math.random() * tileCount);
                    xosa = 0;
                    yosa = 0;
                }

            }

            niz.push({
                x: playerX,
                y: playerY
            });
            while (niz.length > tailLength) {
                niz.shift();
            }

            if (playerX == hranaX &&
                hranaY == playerY) {
                tailLength++;
                rez = rez + 10;
                hranaX = Math.floor(Math.random() * tileCount);
                hranaY = Math.floor(Math.random() * tileCount);
            }

            //hrana zmijce
            drawRect(hranaX * mrezica, hranaY * mrezica, mrezica - 2, mrezica - 2, "orange");


            var p = document.getElementById("score");

            p.innerHTML = rez;
        }
    </script>
    <style>
        button:focus {
            outline: none;
        }
        
        #rezultat {
            left: 40%;
            position: relative;
            width: 15%;
            max-height: 50px;
            display: block;
            background-color: teal;
            color: red;
            font-size: 15px;
        }
        
        button {
            width: 100px;
            height: 30px;
            font-weight: bold;
            font-size: 10px;
            margin-bottom: 5px;
            color: darkred;
            background-color: turquoise;
            border-radius: 3px;
            margin-right: 3px;
        }
        
        body {
            background-color: darkseagreen;
        }
        
        #id1,
        #id2,
        #id3,
        #id4 {
            display: flex;
            justify-content: center;
        }
    </style>
</head>

<body>
    <div id="id1">
        <canvas id="canvas" width="324" height="324" style="display: block; margin: 10px auto; border: 3px solid white; border-radius: 5px;margin:0px;"></canvas>
    </div>
    <br>
    <br>
    <div id="rezultat">Score:
        <p id="score"></p>
    </div>
    <br>
    <br>
    <div id="id2">
        <button onclick="moveUp()">Top</button>
    </div>
    <div id="id3">
        <button onclick="moveLeft()">Left</button>
        <button onclick="toggleGrid()">Grid</button>
        <button onclick="moveRight()">Right</button>
    </div>
    <div id="id4">
        <button onclick="moveDown()">Down</button>
    </div>
</body>

</html>
