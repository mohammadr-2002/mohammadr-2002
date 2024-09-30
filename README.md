// Global Game State
const gameState = {
    gold: 1000,
    elixir: 1000,
    structures: [],
    troops: [],
    enemies: [],
};

// Canvas Setup
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');
canvas.width = 800;
canvas.height = 500;

// Load images
const townHallImg = new Image();
townHallImg.src = 'images/townhall.png'; // Use actual paths for images
const barracksImg = new Image();
barracksImg.src = 'images/barracks.png';
const soldierImg = new Image();
soldierImg.src = 'images/soldier.png';
const enemyImg = new Image();
enemyImg.src = 'images/enemy.png';

// Class for Base Structures
class Structure {
    constructor(name, x, y, hp, image) {
        this.name = name;
        this.x = x;
        this.y = y;
        this.hp = hp;
        this.image = image;
    }

    draw() {
        ctx.drawImage(this.image, this.x, this.y, 100, 100);
    }
}

// Class for Troops
class Troop {
    constructor(name, x, y, attack, hp, speed, image) {
        this.name = name;
        this.x = x;
        this.y = y;
        this.attack = attack;
        this.hp = hp;
        this.speed = speed;
        this.image = image;
    }

    move() {
        // Simple AI to move troop towards enemies or defend base
        this.x += this.speed;
    }

    draw() {
        ctx.drawImage(this.image, this.x, this.y, 50, 50);
    }
}

// Class for Enemy
class Enemy {
    constructor(name, x, y, attack, hp, speed, image) {
        this.name = name;
        this.x = x;
        this.y = y;
        this.attack = attack;
        this.hp = hp;
        this.speed = speed;
        this.image = image;
    }

    move() {
        this.x -= this.speed; // Move left to attack player's base
    }

    draw() {
        ctx.drawImage(this.image, this.x, this.y, 50, 50);
    }
}

// Function to build structures
function buildStructure(type) {
    if (type === 'townHall' && gameState.gold >= 500) {
        const townHall = new Structure('Town Hall', 100, 100, 1000, townHallImg);
        gameState.structures.push(townHall);
        gameState.gold -= 500;
    } else if (type === 'barracks' && gameState.gold >= 300) {
        const barracks = new Structure('Barracks', 200, 100, 500, barracksImg);
        gameState.structures.push(barracks);
        gameState.gold -= 300;
    }
    updateResources();
    drawGame();
}

// Function to train troops
function trainTroop() {
    if (gameState.elixir >= 200) {
        const soldier = new Troop('Soldier', 300, 300, 50, 100, 2, soldierImg);
        gameState.troops.push(soldier);
        gameState.elixir -= 200;
    }
    updateResources();
    drawGame();
}

// Function to handle enemy attacks
function spawnEnemy() {
    const enemy = new Enemy('Enemy', 750, 300, 50, 100, 1, enemyImg);
    gameState.enemies.push(enemy);
}

// Update and Render Game
function drawGame() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Draw Structures
    gameState.structures.forEach(structure => {
        structure.draw();
    });

    // Draw Troops and Move Them
    gameState.troops.forEach(troop => {
        troop.move();
        troop.draw();
    });

    // Draw Enemies and Move Them
    gameState.enemies.forEach(enemy => {
        enemy.move();
        enemy.draw();
    });

    requestAnimationFrame(drawGame);
}

// Update the Resource Count in UI
function updateResources() {
    document.getElementById('gold-count').innerText = gameState.gold;
    document.getElementById('elixir-count').innerText = gameState.elixir;
}

// Game Loop
function gameLoop() {
    drawGame();
}

// Enemy spawn every 15 seconds
setInterval(spawnEnemy, 15000);

// Start the game
gameLoop();
