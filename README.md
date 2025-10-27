# ducklifeproject

// Global Variables

var screen = "mainMenuScreen";
var difficulty = 0;
var skyBlue = color(0, 175, 255);
var grassGreen = color(0, 115, 0);
var waterBlue = color(50, 125, 255);
var colors = [color(255, 0, 0), color(255, 0, 255), color(100, 0, 200), color(0, 0, 255), color(0, 100, 255), color(0, 255, 255), color(0, 255, 0), color(0, 150, 0), color(255, 255, 0), color(255, 75, 0), color(135, 65, 25), color(0)];
var gameCoins = 0;
var showErrorMessage = [false, false, false, false];
var gameOverScreen = false;
var raceOverScreen = false;
var keys = [];

// Global Objects

var cost = {

    run: 25,
    swim: 25,
    energy: 25,
    fly: 25
};

var sliders = {

    h: 200,
    w: 200
};

var items = {

    cost: [25, 50, 100, 250],
    buy: [false, false, false, false],
    wear: [false, false, false, false]
};

var confetti = {

    x: [],
    y: [],
    s: [],
    c: []
};

var energy = {

    remaining: 100,
    loss: 2.5
};

// Game Objects

var g1 = {

    score: 0,
    start: false,
    gravity: 0.75,
    platform: {
        x: [50, random(125, 225), random(275, 375)],
        h: [random(125, 250), random(125, 250), random(125, 250)]
    },
    wave: {
        x: [0, -85, 25],
        y: [350, 350, 370],
        speed: [0.25, 0.15, 0.35]
    }
};

var g2 = {

    score: 0,
    start: false,
    tracksY: 0,
    grassY: 0,
    coin: {
        x: [120, 200, 280],
        y: [random(-400, 150), random(-400, 150), random(-400, 150)],
        timer: 0,
    },
    board: {
        x: [110, 190, 270],
        y: [random(-3000, 150), random(-3000, 150), random(-3000, 150)],
        on: false,
        timer: 1000,
    },
    hole: {
        x: [120, 200, 280],
        y: [random(-400, 150), random(-400, 150), random(-400, 150)]
    }
};

var g3 = {

    score: 0,
    start: false,
    penguinsLanded: 0,
    timer: 0,
    cannon: {
        x: 200,
        y: 300,
        speed: 3
    },
    snowball: {
        x: [],
        y: [],
        speed: []
    }
};

var g4 = {

    score: 0,
    start: false,
    timer: 0,
};

var g5 = {

    score: 0,
    start: false,
    obstacle: {
        x: 1000,
        y: random(75, 275),
        h: 50,
        w: 20,
        s: 5
    }
};

var g6 = {

    score: 0,
    start: false
};

// Race Object

var race = {

    numberOfWins: 0,
    totalGames: 0,
    unlockRace: [true, false, false, false, false, false, false, false],
    beaten: [false, false, false, false, false, false, false, false],
    raceNumber: 1,
    type: "Running",
    opponent: "Donald",
    reward: 100,
    outcome: "undefined",
    x: [0, 0, 0, 0, 0, 0, 0, 0]
};

// OOP for Game 2 

var Train = function(x, y, l, s) {

    this.x = x;
    this.y = y;
    this.l = l;
    this.s = s;
};

var train1 = new Train(104, random(-2000, -400), round(random(1, 3)), round(random(2, 5)));
var train2 = new Train(184, random(-2000, -400), round(random(1, 3)), round(random(2, 5)));
var train3 = new Train(264, random(-2000, -400), round(random(1, 3)), round(random(2, 5)));

// OOP for Game 3 

var Penguin = function(x, y, speed) {

    this.x = x;
    this.y = y;
    this.speed = speed;
};

var penguins = new Penguin([random(35, 365)], [random(-400, -100)], [3]);

// OOP for Game 4 

var Ball = function(x, y, size, sx, sy) {

    this.x = x;
    this.y = y;
    this.size = size;
    this.sx = sx;
    this.sy = sy;
};

var balls = new Ball([random(-100, -35)], [200], [random(10, 50)], [random(2, 4)], [random(3, 6)]);

// OOP for Game 6

var Spike = function(x, y, size, speedX) {

    this.x = x;
    this.y = y;
    this.size = size;
    this.speedX = speedX;
};

var drawSpike = function(x, y) {

    noStroke();
    fill(200);
    triangle(x - 10, y, x, y - 25, x, y);
    fill(175);
    triangle(x, y, x, y - 25, x + 10, y);
};

Spike.prototype.draw = function() {

    for (var i = 0; i < this.x.length; i++) {

        drawSpike(this.x[i], this.y[i]);

        if (this.size[i] === 2) {
            drawSpike(this.x[i], this.y[i]);
            drawSpike(this.x[i] + 20, this.y[i]);
        }

        if (this.size[i] === 3) {
            drawSpike(this.x[i], this.y[i]);
            drawSpike(this.x[i] + 20, this.y[i]);
            drawSpike(this.x[i] + 40, this.y[i]);
        }
    }
};

Spike.prototype.move = function() {

    for (var i = 0; i < this.x.length; i++) {

        if (g6.start === true) {
            this.x[i] -= this.speedX;
        }

        if (this.x[i] < -25) {

            g6.score++;
            gameCoins++;

            this.x.push(random(425, 475));
            this.y.push(250);
            this.size.push(round(random(1, 3)));

            this.x.splice(i, 1);
            this.y.splice(i, 1);
            this.size.splice(i, 1);
        }
    }
};

var spike = new Spike([random(425, 475), random(600, 650)], [250, 250], [round(random(1, 3)), round(random(1, 3))], 3);

// Duck Constructor + Duck Functions (g1v = game 1 variables -> all the duck variables that are specific to game 1 are in that object) 

var Duck = function(x, y, w, h, name, runLevel, swimLevel, energyLevel, flyLevel, gender, color, hairStyle, rotation, g1v, g2v, g4v, g5v, g6v, pgv) {

    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.name = name;
    this.runLevel = runLevel;
    this.swimLevel = swimLevel;
    this.energyLevel = energyLevel;
    this.flyLevel = flyLevel;
    this.gender = gender;
    this.color = color;
    this.hairStyle = hairStyle;
    this.rotation = rotation;
    this.g1v = g1v;
    this.g2v = g2v;
    this.g4v = g4v;
    this.g5v = g5v;
    this.g6v = g6v;
    this.pgv = pgv;
};

// Instance of Your Duck

var duck = new Duck(200, 200, 70, 112, ["Enter a name"], 1, 1, 1, 1, 0, 8, 1, 0, {speedY: 0, numJumps: 0, jumped: false ,jumpSpeed: 12}, {jumped: false, spin: 0, faceLeft: false}, {faceLeft: false}, {fallSpeed: 5, angle: 90}, {speedY: 0, fall: true, click: false}, {quotes: ["Press space to\nsee me dance!", "I'm hungry!", "How can Pablo the\nPenguin even fly?", "Let's race!", "Let's practice!", "Can we go to\nthe shop?"], timer: 350, g: round(random(0, 5)), faceLeft: false, headX: 5, headS: 1, danceTimer: 0});

// Instance of the AI Duck 

var duckAI = new Duck(200, 200, 70, 112, ["Enter a name"], 0, 0, 0, 0, 0, 8, round(random(1, 8)), 0, {}, {}, {}, {}, {}, {});

var tie = function(x, y) {

    strokeWeight(3);

    if (duck.color === 11) {
        fill(255);
        stroke(255);
    } else {
        fill(0);
        stroke(0);
    }

    triangle(x, y, x - 4, y - 10, x + 4, y - 10);
    quad(x, y, x + 6, y + 15, x, y + 25, x - 6, y + 15);
};

var hat1 = function(x, y) {

    strokeWeight(3);
    stroke(0);
    fill(0, 0, 255);
    strokeCap(SQUARE);
    arc(x, y + 35, 35, 35, -175, 0);
    line(x - 19, y + 35, x + 35, y + 35);
    strokeCap(ROUND);
};

var hat2 = function(x, y) {

    strokeWeight(3);
    stroke(0);
    strokeCap(SQUARE);
    fill(255, 0, 0);
    arc(x, y + 35, 35, 35, -185, 2);
    fill(255, 255, 0);
    arc(x, y + 35, 35, 35, -120, 0);
    fill(0, 0, 255);
    arc(x, y + 35, 35, 35, -60, 0);
    line(x - 18, y + 35, x + 18, y + 35);
    fill(0);
    ellipse(x, y + 14, 3, 5);
    noStroke();
    fill(200);
    ellipse(x - 9, y + 13, 15, 4);
    ellipse(x + 9, y + 13, 15, 4);
    strokeCap(ROUND);
};

var shoes = function(x, y) {

    fill(255, 0, 0);
    strokeWeight(2);
    stroke(0);
    ellipse(x + 8, y + 130, 35, 15);
    noFill();
    stroke(255);
    ellipse(x + 13, y + 123, 5, 10);
    ellipse(x + 13, y + 133, 5, 10);
};

var hair = function(x, y, d) {

    stroke(0);
    strokeWeight(3);

    switch (d) {
        case 1:
            break;
        case 2:
            line(x, y + 22, x, y + 8);
            break;
        case 3:
            line(x, y + 20, x - 8, y + 12);
            line(x, y + 18, x, y + 10);
            line(x + 1, y + 20, x + 8, y + 12);
            break;
        case 4:
            fill(0);
            line(x - 12, y + 22, x - 12, y + 18);
            line(x - 8, y + 22, x - 8, y + 14);
            line(x - 4, y + 22, x - 4, y + 12);
            line(x, y + 22, x, y + 8);
            line(x + 4, y + 22, x + 4, y + 12);
            line(x + 8, y + 22, x + 8, y + 14);
            line(x + 12, y + 22, x + 12, y + 18);
            break;
        case 5:
            fill(0);
            rect(x - 15, y + 10, 28, 15);
            break;
        case 6:
            fill(0);
            ellipse(x, y + 20, 60, 35);
            break;
        case 7:
            noFill();
            arc(x - 10, y + 22, 40, 25, 270, 360);
            arc(x - 13, y + 24, 30, 20, 270, 360);
            arc(x - 16, y + 26, 20, 15, 270, 360);
            break;
        case 8:
            noFill();
            arc(x - 10, y + 50, 55, 80, 180, 360);
            arc(x - 10, y + 50, 45, 70, 185, 360);
            arc(x - 10, y + 50, 35, 60, 190, 360);
            break;
    }
};

var drawDucks = function(x, y, d) {

    hair(x + 5, y - d.h / 1.72 - 16, d.hairStyle);

    noStroke();
    fill(colors[d.color]);

    if (d === duck) {
        if (g2.board.on === true) {
            if (g2.board.timer > 75) {
                fill(random(0, 255), random(0, 255), random(0, 255), 200);
            }
            if (g2.board.timer <= 75 && g2.board.timer > 60) {
                fill(colors[duck.color]);
            }
            if (g2.board.timer <= 60 && g2.board.timer > 50) {
                fill(random(0, 255), random(0, 255), random(0, 255), 200);
            }
            if (g2.board.timer <= 50 && g2.board.timer > 30) {
                fill(colors[duck.color]);
            }
            if (g2.board.timer <= 30 && g2.board.timer > 20) {
                fill(random(0, 255), random(0, 255), random(0, 255), 200);
            }
            if (g2.board.timer <= 20 && g2.board.timer > 10) {
                fill(colors[duck.color]);
            }
            if (g2.board.timer <= 10 && g2.board.timer > 0) {
                fill(random(0, 255), random(0, 255), random(0, 255), 200);
            }
        }
    }

    ellipse(x + 5, y - d.h / 1.72 + 30, 50, 50);
    ellipse(x, y, d.w / 1.08, d.h / 1.72);
    stroke(0);
    strokeWeight(2);
    pushMatrix();
    translate(x - 2, y - 15);
    rotate(0);
    arc(0, 0, 18, d.h / 2, 10, 150);
    popMatrix();
    fill(255);
    stroke(0);
    ellipse(x + 15, y - d.h / 1.72 + 25, 15, 15);
    fill(0);
    ellipse(x + 17, y - d.h / 1.72 + 25, 8, 8);
    strokeWeight(3);
    strokeCap(SQUARE);
    line(x, y + d.h / 3.5, x, y + d.h / 2 + 1);
    line(x, y + d.h / 2, x + 25, y + d.h / 2);
    strokeCap(ROUND);
    fill(235, 145, 45);
    strokeWeight(2);
    triangle(x + 30, y - d.h / 1.72 + 25, x + 30, y - d.h / 1.72 + 35, x + 40, y - d.h / 1.72 + 30);

    if (d === duck) {
        if (items.buy[0] === true && items.wear[0] === true) {
            tie(x + 8, y - duck.h / 1.72 + 58);
        }
        if (items.buy[1] === true && items.wear[1] === true) {
            hat1(x + 5, y - duck.h / 1.72 - 22);
        }

        if (items.buy[2] === true && items.wear[2] === true) {
            shoes(x + 5, y + duck.h / 2 - 132);
        }

        if (items.buy[3] === true && items.wear[3] === true) {
            hat2(x + 5, y - duck.h / 2 - 32);
        }

        if (race.beaten[7] === true) {
            textSize(50);
            fill(255, 200, 0);
            text("♛", x + 5, y - duck.h / 2 - 20);
        }
    }
};

var drawPablo = function(x, y) {

    noStroke();
    fill(0);
    ellipse(x + 5, y - duckAI.h / 1.72 + 30, 50, 50);
    ellipse(x, y, duckAI.w / 1.08, duckAI.h / 1.72);
    fill(255);
    ellipse(x + 8, y + 2, 25, 40);
    stroke(0);
    strokeWeight(2);
    ellipse(x + 15, y - duckAI.h / 1.72 + 25, 15, 15);
    fill(0);
    ellipse(x + 17, y - duckAI.h / 1.72 + 25, 8, 8);
    strokeWeight(3);
    strokeCap(SQUARE);
    stroke(235, 145, 45);
    line(x, y + duckAI.h / 3.5, x, y + duckAI.h / 2 + 1);
    line(x, y + duckAI.h / 2, x + 25, y + duckAI.h / 2);
    strokeCap(ROUND);
    fill(235, 145, 45);
    strokeWeight(2);
    triangle(x + 30, y - duckAI.h / 1.72 + 25, x + 30, y - duckAI.h / 1.72 + 35, x + 40, y - duckAI.h / 1.72 + 30);
};

// OOP for Buttons

var Button = function(x, y, w, h, label, nextScreen) {

    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.label = label;
    this.nextScreen = nextScreen;
};

Button.prototype.over = function() {
    return (mouseX > this.x && mouseX < this.x + this.w && mouseY > this.y && mouseY < this.y + this.h);
};

Button.prototype.draw = function() {

    stroke(0);
    strokeWeight(3);

    if (this.over()) {
        fill(245);
        cursor(HAND);
    } else {
        fill(255);
    }
    rect(this.x, this.y, this.w, this.h);

    fill(0);
    text(this.label, this.x + this.w / 2, this.y + this.h / 2);
};

Button.prototype.clicked = function() {
    if (this.over()) {
        screen = this.nextScreen;
    }
};

var buttons = [
    new Button(125, 175, 150, 50, "Start", "settingsScreen"),
    new Button(125, 325, 150, 50, "Next", "personalizeYourDuckScreen"),
    new Button(75, 335, 150, 50, "Next", "homeScreen"),
    new Button(25, 125, 100, 100, "Practice", "practiceSelectScreen"),
    new Button(275, 125, 100, 100, "Race", "raceSelectScreen"),
    new Button(150, 265, 100, 100, "Profile", "profileScreen"),
    new Button(25, 265, 100, 100, "Upgrades", "upgradesScreen"),
    new Button(275, 265, 100, 100, "Shop", "shopScreen"),
    new Button(25, 100, 100, 100, "Game 1", "game1"),
    new Button(150, 100, 100, 100, "Game 2", "game2"),
    new Button(275, 100, 100, 100, "Game 3", "game3"),
    new Button(25, 215, 100, 100, "Game 4", "game4"),
    new Button(150, 215, 100, 100, "Game 5", "game5"),
    new Button(275, 215, 100, 100, "Game 6", "game6"),
    new Button(125, 335, 150, 50, "Back", "homeScreen"),
    new Button(125, 135, 150, 50, "Back", "practiceSelectScreen"),
    new Button(35, 325, 150, 50, "Back", "homeScreen"),
    new Button(215, 325, 150, 50, "Start", "raceScreen"),
    new Button(125, 175, 150, 50, "Back", "raceSelectScreen"),
    new Button(125, 325, 150, 50, "Back", "homeScreen"),
    new Button(125, 325, 150, 50, "Back", "homeScreen"),
    new Button(125, 325, 150, 50, "Back", "homeScreen"),
    new Button(150, 125, 100, 100, "Playground", "playgroundScreen"),
    new Button(125, 335, 150, 50, "Back", "homeScreen"),
    new Button(125, 325, 150, 50, "Next", "upgradeYourDuckScreen"),
];

// Global Functions

var errorMessage = function(message) {

    fill(255);
    rect(100, 125, 200, 100);
    textSize(20);
    fill(0);
    text(message, 200, 175);

    if (mouseX > 265 && mouseX < 300 && mouseY > 125 && mouseY < 160) {
        fill(245);
        cursor(HAND);

        if (mouseIsPressed) {
            if (showErrorMessage[0] === true) {
                showErrorMessage[0] = false;
            }
            if (showErrorMessage[1] === true) {
                showErrorMessage[1] = false;
            }
            if (showErrorMessage[2] === true) {
                showErrorMessage[2] = false;
            }
            if (showErrorMessage[3] === true) {
                showErrorMessage[3] = false;
            }
        }
    } else {
        fill(255);
    }

    rect(265, 125, 35, 35);

    fill(0);
    text("X", 283, 142);
};

var table = function() {

    noFill();
    stroke(0);
    for (var y = 160; y < 275; y += 35) {
        for (var x = 50; x < 350; x += 75) {
            fill(255);
            rect(x, y, 75, 35);
            fill(225);
            rect(x, 110, 75, 50);
        }
    }
};

var upgradeTable = function() {

    table();

    fill(0);
    textSize(16);
    text("Skill", 88, 135);
    text("Cost Per\nUpgrade", 163, 135);
    text("Level", 238, 135);
    text("Buy", 313, 135);

    textSize(14);
    text("Run", 88, 178);
    text("Swim", 88, 212);
    text("Fly", 88, 247);
    text("Energy", 88, 282);

    cost.run = duck.runLevel * 5 + 20;
    cost.swim = duck.swimLevel * 5 + 20;
    cost.energy = duck.energyLevel * 5 + 20;
    cost.fly = duck.flyLevel * 5 + 20;

    text(cost.run, 163, 178);
    text(cost.swim, 163, 212);
    text(cost.fly, 163, 247);
    text(cost.energy, 163, 282);

    if (duck.runLevel < 15) {
        text(duck.runLevel, 238, 178);
    } else {
        text("Max", 238, 178);
    }
    if (duck.swimLevel < 15) {
        text(duck.swimLevel, 238, 212);
    } else {
        text("Max", 238, 212);
    }
    if (duck.flyLevel < 15) {
        text(duck.flyLevel, 238, 247);
    } else {
        text("Max", 238, 247);
    }
    if (duck.energyLevel < 15) {
        text(duck.energyLevel, 238, 282);
    } else {
        text("Max", 238, 282);
    }

    if (mouseX > 275 && mouseX < 350) {
        fill(0, 255, 0, 150);

        if (mouseY > 160 && mouseY < 195) {
            cursor(HAND);
            rect(275, 160, 75, 35);
        }
        if (mouseY > 195 && mouseY < 230) {
            cursor(HAND);
            rect(275, 195, 75, 35);
        }
        if (mouseY > 230 && mouseY < 265) {
            cursor(HAND);
            rect(275, 230, 75, 35);
        }
        if (mouseY > 265 && mouseY < 300) {
            cursor(HAND);
            rect(275, 265, 75, 35);
        }
    } else {
        fill(255);
    }

    fill(0);
    textSize(18);
    for (var y = 178; y < 285; y += 35) {
        text("+", 313, y);
    }
};

var shopTable = function() {

    table();

    fill(0);
    textSize(16);
    text("Item", 88, 135);
    text("Cost", 163, 135);
    text("Buy", 238, 135);
    text("Wear", 313, 135);

    pushMatrix();
    scale(0.65);
    tie(135, 265);
    popMatrix();

    pushMatrix();
    scale(0.85);
    hat1(100, 225);
    popMatrix();

    pushMatrix();
    shoes(80, 120);
    popMatrix();

    pushMatrix();
    scale(0.85);
    hat2(105, 310);
    popMatrix();

    fill(0);

    for (var i = 0; i < items.wear.length; i++) {
        text(items.cost[i], 163, i * 35 + 178);
    }

    if (mouseX > 200 && mouseX < 275) {
        noStroke();
        fill(0, 255, 0, 150);

        if (mouseY > 160 && mouseY < 195) {
            cursor(HAND);
            rect(200, 160, 75, 35);
        }
        if (mouseY > 195 && mouseY < 230) {
            cursor(HAND);
            rect(200, 195, 75, 35);
        }
        if (mouseY > 230 && mouseY < 265) {
            cursor(HAND);
            rect(200, 230, 75, 35);
        }
        if (mouseY > 265 && mouseY < 300) {
            cursor(HAND);
            rect(200, 265, 75, 35);
        }
    } else {
        fill(255);
    }

    fill(0);
    textSize(18);
    for (var y = 178; y < 285; y += 35) {
        text("+", 238, y);
    }

    if (mouseX > 275 && mouseX < 350) {
        noStroke();
        fill(0, 255, 0, 150);

        if (mouseY > 160 && mouseY < 195) {
            cursor(HAND);
            rect(275, 160, 75, 35);
        }
        if (mouseY > 195 && mouseY < 230) {
            cursor(HAND);
            rect(275, 195, 75, 35);
        }
        if (mouseY > 230 && mouseY < 265) {
            cursor(HAND);
            rect(275, 230, 75, 35);
        }
        if (mouseY > 265 && mouseY < 300) {
            cursor(HAND);
            rect(275, 265, 75, 35);
        }
    } else {
        fill(255);
    }

    fill(0);
    textSize(18);
    if (items.wear[0] === true) {
        text("✓", 313, 178);
    } else {
        text("X", 313, 178);
    }
    if (items.wear[1] === true) {
        text("✓", 313, 212);
    } else {
        text("X", 313, 212);
    }
    if (items.wear[2] === true) {
        text("✓", 313, 247);
    } else {
        text("X", 313, 247);
    }
    if (items.wear[3] === true) {
        text("✓", 313, 282);
    } else {
        text("X", 313, 282);
    }
};

var cloud = function(x, y) {

    noStroke();
    fill(255);
    ellipse(x - 45, y, 50, 50);
    ellipse(x - 25, y + 25, 50, 50);
    ellipse(x + 10, y + 20, 50, 50);
    ellipse(x + 20, y - 10, 50, 50);
    ellipse(x - 15, y - 15, 50, 50);
    ellipse(x + 35, y + 10, 50, 50);
};

var textBox = function(x, y) {

    stroke(0);
    fill(255);
    rect(x, y, 145, 25);
    fill(0);
    text(duck.name.join("") + "|", x + 5, y + 12);
};

var colorSample = function(x, y, w, h, c) {

    stroke(200);

    if (mouseX > x && mouseX < x + w && mouseY > y && mouseY < y + h) {
        strokeWeight(3);
        cursor(HAND);
        if (mouseIsPressed) {
            duck.color = c;
        }
    } else {
        strokeWeight(2);
    }

    rect(x, y, w, h);
};

var picker = function(x, y) {

    if (((mouseX > x && mouseX < x + 15) || (mouseX > x + 85 && mouseX < x + 100)) && mouseY > y - 10 && mouseY < y + 10) {
        fill(245);
        cursor(HAND);
    } else {
        fill(255);
    }
    strokeWeight(2);
    stroke(0);
    triangle(x, y, x + 15, y - 10, x + 15, y + 10);
    triangle(x + 100, y, x + 85, y - 10, x + 85, y + 10);
};

var slider = function(x, y, label1, label2) {

    strokeWeight(4);
    strokeCap(SQUARE);
    line(x, y - 10, x, y + 10);
    line(x, y, x + 100, y);
    line(x + 100, y - 10, x + 100, y + 10);
    strokeCap(ROUND);
    textAlign(CENTER, CENTER);
    textSize(8);
    fill(0);
    text(label1, x, y + 15);
    text(label2, x + 100, y + 15);
    textAlign(LEFT, CENTER);
};

var gameBackground = function() {

    background(skyBlue);
    noStroke();
    fill(grassGreen);
    rect(0, 350, 400, 50);
    fill(255, 255, 0);
    ellipse(25, 25, 115, 115);
    cloud(100, 75);
    cloud(300, 50);
};

// Screen Functions

var mainMenuScreen = function() {

    gameBackground();
    fill(0);
    textSize(50);
    text("Duck Life", 200, 75);
    fill(0, 75, 0);
    textSize(25);
    textSize(28);
    buttons[0].draw();
};

var settingsScreen = function() {

    background(skyBlue);
    fill(0);
    textSize(35);
    text("Settings", 200, 50);

    textAlign(LEFT, CENTER);
    textSize(20);

    fill(0);
    text("Difficulty:", 75, 150);
    picker(215, 150);
    textAlign(CENTER, CENTER);
    fill(0);

    textSize(18);
    if (difficulty === 0) {
        text("Easy", 265, 150);
        text("* A good difficulty for first time players. It\nwill take around 10 minutes to beat the game.\nYou start with more coins and the rewards for\nraces are higher.", 200, 250);
    }
    if (difficulty === 1) {
        text("Medium", 265, 150);
        text("* A good difficulty for players looking for\nmore of a challenge. It will take more time to\nbeat the game. You start with less coins and\nthe rewards for races are lower.", 200, 250);
    }
    if (difficulty === 2) {
        text("Hard", 265, 150);
        text("* A good difficulty for experienced players\nlooking for a challenge. It will take even more\ntime to beat the game. You start with even\nless coins and the rewards for races are\neven lower.", 200, 250);
    }

    if (difficulty === 0) {
        gameCoins = 250;
    }
    if (difficulty === 1) {
        gameCoins = 200;
    }
    if (difficulty === 2) {
        gameCoins = 100;
    }

    textAlign(CENTER, CENTER);

    textSize(28);
    buttons[24].draw();
};

var upgradeYourDuckScreen = function() {

    background(skyBlue);
    fill(0);
    textSize(35);
    text("Upgrade Your Duck", 200, 50);
    textSize(14);
    text("Coins: " + round(gameCoins), 200, 85);

    upgradeTable();

    textSize(28);
    buttons[1].draw();
};

var personalizeYourDuckScreen = function() {

    background(skyBlue);
    textSize(35);
    fill(0);
    text("Personalize Your Duck", 200, 40);

    for (var y = 75; y < 300; y += 50) {
        fill(225);
        rect(15, y, 100, 50);
        fill(255);
        rect(115, y, 170, 50);
    }

    textAlign(LEFT, CENTER);
    textSize(20);

    fill(0);
    text("Name:", 25, 100);
    textBox(128, 88);

    fill(0);
    text("Gender:", 25, 150);
    picker(150, 150);
    textAlign(CENTER, CENTER);
    fill(0);

    textSize(18);
    if (duck.gender === 0) {
        text("Male", 200, 150);
    }
    if (duck.gender === 1) {
        text("Female", 200, 150);
    }
    textSize(20);
    textAlign(LEFT, CENTER);

    fill(0);
    textSize(20);
    text("Height:", 25, 200);
    slider(150, 200, "Short", "Tall");
    stroke(0);
    strokeWeight(2);
    fill(255);
    ellipse(sliders.h, 200, 12, 12);

    if (mouseX > sliders.h - 10 && mouseX < sliders.h + 10 && mouseY > 190 && mouseY < 210) {
        fill(245);
        cursor(HAND);
        if (mouseIsPressed) {
            sliders.h = mouseX;

            if (sliders.h < 150) {
                sliders.h = 150;
            }
            if (sliders.h > 250) {
                sliders.h = 250;
            }
        }
    } else {
        fill(255);
    }

    duck.h = sliders.h / 1.85;

    fill(0);
    textSize(20);
    text("Weight:", 25, 250);
    slider(150, 250, "Thin", "Thick");
    stroke(0);
    strokeWeight(2);
    fill(255);
    ellipse(sliders.w, 250, 12, 12);

    if (mouseX > sliders.w - 10 && mouseX < sliders.w + 10 && mouseY > 240 && mouseY < 260) {
        fill(245);
        cursor(HAND);
        if (mouseIsPressed) {
            sliders.w = mouseX;

            if (sliders.w < 150) {
                sliders.w = 150;
            }
            if (sliders.w > 250) {
                sliders.w = 250;
            }
        }
    } else {
        fill(255);
    }

    duck.w = sliders.w / 3.08;

    fill(0);
    textSize(20);
    text("Hair:", 25, 300);
    picker(150, 300);
    textSize(18);
    textAlign(CENTER, CENTER);
    fill(0);
    text("Style " + duck.hairStyle, 200, 300);

    stroke(0);
    strokeWeight(3);
    fill(255);
    rect(295, 75, 95, 310);
    fill(225);
    rect(295, 75, 95, 35);
    rect(295, 282, 95, 35);
    fill(0);
    textSize(18);
    text("Preview", 345, 92);
    text("Color", 345, 300);
    drawDucks(340, 208, duck);

    for (var y = 325; y < 375; y += 20) {
        for (var x = 305; x < 375; x += 20) {
            colorSample(x, y, 15, 15, 2);
        }
    }

    fill(colors[0]);
    colorSample(305, 325, 15, 15, 0);
    fill(colors[1]);
    colorSample(325, 325, 15, 15, 1);
    fill(colors[2]);
    colorSample(345, 325, 15, 15, 2);
    fill(colors[3]);
    colorSample(365, 325, 15, 15, 3);
    fill(colors[4]);
    colorSample(305, 345, 15, 15, 4);
    fill(colors[5]);
    colorSample(325, 345, 15, 15, 5);
    fill(colors[6]);
    colorSample(345, 345, 15, 15, 6);
    fill(colors[7]);
    colorSample(365, 345, 15, 15, 7);
    fill(colors[8]);
    colorSample(305, 365, 15, 15, 8);
    fill(colors[9]);
    colorSample(325, 365, 15, 15, 9);
    fill(colors[10]);
    colorSample(345, 365, 15, 15, 10);
    fill(colors[11]);
    colorSample(365, 365, 15, 15, 11);

    textSize(28);
    buttons[2].draw();
};

var homeScreen = function() {

    background(skyBlue);
    fill(0);
    textSize(35);
    text("Home", 200, 50);
    textSize(14);
    text("Coins: " + round(gameCoins), 200, 85);

    if (buttons[22].over()) {
        duck.x = 200;
        duck.y = 320 - duck.h / 2;
        duck.pgv.timer = 350;
        duck.pgv.g = round(random(0, 5));
        duck.pgv.faceLeft = false;
        duck.pgv.headX = 5;
        duck.pgv.headS = 1;
        duck.pgv.danceTimer = 0;
    }

    textSize(18);
    buttons[3].draw();
    buttons[4].draw();
    buttons[5].draw();
    buttons[6].draw();
    buttons[7].draw();
    buttons[22].draw();
};

var practiceSelectScreen = function() {

    background(skyBlue);
    fill(0);
    textSize(35);
    text("Practice", 200, 50);

    textSize(20);
    buttons[8].draw();
    buttons[9].draw();
    buttons[10].draw();
    buttons[11].draw();
    buttons[12].draw();
    buttons[13].draw();


    if (buttons[8].over()) {
        g1.score = 0;
        g1.start = false;
        duck.x = 75;
        duck.y = 0;
        g1.platform.x = [50, random(125, 225), random(275, 375)];
        g1.platform.h = [random(125, 250), random(125, 250), random(125, 250)];
    }

    if (buttons[9].over()) {
        g2.score = 0;
        g2.start = false;
        duck.x = 200;
        duck.y = 340;
        g2.coin.x = [120, 200, 280];
        g2.coin.y = [random(-400, 150), random(-400, 150), random(-400, 150)];
        g2.coin.timer = 0;
        g2.x = [120, 200, 280];
        g2.y = [random(-400, 150), random(-400, 150), random(-400, 150)];
        g2.board.x = [110, 190, 270];
        g2.board.y = [random(-3000, 150), random(-3000, 150), random(-3000, 150)];
        g2.board.on = false;
        g2.board.timer = 1000;
        train1.x = 104;
        train1.y = random(-2000, -400);
        train1.l = round(random(1, 3));
        train1.s = round(random(2, 5));
        train2.x = 184;
        train2.y = random(-2000, -400);
        train2.l = round(random(1, 3));
        train2.s = round(random(2, 5));
        train3.x = 264;
        train3.y = random(-2000, -400);
        train3.l = round(random(1, 3));
        train3.s = round(random(2, 5));
    }

    if (buttons[10].over()) {
        g3.score = 0;
        g3.start = false;
        g3.penguinsLanded = 0;
        g3.timer = 0;
        g3.cannon.x = 200;
        g3.cannon.y = 300;
        g3.cannon.speed = 3;

        g3.snowball.x.splice(0, g3.snowball.x.length);
        g3.snowball.y.splice(0, g3.snowball.y.length);
        g3.snowball.speed.splice(0, g3.snowball.speed.length);

        penguins.x.splice(0, penguins.x.length);
        penguins.y.splice(0, penguins.y.length);
        penguins.speed.splice(0, penguins.speed.length);

        penguins.x[0] = random(35, 365);
        penguins.y[0] = random(-400, -100);
        penguins.speed[0] = 2;
    }

    if (buttons[11].over()) {
        g4.score = 0;
        g4.start = false;
        g4.timer = 0;

        balls.x.splice(0, balls.x.length);
        balls.y.splice(0, balls.y.length);
        balls.size.splice(0, balls.size.length);
        balls.sx.splice(0, balls.sx.length);
        balls.sy.splice(0, balls.sy.length);

        duck.x = 200;
        duck.y = 345 - duck.h / 4;
        duck.g4v.faceLeft = false;
    }

    if (buttons[12].over()) {
        g5.score = 0;
        g5.start = false;
        g5.obstacle.x = 1000;
        g5.obstacle.y = random(75, 275);
        g5.obstacle.h = 50;
        g5.obstacle.w = 20;
        g5.obstacle.s = 5;
        duck.x = 75;
        duck.y = 200;
        duck.g5v.fallSpeed = 5;
        duck.g5v.angle = 90;
    }

    if (buttons[13].over()) {
        g6.score = 0;
        g6.start = false;
        duck.x = 75;
        duck.y = 200;
        duck.g6v.speedY = 0;
        duck.g6v.fall = true;
        duck.g6v.click = false;
        spike.x[0] = random(425, 475);
        spike.x[1] = random(600, 650);
        spike.y[0] = 250;
        spike.y[1] = 250;
        spike.size[0] = round(random(1, 3));
        spike.size[1] = round(random(1, 3));
        spike.speedX = 3;
    }

    textSize(28);
    buttons[14].draw();
};

// Playground

var drawDuckPG = function(x, y) {

    fill(colors[duck.color]);
    ellipse(x, y, duck.w / 1.08, duck.h / 1.72);

    pushMatrix();
    translate(x + duck.pgv.headX, y - duck.h / 1.72 + 30);

    var rot = atan2(mouseY - (duck.y - duck.h / 1.72 + 30), mouseX - (duck.x + 5));

    if (duck.pgv.faceLeft === true) {
        rot = atan2((mouseY - (duck.y - duck.h / 1.72 + 30)) * -1, mouseX - (duck.x + 5)) - 180;
    }

    if (duck.pgv.faceLeft === false) {
        rot = constrain(rot, -90, 45);
    }
    if (duck.pgv.faceLeft === true) {

        if (rot * -1 > 90 && rot * -1 < 180) {
            rot = -90;
        }
        if (rot * -1 > 180 && rot * -1 < 315) {
            rot = -315;
        }
    }

    rotate(rot);

    hair(0, -46, duck.hairStyle);

    noStroke();
    fill(colors[duck.color]);
    ellipse(0, 0, 50, 50);
    fill(255);
    stroke(0);
    strokeWeight(2);
    ellipse(10, -5, 15, 15);
    fill(0);
    ellipse(12, -5, 8, 8);
    fill(235, 145, 45);
    strokeWeight(2);
    triangle(25, -5, 25, 5, 35, 0);

    if (items.buy[1] === true && items.wear[1] === true) {
        hat1(0, -55);
    }

    if (items.buy[3] === true && items.wear[3] === true) {
        hat2(0, -55);
    }

    popMatrix();

    stroke(0);
    strokeWeight(2);
    noFill();
    arc(x - 2, y - 15, 18, duck.h / 2, 10, 150);
    strokeWeight(3);
    strokeCap(SQUARE);
    line(x, y + duck.h / 3.5, x, y + duck.h / 2 + 1);
    line(x, y + duck.h / 2, x + 25, y + duck.h / 2);
    strokeCap(ROUND);

    if (items.buy[0] === true && items.wear[0] === true) {
        tie(x + 18, y - duck.h / 1.72 + 58);
    }

    if (items.buy[2] === true && items.wear[2] === true) {
        shoes(x + 5, y + duck.h / 2 - 132);
    }
};

Duck.prototype.drawPg = function() {

    pushMatrix();
    translate(this.x, this.y);
    if (this.pgv.faceLeft === true) {
        scale(-1, 1);
    }
    drawDuckPG(0, 0);
    popMatrix();
};

Duck.prototype.movePg = function() {

    if (keys[LEFT] && this.x - this.w / 2 > 0) {
        this.pgv.faceLeft = true;
        this.x -= 3;
    }
    if (keys[RIGHT] && this.x + this.w / 2 < 400) {
        this.pgv.faceLeft = false;
        this.x += 3;
    }
};

Duck.prototype.talkPg = function() {

    if (this.pgv.timer === 350) {
        this.pgv.q = round(random(0, 5));
        this.pgv.timer = 0;
    }

    this.pgv.timer++;

    if (this.pgv.timer > 175 && this.pgv.timer < 350) {

        pushMatrix();
        translate(this.x, this.y);

        if (this.pgv.faceLeft === true) {
            scale(-1, 1);
        }

        strokeWeight(2);
        fill(255);
        rectMode(CENTER);
        rect(112, -53, 125, 35, 10);
        rectMode(CORNER);
        triangle(51, -51, 60, -42, 45, -38);
        strokeWeight(5);
        stroke(255);
        strokeCap(SQUARE);
        line(+53, -51, +60, -43);
        strokeCap(ROUND);
        popMatrix();

        fill(0);
        textSize(12);

        if (this.pgv.faceLeft === true) {
            text(this.pgv.quotes[this.pgv.q], this.x - 112, this.y - 53);
        } else {
            text(this.pgv.quotes[this.pgv.q], this.x + 112, this.y - 53);
        }
    }
};

Duck.prototype.dancePg = function() {

    if (keys[32]) {
        this.pgv.danceTimer = 200;
    }

    if (this.pgv.danceTimer > 0) {
        if (this.pgv.headX > -10) {
            this.pgv.headS *= -1;
        }
        if (this.pgv.headX < 10) {
            this.pgv.headS *= -1;
        }

        this.pgv.headX += this.pgv.headS;

        this.pgv.danceTimer--;
    } else {
        this.pgv.headX = 5;
    }
};

var playgroundScreen = function() {

    gameBackground();
    fill(grassGreen);
    rect(0, 320, 400, 30);
    fill(0);
    textSize(35);
    text("Playground", 200, 50);

    duck.drawPg();
    duck.movePg();
    duck.talkPg();
    duck.dancePg();

    textSize(28);
    buttons[23].draw();
};

var gameOver = function(s) {

    stroke(0);
    strokeWeight(3);
    fill(255);
    rect(100, 60, 200, 135);
    textSize(35);
    fill(0);
    text("Game Over", 200, 85);
    textSize(20);
    text("Coins Earned: " + s, 200, 118);
    textSize(28);
    buttons[15].draw();
};

// Game 1 

Duck.prototype.drawg1 = function() {

    drawDucks(this.x, this.y, duck);
};

Duck.prototype.moveg1 = function() {

    if (keys[LEFT] && gameOverScreen === false) {
        for (var i = 0; i < g1.platform.x.length; i++) {
            g1.platform.x[i] += 10;
            g1.wave.x[0] += 2;
            g1.wave.x[1] += 2;
            g1.wave.x[2] += 2;
        }
    }
    if (keys[RIGHT] && gameOverScreen === false) {
        for (var i = 0; i < g1.platform.x.length; i++) {
            g1.platform.x[i] -= 10;
            g1.wave.x[0] -= 2;
            g1.wave.x[1] -= 2;
            g1.wave.x[2] -= 2;
        }
    }

    if (this.y > 400 && this.y < 450) {
        gameOverScreen = true;
    }
};

Duck.prototype.jumpg1 = function() {

    if (keys[UP] && this.g1v.numJumps < 1 && this.g1v.jumped === false && g1.start === true) {
        this.g1v.speedY = -this.g1v.jumpSpeed;
        this.g1v.numJumps++;
    }
    if (keys[UP] && g1.start === true) {
        this.g1v.jumped = true;
    } else {
        this.g1v.jumped = false;
    }

    this.y += this.g1v.speedY;

    if (this.g1v.speedY < 15) {
        this.g1v.speedY += g1.gravity;
    }
};

var drawPlatform = function(x, y, w, h) {

    noStroke();
    fill(135, 100, 35);
    rect(x, y, w, h);
    fill(grassGreen);
    rect(x, y, w, 10);

    if (duck.x + duck.w / 2 > x && duck.x < x + w && duck.y + duck.h / 2 > y - 8 && duck.y + duck.h / 2 < y + 8) {
        duck.y = y - duck.h / 2;
        duck.g1v.speedY = 0;
        duck.g1v.numJumps = 0;
    }
    if (duck.x + duck.w / 2 > x && duck.x - duck.w / 2 < x && duck.y + duck.h / 2 > y) {
        duck.x = x - duck.w / 2;
    }

    for (var i = 0; i < g1.platform.x.length; i++) {
        if (g1.platform.x[i] + 50 < 0) {
            g1.score++;
            gameCoins++;
            g1.platform.x[i] = random(400, 425);
            g1.platform.h[i] = random(125, 250);
        }
    }
};

var game1 = function() {

    gameBackground();
    fill(grassGreen);
    rect(0, 300, 400, 100);

    for (var i = g1.wave.x[0]; i < 400; i += 100) {
        fill(25, 135, 255);
        triangle(i, 400, i + 100, 400, i + 50, g1.wave.y[0]);
    }

    for (var i = 0; i < g1.platform.x.length; i++) {
        drawPlatform(g1.platform.x[i], 400 - g1.platform.h[i], 50, g1.platform.h[i]);
    }

    for (var i = g1.wave.x[1]; i < 400; i += 125) {
        fill(35, 105, 225);
        triangle(i, 400, i + 150, 400, i + 75, g1.wave.y[1]);
    }
    for (var i = g1.wave.x[2]; i < 400; i += 100) {
        fill(0, 165, 255);
        triangle(i, 400, i + 100, 400, i + 50, g1.wave.y[2]);
    }

    duck.drawg1();

    if (g1.start === true) {
        duck.moveg1();
    }

    duck.jumpg1();

    fill(255);
    textSize(50);
    text(g1.score, 200, 75);

    if (g1.wave.y[0] < 340) {
        g1.wave.speed[0] *= -1;
    }

    if (g1.wave.y[0] > 360) {
        g1.wave.speed[0] *= -1;
    }

    g1.wave.y[0] += g1.wave.speed[0];

    if (g1.wave.y[1] < 340) {
        g1.wave.speed[1] *= -1;
    }

    if (g1.wave.y[1] > 360) {
        g1.wave.speed[1] *= -1;
    }

    g1.wave.y[1] += g1.wave.speed[1];

    if (g1.wave.y[2] < 360) {
        g1.wave.speed[2] *= -1;
    }

    if (g1.wave.y[2] > 380) {
        g1.wave.speed[2] *= -1;
    }

    g1.wave.y[2] += g1.wave.speed[2];

    if (keys[32] && g1.start === false) {
        g1.start = true;
    }

    if (g1.start === false) {
        fill(255);
        textSize(20);
        text("1. Press the spacebar to start\n2. Use the left/right arrow keys to move\n3. Use the up arrow key to jump\n4. Don't fall", 200, 175);
    }

    if (gameOverScreen === true) {
        gameOver(g1.score);
    }
};

// Game 2

Duck.prototype.drawg2 = function() {

    pushMatrix();
    translate(this.x, this.y);

    if (this.g2v.faceLeft === true) {
        scale(-1, 1);
    }

    if (this.g2v.jumped === true && gameOverScreen === false && g2.start === true) {
        rotate(this.g2v.spin);
        this.g2v.spin += 12;
    }

    if (this.g2v.spin > 720) {
        this.g2v.jumped = false;
        this.g2v.spin = 0;
    }

    pushMatrix();
    scale(0.55);
    drawDucks(0, 0, duck);
    popMatrix();
    popMatrix();
};

var coins = function(x, y) {

    strokeWeight(3);
    stroke(235, 235, 0);
    fill(255, 255, 0);
    ellipse(x, y, 20, 20);
    noStroke();

    if (g2.coin.y[0] > 425) {
        g2.coin.x[0] = 120;
        g2.coin.y[0] = random(-400, -25);

    }
    if (g2.coin.y[1] > 425) {
        g2.coin.x[1] = 200;
        g2.coin.y[1] = random(-400, -25);

    }
    if (g2.coin.y[2] > 425) {
        g2.coin.x[2] = 280;
        g2.coin.y[2] = random(-400, -25);
    }

    if (duck.x + duck.w / 4 > x - 10 && duck.x - duck.w / 4 < x + 10 && duck.y + duck.h / 4 > y - 10 && duck.y - duck.h / 4 < y + 10 && g2.coin.x[0] === x && g2.coin.y[0] === y) {
        g2.coin.x[0] = 120;
        g2.coin.y[0] = random(-400, -25);
        g2.score++;
        gameCoins++;
    }
    if (duck.x + duck.w / 4 > x - 10 && duck.x - duck.w / 4 < x + 10 && duck.y + duck.h / 4 > y - 10 && duck.y - duck.h / 4 < y + 10 && g2.coin.x[1] === x && g2.coin.y[1] === y) {
        g2.coin.x[1] = 200;
        g2.coin.y[1] = random(-400, -25);
        g2.score++;
        gameCoins++;
    }
    if (duck.x + duck.w / 4 > x - 10 && duck.x - duck.w / 4 < x + 10 && duck.y + duck.h / 4 > y - 10 && duck.y - duck.h / 4 < y + 10 && g2.coin.x[2] === x && g2.coin.y[2] === y) {
        g2.coin.x[2] = 280;
        g2.coin.y[2] = random(-400, -25);
        g2.score++;
        gameCoins++;
    }
};

var holes = function(x, y) {

    fill(75);
    ellipse(x, y, 28, 20);
    noStroke();

    if (g2.y[0] > 425) {
        g2.x[0] = 120;
        g2.y[0] = random(-400, -25);

    }
    if (g2.y[1] > 425) {
        g2.x[1] = 200;
        g2.y[1] = random(-400, -25);
    }
    if (g2.y[2] > 425) {
        g2.x[2] = 280;
        g2.y[2] = random(-400, -25);
    }

    if (g2.y[0] + 15 > g2.coin.y[0] - 15 && g2.y[0] - 15 < g2.coin.y[0] + 15) {
        g2.y[0] -= 75;
    }
    if (g2.y[1] + 15 > g2.coin.y[1] - 15 && g2.y[1] - 15 < g2.coin.y[1] + 15) {
        g2.y[1] -= 75;
    }
    if (g2.y[2] + 15 > g2.coin.y[2] - 15 && g2.y[2] - 15 < g2.coin.y[2] + 15) {
        g2.y[2] -= 75;
    }

    if (duck.x + duck.w / 4 > x - 10 && duck.x - duck.w / 4 < x + 10 && duck.y + duck.h / 4 > y - 14 && duck.y - duck.h / 4 < y + 14 && duck.g2v.jumped === false && g2.board.on === false) {
        gameOverScreen = true;
    }
};

var boards = function(x, y) {

    fill(50, 150, 255);
    rect(x, y, 20, 50, 10);
    fill(0, 255, 0);
    rect(x + 6, y, 8, 50, 5);
    noStroke();

    if (g2.board.y[0] > 425) {
        g2.board.x[0] = 110;
        g2.board.y[0] = random(-3000, -25);

    }
    if (g2.board.y[1] > 425) {
        g2.board.x[1] = 190;
        g2.board.y[1] = random(-3000, -25);
    }
    if (g2.board.y[2] > 425) {
        g2.board.x[2] = 270;
        g2.board.y[2] = random(-3000, -25);
    }

    if (duck.x + duck.w / 4 > x && duck.x - duck.w / 4 < x + 20 && duck.y + duck.h / 4 > y && duck.y - duck.h / 4 < y + 50 && g2.board.x[0] === x) {
        g2.board.on = true;
        g2.board.timer = 1000;
        g2.board.x[0] = 110;
        g2.board.y[0] = random(-3000, -25);
    }
    if (duck.x + duck.w / 4 > x && duck.x - duck.w / 4 < x + 20 && duck.y + duck.h / 4 > y && duck.y - duck.h / 4 < y + 50 && g2.board.x[1] === x) {
        g2.board.on = true;
        g2.board.x[1] = 190;
        g2.board.y[1] = random(-3000, -25);
    }
    if (duck.x + duck.w / 4 > x && duck.x - duck.w / 4 < x + 20 && duck.y + duck.h / 4 > y && duck.y - duck.h / 4 < y + 50 && g2.board.x[2] === x) {
        g2.board.on = true;
        g2.board.x[2] = 270;
        g2.board.y[2] = random(-3000, -25);
    }

    if (g2.board.on === true) {
        g2.board.timer--;

        if (g2.board.timer < 0) {
            g2.board.on = false;
        }
    }
};

Train.prototype.draw = function() {

    if (this.y - this.l > 400) {
        this.y = random(-2000, -400);
        this.l = round(random(1, 3));
        this.s = round(random(2, 5));
    }

    if (this.l === 1) {
        this.l = 208;
    }
    if (this.l === 2) {
        this.l = 316;
    }
    if (this.l === 3) {
        this.l = 424;
    }
    if (this.s === 5) {
        this.s = 8;
    }

    for (var i = 0; i <= this.l; i += 108) {
        fill(35, 135, 215);
        rect(this.x, i + this.y, 30, 100);
        fill(125, 175, 215);
        rect(this.x + 5, i + this.y + 5, 20, 90);
    }

    for (var i = 100; i < this.l; i += 108) {
        fill(210, 215, 125);
        rect(this.x + 5, i + this.y, 20, 8);
    }

    if (g2.start === true && gameOverScreen === false) {
        this.y += this.s;
    }

    if (duck.x + duck.w / 4 > this.x && duck.x - duck.w / 4 < this.x + 30 && duck.y + duck.h / 4 > this.y && duck.y - duck.h / 4 < this.y + this.l && g2.board.on === false) {
        gameOverScreen = true;
    }
};

var trainTrack = function(x) {

    noStroke();
    fill(150);
    rect(x, 0, 5, 400);
    rect(x + 50, 0, 5, 400);

    for (var i = -20; i < 400; i += 30) {
        fill(170, 125, 40);
        rect(x - 5, i + g2.tracksY, 65, 5);
    }

    if (g2.tracksY > 15) {
        g2.tracksY = -15;
    }
};

var game2Background = function() {

    background(165, 225, 75);

    for (var y = -25; y < 400; y += 50) {
        fill(180, 235, 85);
        rect(0, y + g2.grassY, 400, 25);
    }

    if (g2.grassY > 25) {
        g2.grassY = -25;
    }

    fill(115);
    rect(75, 0, 250, 400);

    for (var x = 92; x < 300; x += 80) {
        trainTrack(x);
    }
};

var game2 = function() {

    noStroke();
    game2Background();

    if (keys[32] && g2.start === false) {
        g2.start = true;
    }

    if (g2.start === true && gameOverScreen === false) {
        g2.tracksY += 2;
        g2.grassY += 2;
    }

    for (var i = 0; i < g2.x.length; i++) {
        holes(g2.x[i], g2.y[i]);

        if (g2.start === true && gameOverScreen === false) {
            g2.y[i] += 2;
        }
    }
    for (var i = 0; i < g2.board.x.length; i++) {
        boards(g2.board.x[i], g2.board.y[i]);

        if (g2.start === true && gameOverScreen === false) {
            g2.board.y[i] += 2;
        }
    }

    train1.draw();
    train2.draw();
    train3.draw();

    for (var i = 0; i < g2.coin.x.length; i++) {
        coins(g2.coin.x[i], g2.coin.y[i]);

        if (g2.start === true && gameOverScreen === false) {
            g2.coin.y[i] += 2;
        }
    }

    duck.drawg2();

    if (g2.start === false) {
        fill(255);
        textSize(20);
        text("1. Press the spacebar to start\n2. Use the left/right arrow keys to move\n3. Use the up arrow key to jump over\nthe potholes\n4. Avoid the trains and potholes\n5. Collect hoverboards to make yourself\ninvincible for a short period of time\n6. Try to collect coins", 200, 185);
    }

    fill(255);
    textSize(50);
    text(g2.score, 200, 50);

    if (gameOverScreen === true) {
        gameOver(g2.score);
    }
};

// Game 3

Penguin.prototype.draw = function() {

    for (var i = 0; i < this.x.length; i++) {

        stroke(0);
        strokeWeight(2);
        line(this.x[i] - 25, this.y[i] - 50, this.x[i], this.y[i]);
        line(this.x[i] - 10, this.y[i] - 50, this.x[i], this.y[i]);
        line(this.x[i] + 8, this.y[i] - 50, this.x[i], this.y[i]);
        line(this.x[i] + 25, this.y[i] - 50, this.x[i], this.y[i]);
        fill(0);
        ellipse(this.x[i], this.y[i], 20, 20);
        strokeWeight(5);
        line(this.x[i] - 10, this.y[i] - 5, this.x[i] - 20, this.y[i] + 5);
        line(this.x[i] + 10, this.y[i] - 5, this.x[i] + 20, this.y[i] + 5);
        noStroke();
        ellipse(this.x[i], this.y[i], 25, 35);
        fill(255);
        ellipse(this.x[i], this.y[i], 15, 25);
        fill(0);
        ellipse(this.x[i], this.y[i] - 25, 23, 25);
        fill(255, 100, 0);
        ellipse(this.x[i] - 5, this.y[i] + 18, 8, 6);
        ellipse(this.x[i] + 5, this.y[i] + 18, 8, 6);
        fill(255);
        ellipse(this.x[i] - 2, this.y[i] - 25, 8, 8);
        ellipse(this.x[i] + 2, this.y[i] - 25, 8, 8);
        fill(0);
        ellipse(this.x[i] - 2, this.y[i] - 25, 2, 4);
        ellipse(this.x[i] + 2, this.y[i] - 25, 2, 4);
        fill(255, 100, 0);
        triangle(this.x[i] - 4, this.y[i] - 20, this.x[i] + 4, this.y[i] - 20, this.x[i], this.y[i] - 15);
        stroke(0);
        strokeWeight(2);
        fill(255);
        arc(this.x[i], this.y[i] - 50, 50, 50, -179, 0);
    }
};

Penguin.prototype.move = function() {

    for (var i = 0; i < this.x.length; i++) {
        if (this.y[i] + 35 < 360 && g3.start === true) {
            this.y[i] += this.speed[i];
        }
    }
};

Penguin.prototype.hitbysnowball = function() {

    for (var i = 0; i < this.x.length; i++) {

        for (var j = 0; j < g3.snowball.x.length; j++) {

            if (g3.snowball.x[j] + 10 > this.x[i] - 35 && g3.snowball.x[j] - 10 < this.x[i] + 30 && g3.snowball.y[j] - 10 < this.y[i] + 25 && this.y[i] + 15 < 300) {
                this.x.splice(i, 1);
                this.y.splice(i, 1);
                this.speed.splice(i, 1);
                g3.snowball.x.splice(i, 1);
                g3.snowball.y.splice(i, 1);
                g3.snowball.speed.splice(i, 1);
                g3.score++;
                gameCoins++;
            }
        }

        if (gameOverScreen === false && this.y[i] > 320 && this.y[i] < 323) {
            g3.penguinsLanded++;
        }
    }

    if (g3.penguinsLanded === 3) {

        gameOverScreen = true;
        g3.cannon.speed = 0;

        for (var i = 0; i < penguins.x.length; i++) {
            penguins.speed[i] = 0;
        }
        for (var i = 0; i < g3.snowball.x.length; i++) {
            g3.snowball.speed[i] = 0;
        }
    }
};

var cannonFunction = function(x, y) {

    stroke(0);
    strokeWeight(2);
    fill(200);
    rect(x - 11, y + 15, 20, 10);
    rect(x - 15, y, 28, 15);
    arc(x, y + 48, 50, 50, -179, 0);
    line(x - 25, y + 48, x + 23, y + 48);

    if (keys[LEFT] && g3.cannon.x > 35) {
        g3.cannon.x -= g3.cannon.speed;
    }

    if (keys[RIGHT] && g3.cannon.x < 365) {
        g3.cannon.x += g3.cannon.speed;
    }
};

var snowballFunction = function() {

    for (var i = 0; i < g3.snowball.x.length; i++) {

        fill(255);
        strokeWeight(2);
        stroke(0);
        ellipse(g3.snowball.x[i], g3.snowball.y[i], 20, 20);

        g3.snowball.y[i] -= g3.snowball.speed[i];

        if (g3.snowball.y[i] < 0) {
            g3.snowball.x.splice(i, 1);
            g3.snowball.y.splice(i, 1);
            g3.snowball.speed.splice(i, 1);
        }

        if (g3.snowball.y[i] > 300) {

            if (keys[LEFT] && g3.cannon.x > 35) {
                g3.snowball.x[i] -= g3.cannon.speed;
            }
            if (keys[RIGHT] && g3.cannon.x < 365) {
                g3.snowball.x[i] += g3.cannon.speed;
            }
        }
    }
};

var game3 = function() {

    background(skyBlue);
    fill(255);
    noStroke();
    rect(0, 350, 400, 50);
    stroke(0);
    strokeWeight(4);
    line(0, 350, 400, 350);

    penguins.draw();
    penguins.move();
    penguins.hitbysnowball();

    if (keys[32] && g3.start === false) {
        g3.start = true;
    }

    if (g3.start === false) {
        fill(255);
        textSize(20);
        text("1. Press the spacebar to start\n2. Use the left/right arrow keys to move\n3. Click anywhere to shoot\n4. If you let 3 penguins land you lose", 200, 175);
    }

    if (g3.timer === 75) {
        penguins.x.push(random(35, 365));
        penguins.y.push(random(-400, -100));
        penguins.speed.push(3);
        g3.timer = 0;
    }

    if (g3.start === true) {
        g3.timer++;
    }

    snowballFunction();
    cannonFunction(g3.cannon.x, g3.cannon.y);

    textSize(50);
    fill(255);
    text(g3.score, 200, 75);
    textSize(18);
    fill(0);
    text("Penguins Landed: " + g3.penguinsLanded, 200, 375);

    if (gameOverScreen === true) {
        gameOver(g3.score);
    }
};

// Game 4

Duck.prototype.drawg4 = function() {

    pushMatrix();
    translate(this.x, this.y);
    if (this.g4v.faceLeft === true) {
        scale(-1, 1);
    }
    scale(0.55);
    drawDucks(0, 0, duck);
    popMatrix();
    popMatrix();
};

Duck.prototype.moveg4 = function() {

    if (keys[LEFT] && gameOverScreen === false && this.x - this.w / 4 > 10) {
        this.x -= 5;
        this.g4v.faceLeft = true;
    }
    if (keys[RIGHT] && gameOverScreen === false && this.x + this.w / 4 < 390) {
        this.x += 5;
        this.g4v.faceLeft = false;
    }
};

Ball.prototype.draw = function() {

    fill(200);
    strokeWeight(3);
    stroke(150);
    for (var i = 0; i < this.x.length; i++) {
        ellipse(this.x[i], this.y[i], this.size[i], this.size[i]);
    }
};

Ball.prototype.move = function() {

    for (var i = 0; i < this.x.length; i++) {
        this.x[i] += this.sx[i];
        this.y[i] += this.sy[i];

        if (this.x[i] + this.size[i] / 2 > 600) {
            this.sx[i] = this.sx[i] * -1;
        }

        if (this.y[i] + this.size[i] / 2 > 350) {
            this.sy[i] = this.sy[i] * -1;
        }

        if (this.x[i] - this.size[i] / 2 < -200) {
            this.sx[i] = this.sx[i] * -1;
        }

        if (this.y[i] < 200) {
            this.sy[i] = this.sy[i] * -1;
        }
    }
};

Ball.prototype.hit = function() {

    for (var i = 0; i < this.x.length; i++) {
        if (duck.x + duck.w / 4 > this.x[i] - this.size[i] / 2 && duck.x - duck.w / 4 < this.x[i] + this.size[i] / 2 && this.y[i] + this.size[i] / 2 > duck.y - duck.h / 4) {
            gameOverScreen = true;

            for (var j = 0; j < this.x.length; j++) {
                balls.sx[j] = 0;
                balls.sy[j] = 0;
            }
        }
    }
};

var game4 = function() {

    gameBackground();

    if (keys[32] && g4.start === false) {
        g4.start = true;
    }

    if (g4.start === false) {
        fill(255);
        textSize(20);
        text("1. Press the spacebar to start\n2. Use the left/right arrows key to move\n3. Try to avoid the balls", 200, 175);
    }

    duck.drawg4();
    duck.moveg4();

    if (g4.start === true) {
        if (g4.timer === 150) {
            balls.x.push(random(-100, -35));
            balls.y.push(200);
            balls.size.push(random(10, 50));
            balls.sx.push(random(2, 4));
            balls.sy.push(random(3, 6));
            g4.timer = 0;
        }

        g4.timer++;
    }

    balls.draw();
    balls.move();
    balls.hit();

    textSize(50);
    fill(255);
    text(round(g4.score), 200, 75);

    if (gameOverScreen === false && g4.start === true) {
        g4.score += 0.01;
        gameCoins += 0.01;
    }

    if (gameOverScreen === true) {
        gameOver(round(g4.score));
    }
};

// Game 5

Duck.prototype.drawg5 = function() {

    pushMatrix();
    translate(this.x, this.y);

    if (g5.start === true) {
        if (mouseIsPressed && gameOverScreen === false) {
            this.g5v.angle = 75;
        } else {
            this.g5v.angle = 105;
        }
    }

    rotate(this.g5v.angle);
    scale(0.55);
    drawDucks(0, 0, duck);
    popMatrix();
    popMatrix();
};

Duck.prototype.flyg5 = function() {

    if (mouseIsPressed) {
        this.g5v.fallSpeed = -5;
    } else {
        this.g5v.fallSpeed = 5;
    }

    if (gameOverScreen === false) {
        this.y += this.g5v.fallSpeed;
    }

    if (this.y + this.w / 4 < 0) {
        gameOverScreen = true;
    }

    if ((this.y + this.w / 4 > 350) || (this.x + this.h / 4 > g5.obstacle.x && this.x - this.h / 4 < g5.obstacle.x + g5.obstacle.w && this.y + this.w / 4 > g5.obstacle.y && this.y - this.w / 4 < g5.obstacle.y + g5.obstacle.h)) {
        gameOverScreen = true;
    }
};

var obstacles = function() {

    strokeWeight(3);
    stroke(150);
    fill(200);
    rect(g5.obstacle.x, g5.obstacle.y, g5.obstacle.w, g5.obstacle.h);

    g5.obstacle.x -= g5.obstacle.s;

    if (g5.obstacle.x < -20) {
        g5.obstacle.x = 440;
        g5.obstacle.y = random(75, 275);

        if (gameOverScreen === false) {
            g5.score++;
            gameCoins++;
        }
    }
};

var game5 = function() {

    gameBackground();

    duck.drawg5();

    if (keys[32] && g5.start === false) {
        g5.start = true;
    }

    if (g5.start === false) {
        fill(255);
        textSize(20);
        text("1. Press the spacebar to start\n2. Press the mouse to fly, release to fall\n3. Try to avoid the obstacles and don't\nfly off the top of the screen", 200, 175);
    }

    if (g5.start === true) {
        duck.flyg5();
        obstacles();
    }

    fill(255);
    textSize(50);
    text(g5.score, 200, 75);

    if (gameOverScreen === true) {
        duck.fallSpeed = 0;
        g5.obstacle.s = 0;
        gameOver(g5.score);
    }
};

// Game 6

Duck.prototype.drawg6 = function() {

    pushMatrix();
    translate(this.x, this.y);
    scale(0.55);
    drawDucks(0, 0, duck);
    popMatrix();
};

Duck.prototype.bounceg6 = function() {

    if (this.y + this.h / 4 > 245) {

        this.g6v.fall = false;

        if (this.g6v.click === true) {
            this.g6v.click = false;
        }
    }

    if (this.y - this.h / 4 < 125) {
        this.g6v.fall = true;
    }

    if (this.g6v.fall === true) {
        this.g6v.speedY = this.y / 25 - 5;
    } else {
        this.g6v.speedY = -(this.y / 25 - 5);
    }

    if (this.g6v.click === true) {
        this.y += (this.g6v.speedY + 5);
        this.g6v.fall = true;
    } else {
        this.y += this.g6v.speedY;
    }
};

Duck.prototype.hitSpikeg6 = function() {

    for (var i = 0; i < spike.x.length; i++) {

        if ((this.x + this.w / 4 > spike.x[i] - 10 && this.x - this.w / 4 < spike.x[i] + 10 && this.y + this.h / 4 > spike.y[i] - 25 && spike.size[i] === 1) || (this.x + this.w / 4 > spike.x[i] - 10 && this.x - this.w / 4 < spike.x[i] + 30 && this.y + this.h / 4 > spike.y[i] - 25 && spike.size[i] === 2) || (this.x + this.w / 4 > spike.x[i] - 10 && this.x - this.w / 4 < spike.x[i] + 50 && this.y + this.h / 4 > spike.y[i] - 25 && spike.size[i] === 3)) {
            gameOverScreen = true;
        }
    }
};

var game6 = function() {

    background(skyBlue);

    noStroke();
    fill(grassGreen);
    rect(0, 250, width, 150);

    fill(255);
    textSize(50);
    text(g6.score, 200, 75);

    duck.drawg6();
    duck.hitSpikeg6();

    if (gameOverScreen === false) {
        duck.bounceg6();
    }

    spike.draw();
    spike.move();

    if (keys[32] && g6.start === false) {
        g6.start = true;
    }

    if (g6.start === false) {
        fill(255);
        textSize(20);
        text("1. Press the spacebar to start\n2. Click anywhere to bounce\n3. Try to jump over the spikes", 200, 175);
    }

    if (gameOverScreen === true) {

        gameOver(g6.score);
        spike.speedX = 0;
        duck.g6v.speedY = 0;
    }
};

// Race Functions

var raceOver = function() {

    stroke(0);
    strokeWeight(3);
    fill(255);
    rect(100, 100, 200, 135);
    textSize(35);
    fill(0);
    text("You " + race.outcome + "!", 200, 125);
    if (race.outcome === "Won") {
        textSize(18);
        text("Coins Earned: " + race.reward, 200, 158);
    } else {
        textSize(12);
        text("Upgrade your duck and try again!", 200, 158);
    }
    textSize(28);
    buttons[18].draw();
};

var land = function(x, y, w, h) {

    noStroke();
    fill(grassGreen);
    rect(x, y, w, h);
};

var water = function(x, y, w, h) {

    noStroke();
    fill(waterBlue);
    rect(x, y, w, h);

    triangle(x, y, x, y + h, x - 75, y);
    triangle(x + w, y, x + w, y + h, x + w + 75, y);

    fill(245, 215, 135);
    rect(x, y + h - 12, w, 12);
    triangle(x, y + h - 12, x, y + h, x - 12, y + h - 12);
    triangle(x + w, y + h - 12, x + w, y + h, x + w + 12, y + h - 12);
};

var sky = function(x, y, w, h) {

    noStroke();
    fill(skyBlue);
    rect(x, y, w, h);
};

var flag = function(x, y) {

    stroke(0);
    strokeWeight(3);
    line(x, y, x, y + 75);
    fill(255);
    strokeWeight(2);
    triangle(x, y, x, y + 25, x - 35, y + 12);
};

var lock = function(x, y) {

    noStroke();
    fill(200);
    rect(x, y, 30, 25);
    fill(0);
    ellipse(x + 15, y + 10, 8, 8);
    triangle(x + 15, y + 10, x + 10, y + 20, x + 20, y + 20);
    stroke(200);
    noFill();
    arc(x + 15, y, 16, 18, 181, 360);
};

var raceIcon = function(x, y, unlock, raceNum) {

    if (mouseX > x && mouseX < x + 50 && mouseY > y && mouseY < y + 50) {
        fill(245);
        cursor(HAND);
    } else {
        fill(255);
    }

    stroke(0);
    strokeWeight(3);
    rect(x, y, 50, 50);

    if (unlock !== true) {
        lock(x + 10, y + 18);
    } else {
        fill(0);
        textSize(35);
        text(raceNum, x + 25, y + 25);
    }

    if (mouseIsPressed && mouseX > x && mouseX < x + 50 && mouseY > y && mouseY < y + 50) {

        switch (raceNum) {
            case 1:
                race.raceNumber = 1;
                race.type = "Running";
                race.opponent = "Donald";
                if (difficulty === 0) {
                    race.reward = 75;
                }
                if (difficulty === 1) {
                    race.reward = 50;
                }
                if (difficulty === 2) {
                    race.reward = 35;
                }
                break;
            case 2:
                race.raceNumber = 2;
                race.type = "Swimming";
                race.opponent = "Bullet";
                if (difficulty === 0) {
                    race.reward = 150;
                }
                if (difficulty === 1) {
                    race.reward = 125;
                }
                if (difficulty === 2) {
                    race.reward = 100;
                }
                break;
            case 3:
                race.raceNumber = 3;
                race.type = "Flying";
                race.opponent = "Daffy";
                if (difficulty === 0) {
                    race.reward = 200;
                }
                if (difficulty === 1) {
                    race.reward = 175;
                }
                if (difficulty === 2) {
                    race.reward = 150;
                }
                break;
            case 4:
                race.raceNumber = 4;
                race.type = "All Skills";
                race.opponent = "Zippy";
                if (difficulty === 0) {
                    race.reward = 300;
                }
                if (difficulty === 1) {
                    race.reward = 275;
                }
                if (difficulty === 2) {
                    race.reward = 250;
                }
                break;
            case 5:
                race.raceNumber = 5;
                race.type = "Running";
                race.opponent = "Speedy";
                if (difficulty === 0) {
                    race.reward = 400;
                }
                if (difficulty === 1) {
                    race.reward = 375;
                }
                if (difficulty === 2) {
                    race.reward = 350;
                }
                break;
            case 6:
                race.raceNumber = 6;
                race.type = "Swimming";
                race.opponent = "Blazer";
                if (difficulty === 0) {
                    race.reward = 500;
                }
                if (difficulty === 1) {
                    race.reward = 475;
                }
                if (difficulty === 2) {
                    race.reward = 450;
                }
                break;
            case 7:
                race.raceNumber = 7;
                race.type = "Flying";
                race.opponent = "Kevin";
                if (difficulty === 0) {
                    race.reward = 600;
                }
                if (difficulty === 1) {
                    race.reward = 575;
                }
                if (difficulty === 2) {
                    race.reward = 550;
                }
                break;
            case 8:
                race.raceNumber = 8;
                race.type = "All Skills";
                race.opponent = "Pablo";
                race.reward = 1000;
                break;
        }
    }

    textSize(50);
    fill(0, 255, 0, 100);

    for (var i = 0; i < 4; i++) {
        if (race.beaten[i] === true) {
            text("✓", 85 + i * 75, 125);
        }
    }

    for (var i = 0; i < 4; i++) {
        if (race.beaten[i + 4] === true) {
            text("✓", 85 + i * 75, 200);
        }
    }
};

var raceSelectScreen = function() {

    background(skyBlue);
    fill(0);
    textSize(35);
    text("Race Select", 200, 50);

    raceIcon(60, 100, race.unlockRace[0], 1);
    raceIcon(135, 100, race.unlockRace[1], 2);
    raceIcon(210, 100, race.unlockRace[2], 3);
    raceIcon(285, 100, race.unlockRace[3], 4);
    raceIcon(60, 175, race.unlockRace[4], 5);
    raceIcon(135, 175, race.unlockRace[5], 6);
    raceIcon(210, 175, race.unlockRace[6], 7);
    raceIcon(285, 175, race.unlockRace[7], 8);

    fill(255);
    stroke(0);
    rect(35, 240, 330, 65);
    strokeCap(SQUARE);
    line(35, 273, 365, 273);
    line(165, 240, 165, 305);
    strokeCap(ROUND);
    textSize(16);
    fill(0);
    textAlign(LEFT, CENTER);
    text("Level: " + race.raceNumber, 50, 255);
    text("Type of Race: " + race.type, 178, 255);
    text("Reward: " + race.reward, 50, 288);
    text("Opponent: " + race.opponent, 178, 288);
    textAlign(CENTER, CENTER);

    if (buttons[17].over()) {

        if (race.raceNumber !== 8) {
            duckAI.h = random(85, 135);
            duckAI.w = random(50, 80);
            duckAI.color = round(random(0, 11));
            duckAI.hairStyle = round(random(1, 8));
        } else {
            duckAI.h = 112;
            duckAI.w = 70;
            duckAI.hairStyle = 0;
        }

        duckAI.x = 50;
        duckAI.y = 125 - duckAI.h / 1.32;
        duck.x = 50;
        duck.y = 325 - duck.h / 1.32;

        switch (race.raceNumber) {
            case 1:
                duckAI.runLevel = 4;
                duckAI.swimLevel = 4;
                duckAI.energyLevel = 4;
                duckAI.flyLevel = 4;
                duckAI.rotation = 0;
                duck.rotation = 0;
                break;
            case 2:
                duckAI.runLevel = 4;
                duckAI.swimLevel = 4;
                duckAI.energyLevel = 4;
                duckAI.flyLevel = 4;
                duckAI.rotation = 0;
                duck.rotation = 0;
                break;
            case 3:
                duckAI.runLevel = 4;
                duckAI.swimLevel = 4;
                duckAI.energyLevel = 4;
                duckAI.flyLevel = 4;
                duckAI.rotation = 0;
                duck.rotation = 0;
                break;
            case 4:
                duckAI.runLevel = 4;
                duckAI.swimLevel = 4;
                duckAI.energyLevel = 4;
                duckAI.flyLevel = 4;
                duckAI.rotation = 0;
                duck.rotation = 0;
                break;
            case 5:
                duckAI.runLevel = 8;
                duckAI.swimLevel = 8;
                duckAI.energyLevel = 8;
                duckAI.flyLevel = 8;
                duckAI.rotation = 0;
                duck.rotation = 0;
                break;
            case 6:
                duckAI.runLevel = 9;
                duckAI.swimLevel = 9;
                duckAI.energyLevel = 9;
                duckAI.flyLevel = 9;
                duckAI.rotation = 0;
                duck.rotation = 0;
                break;
            case 7:
                duckAI.runLevel = 10;
                duckAI.swimLevel = 10;
                duckAI.energyLevel = 10;
                duckAI.flyLevel = 10;
                duckAI.rotation = 0;
                duck.rotation = 0;
                break;
            case 8:
                duckAI.runLevel = 12;
                duckAI.swimLevel = 12;
                duckAI.energyLevel = 12;
                duckAI.flyLevel = 12;
                duckAI.rotation = 0;
                duck.rotation = 0;
                break;
        }

        energy.remaining = 100;
    }

    textSize(28);
    buttons[16].draw();
    buttons[17].draw();

    if (buttons[17].over() && mouseIsPressed) {
        if (race.unlockRace[race.raceNumber - 1] === false) {
            showErrorMessage[2] = true;
        }
        if (race.beaten[race.raceNumber - 1] === true) {
            showErrorMessage[3] = true;
        }
    }
};

var energySystem = function() {

    textSize(20);
    fill(255);
    text("Energy Remaining: " + round(energy.remaining) + "%", 200, 250);

    if (energy.remaining > 0 && raceOverScreen === false) {
        energy.remaining -= (energy.loss / duck.energyLevel) / 2;
    }
};

var winningAndLosingRace = function(n, d) {

    var percentCompletedAI = round((abs(race.x[n]) + duckAI.x - 50) / d);
    var percentCompletedPlayer = round((abs(race.x[n + 1]) + duck.x - 50) / d);

    textSize(20);
    fill(255);
    text("Completed: " + percentCompletedAI + "%", 85, 25);
    text("Completed: " + percentCompletedPlayer + "%", 85, 225);

    if (percentCompletedPlayer > percentCompletedAI) {
        text("Place: 2nd", 250, 25);
        text("Place: 1st", 250, 225);
    }

    if (percentCompletedAI > percentCompletedPlayer) {
        text("Place: 1st", 250, 25);
        text("Place: 2nd", 250, 225);
    }
    if (percentCompletedPlayer === percentCompletedAI) {
        text("Place: Tied", 250, 25);
        text("Place: Tied", 250, 225);
    }

    text("CPU", 350, 25);
    text("You", 350, 225);

    if (energy.remaining === 0 && raceOverScreen === false) {
        raceOverScreen = true;
        race.outcome = "Lost";
    }

    if (percentCompletedAI >= 100 || percentCompletedPlayer >= 100) {
        raceOverScreen = true;
    }

    if (percentCompletedPlayer >= 100 && percentCompletedAI < 100 && raceOverScreen === true) {
        race.outcome = "Won";
    }

    if (percentCompletedAI >= 100 && percentCompletedPlayer < 100 && raceOverScreen === true) {
        race.outcome = "Lost";
    }

    if (percentCompletedAI >= 100 && percentCompletedPlayer >= 100 && raceOverScreen === true) {
        race.outcome = "Tied";
    }
};

Duck.prototype.raceDrawPlayer = function() {

    pushMatrix();
    translate(this.x, this.y + this.h / 2);
    scale(0.5);
    rotate(this.rotation);
    drawDucks(0, 0, duck);
    popMatrix();
};

Duck.prototype.raceDrawAI = function() {

    pushMatrix();
    translate(this.x, this.y + this.h / 2);
    scale(0.5);
    rotate(this.rotation);
    if (race.raceNumber === 8) {
        drawPablo(0, 0);
    } else {
        drawDucks(0, 0, duckAI);
    }
    popMatrix();
};

Duck.prototype.raceMove1 = function(i) {

    if (raceOverScreen === false) {
        if (race.x[i] > -600) {
            race.x[i] -= this.runLevel;
        }

        if (race.x[i] <= -600 && this.x < 350) {
            this.x += this.runLevel;
        }
    }
};

Duck.prototype.raceMove2 = function(i) {

    if (raceOverScreen === false) {

        if (race.x[i] > -100) {
            race.x[i] -= this.runLevel;
        }
        if (race.x[i] <= -100 && race.x[i] > -1095) {
            race.x[i] -= this.swimLevel;
        }
        if (race.x[i] <= -1095 && this.x < 350) {
            this.x += this.runLevel;
        }
        if (race.x[i] < -100 && race.x[i] > -1090) {
            if (i === 2) {
                this.y = 150 - this.h / 1.32;
            }
            if (i === 3) {
                this.y = 350 - this.h / 1.32;
            }
        }
        if (race.x[i] < -1090) {
            if (i === 2) {
                this.y = 125 - this.h / 1.32;
            }
            if (i === 3) {
                this.y = 325 - this.h / 1.32;
            }
        }
    }
};

Duck.prototype.raceMove3 = function(i) {

    if (raceOverScreen === false) {

        if (race.x[i] > -100) {
            race.x[i] -= this.runLevel;
        }
        if (race.x[i] <= -100 && race.x[i] > -1095) {
            race.x[i] -= this.flyLevel;
        }
        if (race.x[i] <= -1095 && this.x < 350) {
            this.x += this.runLevel;
        }
        if (race.x[i] < -100 && race.x[i] > -1095) {
            this.rotation = 90;
        }
        if (this.x > 50) {
            this.rotation = 0;
        }
    }
};

Duck.prototype.raceMove4 = function(i) {

    if (raceOverScreen === false) {

        if (race.x[i] > -100) {
            race.x[i] -= this.runLevel;
        }
        if (race.x[i] <= -100 && race.x[i] > -1095) {
            race.x[i] -= this.swimLevel;
        }
        if (race.x[i] < -100 && race.x[i] > -1095) {
            if (i === 6) {
                this.y = 150 - this.h / 1.32;
            }
            if (i === 7) {
                this.y = 350 - this.h / 1.32;
            }
        }
        if (race.x[i] <= -1095) {
            if (i === 6) {
                this.y = 125 - this.h / 1.32;
            }
            if (i === 7) {
                this.y = 325 - this.h / 1.32;
            }
        }
        if (race.x[i] <= -1095 && race.x[i] > -1800) {
            race.x[i] -= this.runLevel;
        }
        if (race.x[i] <= -1800 && race.x[i] > -2800) {
            race.x[i] -= this.flyLevel;
        }
        if (race.x[i] < -1800 && race.x[i] > -2900) {
            this.rotation = 90;
        }
        if (race.x[i] < -2775) {
            this.rotation = 0;
        }
        if (race.x[i] <= -2800 && this.x < 350) {
            this.x += this.runLevel;
        }
    }
};

var race1 = function() {

    land(race.x[0], 125, 1000, 75);
    land(race.x[1], 325, 1000, 75);

    duckAI.raceDrawAI();
    duckAI.raceMove1(0);
    duck.raceDrawPlayer();
    duck.raceMove1(1);

    flag(race.x[0] + 835, 50);
    flag(race.x[1] + 835, 250);

    winningAndLosingRace(0, 8.22);
};

var race2 = function() {

    land(race.x[2], 125, 200, 75);
    land(race.x[2] + 1200, 125, 300, 75);
    water(race.x[2] + 200, 125, 1000, 75);

    land(race.x[3], 325, 200, 75);
    land(race.x[3] + 1200, 325, 300, 75);
    water(race.x[3] + 200, 325, 1000, 75);

    duckAI.raceDrawAI();
    duckAI.raceMove2(2);
    duck.raceDrawPlayer();
    duck.raceMove2(3);

    flag(race.x[2] + 1390, 50);
    flag(race.x[3] + 1390, 250);

    winningAndLosingRace(2, 13.8);
};

var race3 = function() {

    land(race.x[4], 125, 200, 75);
    land(race.x[4] + 1150, 125, 350, 75);
    sky(race.x[4] + 200, 125, 950, 75);

    land(race.x[5], 325, 200, 75);
    land(race.x[5] + 1150, 325, 350, 75);
    sky(race.x[5] + 200, 325, 950, 75);

    duckAI.raceDrawAI();
    duckAI.raceMove3(4);
    duck.raceDrawPlayer();
    duck.raceMove3(5);

    flag(race.x[4] + 1400, 50);
    flag(race.x[5] + 1400, 250);

    winningAndLosingRace(4, 13.8);
};

var race4 = function() {

    land(race.x[6], 125, 200, 75);
    land(race.x[6] + 1200, 125, 700, 75);
    water(race.x[6] + 200, 125, 1000, 75);

    sky(race.x[6] + 1900, 125, 1000, 75);
    land(race.x[6] + 2900, 125, 300, 75);

    land(race.x[7], 325, 200, 75);
    land(race.x[7] + 1200, 325, 700, 75);
    water(race.x[7] + 200, 325, 1000, 75);

    sky(race.x[7] + 1900, 325, 1000, 75);
    land(race.x[7] + 2900, 325, 325, 75);

    duckAI.raceDrawAI();
    duckAI.raceMove4(6);
    duck.raceDrawPlayer();
    duck.raceMove4(7);

    flag(race.x[6] + 3035, 50);
    flag(race.x[7] + 3035, 250);

    winningAndLosingRace(6, 30.29);
};

var raceScreen = function() {

    background(skyBlue);

    switch (race.raceNumber) {
        case 1:
            race1();
            break;
        case 2:
            race2();
            break;
        case 3:
            race3();
            break;
        case 4:
            race4();
            break;
        case 5:
            race1();
            break;
        case 6:
            race2();
            break;
        case 7:
            race3();
            break;
        case 8:
            race4();
            break;
    }

    stroke(0);
    strokeWeight(5);
    line(0, 200, 400, 200);

    energySystem();

    if (raceOverScreen === true) {
        raceOver();
    }
};

var drawConfetti = function() {

    if (confetti.x.length < 25) {
        confetti.x.push(random(0, 392));
    }
    if (confetti.y.length < 25) {
        confetti.y.push(random(-400, -25));
    }
    if (confetti.s.length < 25) {
        confetti.s.push(random(1, 3));
    }
    if (confetti.c.length < 25) {
        confetti.c.push(round(random(0, 9)));
    }

    for (var i = 0; i < confetti.x.length; i++) {

        noStroke();
        fill(colors[confetti.c[i]]);
        rect(confetti.x[i], confetti.y[i], 8, 25);

        confetti.y[i] += confetti.s[i];

        if (confetti.y[i] > 425) {
            confetti.y[i] = random(-400, -25);
        }
    }
};

var beatGame = function() {

    background(skyBlue);

    fill(0);
    textSize(50);
    text("Congratulations!", 200, 50);
    textSize(25);
    text("Now that you beat Pablo,\nyou are the champion!", 200, 125);
    drawDucks(200, 300, duck);

    drawConfetti();
};

// Rest of Screen Functions

var profileScreen = function() {

    background(skyBlue);
    fill(0);
    textSize(35);
    text("Profile", 200, 50);
    noFill();
    stroke(0);
    for (var y = 90; y < 300; y += 35) {
        fill(255);
        rect(50, y, 150, 35);
        fill(225);
        rect(200, y, 150, 35);
    }

    textSize(20);
    fill(0);
    text("Name:", 125, 105);
    text("Gender:", 125, 140);
    text("Weight:", 125, 175);
    text("Height:", 125, 210);
    text("Win %", 125, 245);
    text("Overall Skill", 125, 280);

    text(duck.name.join(""), 275, 105);
    if (duck.gender === 0) {
        text("Male", 275, 140);
    }
    if (duck.gender === 1) {
        text("Female", 275, 140);
    }
    text(round(duck.w) + " lbs", 275, 175);
    text(round(duck.h) + " in", 275, 210);
    if (race.totalGames === 0) {
        text("N/A", 275, 245);
    } else {
        text(round((race.numberOfWins / race.totalGames) * 100) + "%", 275, 245);
    }
    text(duck.runLevel + duck.swimLevel + duck.energyLevel + duck.flyLevel, 275, 280);
    textSize(28);
    buttons[19].draw();
};

var upgradesScreen = function() {

    background(skyBlue);
    fill(0);
    textSize(35);
    text("Upgrades", 200, 50);
    textSize(14);
    text("Coins: " + round(gameCoins), 200, 85);

    upgradeTable();

    textSize(28);
    buttons[20].draw();
};

var shopScreen = function() {

    background(skyBlue);
    fill(0);
    textSize(35);
    text("Shop", 200, 50);
    textSize(14);
    text("Coins: " + round(gameCoins), 200, 85);
    shopTable();
    textSize(28);
    buttons[21].draw();
};

var finalGame = function() {

    textFont(createFont("sans-serif"));
    textAlign(CENTER, CENTER);
    cursor(ARROW);

    switch (screen) {
        case "mainMenuScreen":
            mainMenuScreen();
            break;
        case "settingsScreen":
            settingsScreen();
            break;
        case "upgradeYourDuckScreen":
            upgradeYourDuckScreen();
            break;
        case "personalizeYourDuckScreen":
            personalizeYourDuckScreen();
            break;
        case "homeScreen":
            homeScreen();
            break;
        case "practiceSelectScreen":
            practiceSelectScreen();
            break;
        case "game1":
            game1();
            break;
        case "game2":
            game2();
            break;
        case "game3":
            game3();
            break;
        case "game4":
            game4();
            break;
        case "game5":
            game5();
            break;
        case "game6":
            game6();
            break;
        case "playgroundScreen":
            playgroundScreen();
            break;
        case "raceSelectScreen":
            raceSelectScreen();
            break;
        case "raceScreen":
            raceScreen();
            break;
        case "profileScreen":
            profileScreen();
            break;
        case "upgradesScreen":
            upgradesScreen();
            break;
        case "shopScreen":
            shopScreen();
            break;
    }

    if (showErrorMessage[0] === true) {
        errorMessage("Sorry, you\n don't have enough\ncoins.");
    }
    if (showErrorMessage[1] === true) {
        errorMessage("Sorry, you\n don't own this\nitem.");
    }
    if (showErrorMessage[2] === true) {
        errorMessage("Sorry, you\n haven't unlocked\nthis race yet.");
    }
    if (showErrorMessage[3] === true) {
        errorMessage("Sorry, you\n have already beaten\nthis race.");
    }

    if (race.beaten[7] === true) {
        beatGame();
        items.wear[1] = false;
        items.wear[3] = false;
    }
};

// User Input Functions

mouseClicked = function() {

    switch (screen) {
        case "mainMenuScreen":
            buttons[0].clicked();
            break;
        case "settingsScreen":
            buttons[24].clicked();
            break;
        case "upgradeYourDuckScreen":
            buttons[1].clicked();
            break;
        case "personalizeYourDuckScreen":
            buttons[2].clicked();
            break;
        case "homeScreen":
            buttons[3].clicked();
            buttons[4].clicked();
            buttons[5].clicked();
            buttons[6].clicked();
            buttons[7].clicked();
            buttons[22].clicked();
            break;
        case "practiceSelectScreen":
            buttons[8].clicked();
            buttons[9].clicked();
            buttons[10].clicked();
            buttons[11].clicked();
            buttons[12].clicked();
            buttons[13].clicked();
            buttons[14].clicked();
            break;
        case "game1":
        case "game2":
        case "game3":
        case "game4":
        case "game5":
        case "game6":
            if (gameOverScreen === true) {
                buttons[15].clicked();
            }
            break;
        case "raceSelectScreen":
            buttons[16].clicked();

            if (race.unlockRace[race.raceNumber - 1] === true && race.beaten[race.raceNumber - 1] === false) {
                buttons[17].clicked();
            }
            break;
        case "playgroundScreen":
            buttons[23].clicked();
            break;
        case "raceScreen":
            if (raceOverScreen === true) {
                buttons[18].clicked();
            }
            break;
        case "profileScreen":
            buttons[19].clicked();
            break;
        case "upgradesScreen":
            buttons[20].clicked();
            break;
        case "shopScreen":
            buttons[21].clicked();
            break;
    }

    if (screen === "settingsScreen") {
        if (mouseY > 140 && mouseY < 160) {
            if (mouseX > 215 && mouseX < 230) {
                difficulty--;
            }
            if (mouseX > 300 && mouseX < 315) {
                difficulty++;
            }
            if (difficulty < 0) {
                difficulty = 2;
            }
            if (difficulty > 2) {
                difficulty = 0;
            }
        }
    }

    if (screen === "upgradeYourDuckScreen" || screen === "upgradesScreen") {
        if (mouseX > 275 && mouseX < 350) {
            if (mouseY > 160 && mouseY < 195) {
                if (gameCoins >= cost.run) {
                    if (duck.runLevel < 15) {
                        gameCoins -= cost.run;
                        duck.runLevel++;
                    }
                } else {
                    showErrorMessage[0] = true;
                }
            }
            if (mouseY > 195 && mouseY < 230) {
                if (gameCoins >= cost.swim) {
                    if (duck.swimLevel < 15) {
                        gameCoins -= cost.swim;
                        duck.swimLevel++;
                    }
                } else {
                    showErrorMessage[0] = true;
                }
            }
            if (mouseY > 230 && mouseY < 265) {
                if (gameCoins >= cost.fly) {
                    if (duck.flyLevel < 15) {
                        gameCoins -= cost.fly;
                        duck.flyLevel++;
                    }
                } else {
                    showErrorMessage[0] = true;
                }
            }
            if (mouseY > 265 && mouseY < 300) {
                if (gameCoins >= cost.energy) {
                    if (duck.energyLevel < 15) {
                        gameCoins -= cost.energy;
                        duck.energyLevel++;
                    }
                } else {
                    showErrorMessage[0] = true;
                }
            }
        }
    }

    if (screen === "personalizeYourDuckScreen") {
        if (mouseY > 140 && mouseY < 160) {
            if (mouseX > 150 && mouseX < 165) {
                duck.gender--;
            }
            if (mouseX > 235 && mouseX < 250) {
                duck.gender++;
            }
            if (duck.gender < 0) {
                duck.gender = 1;
            }
            if (duck.gender > 1) {
                duck.gender = 0;
            }
        }
        if (mouseY > 290 && mouseY < 310) {
            if (mouseX > 150 && mouseX < 165) {
                duck.hairStyle--;
            }
            if (mouseX > 235 && mouseX < 250) {
                duck.hairStyle++;
            }
            if (duck.hairStyle < 1) {
                duck.hairStyle = 8;
            }
            if (duck.hairStyle > 8) {
                duck.hairStyle = 1;
            }
        }
    }

    if (screen === "shopScreen") {
        if (mouseX > 200 && mouseX < 275) {
            if (mouseY > 160 && mouseY < 195) {
                if (gameCoins >= items.cost[0]) {
                    if (items.buy[0] === false) {
                        gameCoins -= items.cost[0];
                        items.buy[0] = true;
                        items.wear[0] = true;
                    }
                } else {
                    showErrorMessage[0] = true;
                }
            }
            if (mouseY > 195 && mouseY < 230) {
                if (gameCoins >= items.cost[1]) {
                    if (items.buy[1] === false) {
                        gameCoins -= items.cost[1];
                        items.buy[1] = true;
                        items.wear[1] = true;
                    }
                } else {
                    showErrorMessage[0] = true;
                }
            }
            if (mouseY > 230 && mouseY < 265) {
                if (gameCoins >= items.cost[2]) {
                    if (items.buy[2] === false) {
                        gameCoins -= items.cost[2];
                        items.buy[2] = true;
                        items.wear[2] = true;
                    }
                } else {
                    showErrorMessage[0] = true;
                }
            }
            if (mouseY > 265 && mouseY < 300) {
                if (gameCoins >= items.cost[3]) {
                    if (items.buy[3] === false) {
                        gameCoins -= items.cost[3];
                        items.buy[3] = true;
                        items.wear[3] = true;
                    }
                } else {
                    showErrorMessage[0] = true;
                }
            }
        }
        if (mouseX > 275 && mouseX < 350) {
            if (mouseY > 160 && mouseY < 195) {
                if (items.buy[0] === true) {
                    if (items.wear[0] === true) {
                        items.wear[0] = false;
                    } else {
                        items.wear[0] = true;
                    }
                } else {
                    showErrorMessage[1] = true;
                }
            }
            if (mouseY > 195 && mouseY < 230) {
                if (items.buy[1] === true) {
                    if (items.wear[1] === true) {
                        items.wear[1] = false;
                    } else {
                        items.wear[1] = true;
                    }
                } else {
                    showErrorMessage[1] = true;
                }
            }
            if (mouseY > 230 && mouseY < 265) {
                if (items.buy[2] === true) {
                    if (items.wear[2] === true) {
                        items.wear[2] = false;
                    } else {
                        items.wear[2] = true;
                    }
                } else {
                    showErrorMessage[1] = true;
                }
            }
            if (mouseY > 265 && mouseY < 300) {
                if (items.buy[3] === true) {
                    if (items.wear[3] === true) {
                        items.wear[3] = false;
                    } else {
                        items.wear[3] = true;
                    }
                } else {
                    showErrorMessage[1] = true;
                }
            }
        }
    }

    if (buttons[15].over()) {
        gameOverScreen = false;
    }

    if (buttons[18].over() && raceOverScreen === true) {
        if (race.outcome === "Won") {
            gameCoins += race.reward;
            race.numberOfWins++;
            race.beaten[race.raceNumber - 1] = true;
            race.unlockRace[race.raceNumber] = true;
        }

        for (var i = 0; i < race.x.length; i++) {
            race.x[i] = 0;
        }

        race.totalGames++;
        race.outcome = "undefined";
        raceOverScreen = false;
    }

    if (screen === "game3" && g3.snowball.x.length < 3 && g3.start === true) {
        g3.snowball.x.push(g3.cannon.x);
        g3.snowball.y.push(335);
        g3.snowball.speed.push(5);
    }

    if (screen === "game6") {
        if (gameOverScreen === false) {
            duck.g6v.click = true;
        }
    }
};

keyPressed = function() {
    keys[keyCode] = true;
};

keyReleased = function() {
    keys[keyCode] = false;

    if (gameOverScreen === false && g2.start === true) {
        if (keyCode === LEFT && duck.x > 120) {
            duck.x -= 80;
            duck.g2v.faceLeft = true;
        }
        if (keyCode === RIGHT && duck.x < 250) {
            duck.x += 80;
            duck.g2v.faceLeft = false;
        }
        if (keyCode === UP) {
            duck.g2v.jumped = true;
        }
    }
};

keyTyped = function() {

    if (key.code !== BACKSPACE && duck.name.length < 12) {
        duck.name.push(key);
    }

    if (key.code === BACKSPACE) {
        duck.name.pop();
    }
};

draw = function() {

    finalGame();
};
