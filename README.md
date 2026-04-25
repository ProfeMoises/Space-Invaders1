# 🚀 Space Invaders - Juego Web Retro

![Space Invaders](https://img.shields.io/badge/HTML5-Canvas-orange) ![CSS3](https://img.shields.io/badge/CSS3-neon-blue) ![JavaScript](https://img.shields.io/badge/JavaScript-Vanilla-yellow)

Defiende la Tierra de oleadas alienígenas en este juego arcade clásico recreado con HTML, CSS y JavaScript usando Canvas. Inspirado en el legendario **Space Invaders** de 1978, este proyecto incluye efectos visuales modernos, partículas dinámicas y dificultad progresiva.

---

## 📖 Historia

En el año 2247, la humanidad ha colonizado varios sistemas estelares. Una misteriosa raza alienígena, los **Exódicos**, emerge de los confines del espacio con una sola misión: eliminar toda resistencia. Tú eres el piloto de la última nave defensiva, el *Centinela Estelar*. Con cañones láser y escudos limitados, debes repeler la invasión ola tras ola.

Cada nivel representa un nuevo escuadrón de ataque enemigo. Los Exódicos se organizan en formaciones de combate y se vuelven más rápidos y agresivos. ¿Podrás salvar a la humanidad?

---

## 🎮 Cómo jugar

### Controles

| Tecla | Acción |
|-------|--------|
| **Flecha izquierda / A** | Mover la nave a la izquierda |
| **Flecha derecha / D** | Mover la nave a la derecha |
| **Barra espaciadora** | Disparar láser |
| **R** | Reiniciar la partida en cualquier momento |

Al cargar el juego, presiona **ESPACIO** para comenzar la batalla.

### Mecánicas

- **Objetivo**: Destruye todos los alienígenas de cada nivel.
- **Vidas**: Comienzas con 3 vidas. Si recibes un disparo o los aliens alcanzan tu posición, pierdes una vida. Al quedarte sin vidas, el juego termina.
- **Puntuación**: Cada fila de alienígenas otorga diferentes puntos:
  - Fila 1 (rojos): 50 puntos
  - Fila 2 (naranjas): 40 puntos
  - Fila 3 (amarillos): 30 puntos
  - Fila 4 (cyan): 20 puntos
  - Fila 5 (verdes): 10 puntos
- **Niveles**: Al eliminar a todos los aliens, avanzas al siguiente nivel. La dificultad aumenta:
  - Mayor velocidad de movimiento enemigo.
  - Mayor frecuencia de disparos enemigos.
  - La cadencia de movimiento se reduce (los aliens bajan más rápido).
- **Vida extra**: Cada 3 niveles completados recuperas una vida (hasta un máximo de 5).
- **Game Over**: Pantalla final con puntuación. Presiona **ESPACIO** o **R** para reiniciar.
- **Transición entre niveles**: Mensaje de "¡Prepárate!" antes de la siguiente oleada.

---

## ✨ Características visuales

- **Fondo animado** de estrellas con brillo variable (efecto twinkle).
- **Nave del jugador** con cabina brillante, motores duales y gradientes.
- **Alienígenas detallados** con antenas, ojos brillantes y colores por fila.
- **Explosiones de partículas** al destruir enemigos o recibir daño.
- **Balas con efectos de luz** y colores diferenciados (láser amarillo del jugador, rayos rojos enemigos).
- **HUD retro** con sombras neón verde/amarillo y bordes estilizados.

---

## 🧠 Datos interesantes

- El **Space Invaders original** (Taito, 1978) popularizó los videojuegos a nivel mundial y estableció el género de disparos.
- En esta versión, los aliens de las filas inferiores (más cercanos a ti) tienen mayor probabilidad de disparar, simulando un comportamiento táctico.
- La dificultad escala no solo con la velocidad, sino también con la frecuencia de disparos y la reducción del intervalo de movimiento enemigo.
- El código está escrito completamente en **vanilla JavaScript**, sin librerías externas, aprovechando al máximo el elemento `<canvas>` para un renderizado a 60 fps.
- La estética mezcla el aspecto CRT clásico con toques de neón y partículas modernas.

---

## 🛠️ Cómo ejecutar

1. Descarga el archivo `index.html` (o clónalo desde el repositorio).
2. Ábrelo directamente con cualquier navegador moderno (Chrome, Firefox, Edge, Safari).
3. No requiere instalación ni servidor local. ¡Simplemente funciona!

---

## 📂 Código fuente

A continuación se incluye el código completo del juego. Puedes guardarlo como `index.html` y ejecutarlo directamente.

<details>
<summary>Ver código completo (HTML + CSS + JavaScript)</summary>

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Space Invaders</title>
    <style>
        :root { --bg: #0a0a0a; }
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            background: #050510; display: flex; justify-content: center; align-items: center;
            height: 100vh; font-family: 'Courier New', 'Consolas', monospace; overflow: hidden;
            user-select: none; -webkit-user-select: none;
        }
        .stars { position: fixed; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 0; }
        .game-wrapper { position: relative; z-index: 1; display: flex; flex-direction: column; align-items: center; gap: 10px; }
        .game-header {
            display: flex; justify-content: space-between; align-items: center; width: 600px;
            padding: 10px 20px; background: rgba(10,10,30,0.8); border: 2px solid #00ff88;
            border-radius: 10px; box-shadow: 0 0 20px rgba(0,255,136,0.3), inset 0 0 20px rgba(0,255,136,0.05);
        }
        .game-title { font-size: 1.4rem; font-weight: bold; color: #00ff88; letter-spacing: 4px; text-shadow: 0 0 10px #00ff88, 0 0 30px #00ff88; text-transform: uppercase; }
        .score-display, .lives-display, .level-display { color: #ffffff; font-size: 1rem; letter-spacing: 2px; text-shadow: 0 0 8px rgba(255,255,255,0.6); }
        .score-display span, .lives-display span, .level-display span { color: #ffcc00; font-weight: bold; text-shadow: 0 0 10px #ffcc00; }
        canvas { border: 2px solid #00ff88; border-radius: 6px; box-shadow: 0 0 15px rgba(0,255,136,0.4), 0 0 40px rgba(0,255,136,0.15), 0 0 80px rgba(0,255,136,0.05); display: block; background: #020210; }
        .controls-info { display: flex; gap: 25px; color: #aaccff; font-size: 0.85rem; letter-spacing: 1px; background: rgba(10,10,30,0.7); padding: 8px 18px; border-radius: 20px; border: 1px solid rgba(100,150,255,0.3); }
        .controls-info span { color: #ffcc00; font-weight: bold; }
    </style>
</head>
<body>
    <canvas class="stars" id="starsCanvas"></canvas>
    <div class="game-wrapper">
        <div class="game-header">
            <div class="game-title">🚀 Space Invaders</div>
            <div class="score-display">PUNTOS: <span id="scoreValue">0</span></div>
            <div class="level-display">NIVEL: <span id="levelValue">1</span></div>
            <div class="lives-display">❤️ <span id="livesValue">3</span></div>
        </div>
        <canvas id="gameCanvas" width="600" height="550"></canvas>
        <div class="controls-info">
            <span>← →</span> Mover &nbsp;|&nbsp; <span>ESPACIO</span> Disparar &nbsp;|&nbsp; <span>R</span> Reiniciar
        </div>
    </div>
    <script>
        (function() {
            const canvas = document.getElementById('gameCanvas');
            const ctx = canvas.getContext('2d');
            const W = canvas.width, H = canvas.height;
            const scoreValueEl = document.getElementById('scoreValue');
            const livesValueEl = document.getElementById('livesValue');
            const levelValueEl = document.getElementById('levelValue');

            const starsCanvas = document.getElementById('starsCanvas');
            const starsCtx = starsCanvas.getContext('2d');
            let starParticles = [];
            function resizeStars() { starsCanvas.width = window.innerWidth; starsCanvas.height = window.innerHeight; }
            resizeStars(); window.addEventListener('resize', resizeStars);
            function createStars() {
                starParticles = [];
                const count = Math.floor((starsCanvas.width * starsCanvas.height) / 2000);
                for (let i = 0; i < count; i++) {
                    starParticles.push({
                        x: Math.random() * starsCanvas.width,
                        y: Math.random() * starsCanvas.height,
                        r: Math.random() * 1.8 + 0.3,
                        speed: Math.random() * 0.4 + 0.1,
                        opacity: Math.random(),
                        twinkleSpeed: Math.random() * 0.02 + 0.005,
                        twinkleOffset: Math.random() * Math.PI * 2,
                    });
                }
            }
            createStars(); window.addEventListener('resize', createStars);
            function drawStars(time) {
                starsCtx.clearRect(0, 0, starsCanvas.width, starsCanvas.height);
                for (const s of starParticles) {
                    s.y += s.speed;
                    if (s.y > starsCanvas.height + 5) { s.y = -5; s.x = Math.random() * starsCanvas.width; }
                    const opacity = 0.25 + 0.55 * Math.abs(Math.sin(time * s.twinkleSpeed + s.twinkleOffset));
                    starsCtx.beginPath();
                    starsCtx.arc(s.x, s.y, s.r, 0, Math.PI*2);
                    starsCtx.fillStyle = `rgba(180,210,255,${opacity})`;
                    starsCtx.fill();
                    if (s.r > 1.2 && opacity > 0.65) {
                        starsCtx.beginPath();
                        starsCtx.arc(s.x, s.y, s.r*2.5, 0, Math.PI*2);
                        starsCtx.fillStyle = `rgba(180,210,255,${opacity*0.15})`;
                        starsCtx.fill();
                    }
                }
            }

            const STATE = { IDLE:'idle', PLAYING:'playing', PAUSED:'paused', GAMEOVER:'gameover', LEVELTRANSITION:'levelTransition' };
            let gameState = STATE.IDLE;
            let score = 0, lives = 3, level = 1, transitionTimer = 0;
            const TRANSITION_DURATION = 120;

            const player = { x: W/2, y: H-55, width:40, height:30, speed:5, cooldown:0, cooldownMax:25 };
            let playerBullets = [], enemyBullets = [];
            const BULLET_SPEED = 8, ENEMY_BULLET_SPEED = 4.5;

            let aliens = [], alienDirection = 1, alienSpeed = 1.2, alienMoveTimer = 0, alienMoveInterval = 40;
            let alienShootTimer = 0, alienShootInterval = 55;
            const ALIEN_ROWS = 5, ALIEN_COLS = 10, ALIEN_WIDTH = 34, ALIEN_HEIGHT = 26;
            const ALIEN_PADDING_X = 12, ALIEN_PADDING_Y = 10, ALIEN_START_X = 55, ALIEN_START_Y = 55;
            const ALIEN_SCORES = [50,40,30,20,10];
            const ALIEN_COLORS = ['#ff3366','#ff9933','#ffcc00','#33ccff','#66ff66'];

            let particles = [];

            const keys = {};
            window.addEventListener('keydown', (e) => {
                keys[e.key] = true; keys[e.code] = true;
                if (e.key === ' ' || e.code === 'Space') {
                    e.preventDefault();
                    if (gameState === STATE.IDLE) startGame();
                    else if (gameState === STATE.GAMEOVER) { resetGame(); startGame(); }
                    else if (gameState === STATE.LEVELTRANSITION && transitionTimer <= 0) { transitionTimer=0; gameState=STATE.PLAYING; spawnAliens(); }
                }
                if (e.key === 'r' || e.key === 'R') { resetGame(); startGame(); }
            });
            window.addEventListener('keyup', (e) => { keys[e.key]=false; keys[e.code]=false; });

            function spawnAliens() {
                aliens = [];
                const baseSpeed = 1.0 + (level-1)*0.4;
                alienSpeed = Math.min(baseSpeed, 4.5);
                alienMoveInterval = Math.max(12, 40 - (level-1)*4);
                alienShootInterval = Math.max(20, 55 - (level-1)*6);
                alienDirection = 1;
                for (let row=0; row<ALIEN_ROWS; row++) {
                    for (let col=0; col<ALIEN_COLS; col++) {
                        aliens.push({ x: ALIEN_START_X + col*(ALIEN_WIDTH+ALIEN_PADDING_X), y: ALIEN_START_Y + row*(ALIEN_HEIGHT+ALIEN_PADDING_Y), width: ALIEN_WIDTH, height: ALIEN_HEIGHT, row: row, alive: true, animFrame:0, animTimer:0 });
                    }
                }
            }
            function getLeftmostAlien() { let minX=Infinity; for(const a of aliens) if(a.alive && a.x<minX) minX=a.x; return minX===Infinity?0:minX; }
            function getRightmostAlien() { let maxX=-Infinity; for(const a of aliens) if(a.alive && a.x+a.width>maxX) maxX=a.x+a.width; return maxX===-Infinity?W:maxX; }
            function getLowestAlienY() { let maxY=-Infinity; for(const a of aliens) if(a.alive && a.y+a.height>maxY) maxY=a.y+a.height; return maxY===-Infinity?0:maxY; }
            function countAliveAliens() { return aliens.filter(a=>a.alive).length; }

            function alienShoot() {
                const aliveAliens = aliens.filter(a=>a.alive);
                if (aliveAliens.length===0) return;
                const colsWithAliens = {};
                for (const a of aliveAliens) { if(!colsWithAliens[a.x] || a.y>colsWithAliens[a.x].y) colsWithAliens[a.x]=a; }
                const candidates = Object.values(colsWithAliens);
                if (candidates.length>0) {
                    const shooter = candidates[Math.floor(Math.random()*candidates.length)];
                    enemyBullets.push({ x: shooter.x+shooter.width/2, y: shooter.y+shooter.height, speed: ENEMY_BULLET_SPEED+(level-1)*0.7, width:4, height:12 });
                }
            }

            function spawnParticles(x,y,color,count=15) {
                for(let i=0; i<count; i++) {
                    const angle = Math.random()*Math.PI*2, speed = Math.random()*4+1.5;
                    particles.push({ x,y, vx:Math.cos(angle)*speed, vy:Math.sin(angle)*speed, life:1, decay:Math.random()*0.03+0.02, color, size:Math.random()*3.5+1.5 });
                }
            }
            function updateParticles() {
                for(let i=particles.length-1; i>=0; i--) {
                    const p = particles[i]; p.x+=p.vx; p.y+=p.vy; p.life-=p.decay;
                    if(p.life<=0) particles.splice(i,1);
                }
            }
            function drawParticles(ctx) {
                for(const p of particles) {
                    const alpha = p.life;
                    ctx.beginPath(); ctx.arc(p.x,p.y,p.size,0,Math.PI*2);
                    ctx.fillStyle = p.color.replace('1)',`${alpha})`).replace('rgb','rgba');
                    if(p.color.startsWith('#')) ctx.fillStyle = p.color + Math.floor(alpha*255).toString(16).padStart(2,'0');
                    ctx.globalAlpha = alpha; ctx.fill();
                    ctx.beginPath(); ctx.arc(p.x,p.y,p.size*2,0,Math.PI*2);
                    ctx.fillStyle = p.color.replace('1)',`${alpha*0.3})`).replace('rgb','rgba');
                    if(p.color.startsWith('#')) ctx.fillStyle = p.color + Math.floor(alpha*80).toString(16).padStart(2,'0');
                    ctx.fill(); ctx.globalAlpha=1;
                }
            }

            function drawPlayer() {
                const {x,y,width,height} = player;
                ctx.save(); ctx.translate(x,y);
                const engineGlow = ctx.createRadialGradient(0,height/2+6,2,0,height/2+6,20);
                engineGlow.addColorStop(0,'rgba(100,180,255,0.9)'); engineGlow.addColorStop(0.5,'rgba(60,140,255,0.4)'); engineGlow.addColorStop(1,'rgba(0,80,255,0)');
                ctx.fillStyle=engineGlow; ctx.beginPath(); ctx.arc(0,height/2+6,20,0,Math.PI*2); ctx.fill();
                ctx.fillStyle='#4488ff'; ctx.beginPath(); ctx.moveTo(-width/3,height/2); ctx.lineTo(-width/4,height/2+12); ctx.lineTo(-width/6,height/2); ctx.fill();
                ctx.fillStyle='#aaddff'; ctx.beginPath(); ctx.moveTo(-width/3.5,height/2); ctx.lineTo(-width/4,height/2+8); ctx.lineTo(-width/5.5,height/2); ctx.fill();
                ctx.fillStyle='#4488ff'; ctx.beginPath(); ctx.moveTo(width/3,height/2); ctx.lineTo(width/4,height/2+12); ctx.lineTo(width/6,height/2); ctx.fill();
                ctx.fillStyle='#aaddff'; ctx.beginPath(); ctx.moveTo(width/3.5,height/2); ctx.lineTo(width/4,height/2+8); ctx.lineTo(width/5.5,height/2); ctx.fill();
                const bodyGrad = ctx.createLinearGradient(0,-height/2,0,height/2); bodyGrad.addColorStop(0,'#e0e8ff'); bodyGrad.addColorStop(0.5,'#8899cc'); bodyGrad.addColorStop(1,'#556688');
                ctx.fillStyle=bodyGrad; ctx.beginPath(); ctx.moveTo(0,-height/2); ctx.lineTo(-width/2,height/2); ctx.lineTo(-width/3,height/3); ctx.lineTo(width/3,height/3); ctx.lineTo(width/2,height/2); ctx.closePath(); ctx.fill();
                ctx.strokeStyle='#aaccff'; ctx.lineWidth=1.5; ctx.stroke();
                const cabinGrad = ctx.createRadialGradient(0,-4,2,0,-4,10); cabinGrad.addColorStop(0,'#ffffff'); cabinGrad.addColorStop(0.6,'#88ccff'); cabinGrad.addColorStop(1,'#3366aa');
                ctx.fillStyle=cabinGrad; ctx.beginPath(); ctx.ellipse(0,-4,9,11,0,0,Math.PI*2); ctx.fill(); ctx.strokeStyle='#ddeeff'; ctx.lineWidth=1; ctx.stroke();
                ctx.fillStyle='rgba(255,255,255,0.5)'; ctx.beginPath(); ctx.arc(-3,-7,3.5,0,Math.PI*2); ctx.fill();
                ctx.restore();
            }

            function drawAlien(alien) {
                const {x,y,width,height,row,animFrame} = alien;
                const color = ALIEN_COLORS[row];
                ctx.save(); ctx.translate(x+width/2, y+height/2);
                const glow = ctx.createRadialGradient(0,0,width*0.3,0,0,width*0.8); glow.addColorStop(0,'rgba(255,255,255,0.15)'); glow.addColorStop(1,'rgba(0,0,0,0)');
                ctx.fillStyle=glow; ctx.beginPath(); ctx.arc(0,0,width*0.8,0,Math.PI*2); ctx.fill();
                const bodyGrad = ctx.createLinearGradient(0,-height/2,0,height/2); bodyGrad.addColorStop(0,color); bodyGrad.addColorStop(1,'#1a1a2e');
                ctx.fillStyle=bodyGrad; ctx.beginPath();
                const bodyTop=-height/2+3, bodyBottom=height/2-2;
                ctx.moveTo(0,bodyTop); ctx.quadraticCurveTo(-width/2-2,bodyTop+4,-width/2+2,bodyTop+10); ctx.lineTo(-width/2+5,bodyBottom); ctx.lineTo(-width/3,bodyBottom+3); ctx.lineTo(-width/6,bodyBottom); ctx.lineTo(width/6,bodyBottom); ctx.lineTo(width/3,bodyBottom+3); ctx.lineTo(width/2-5,bodyBottom); ctx.lineTo(width/2-2,bodyTop+10); ctx.quadraticCurveTo(width/2+2,bodyTop+4,0,bodyTop);
                ctx.fill(); ctx.strokeStyle='rgba(255,255,255,0.3)'; ctx.lineWidth=0.8; ctx.stroke();
                const eyeY=-4, eyeSpread=7, eyeBob=Math.sin(animFrame*0.4)*1.2;
                ctx.fillStyle='#111122'; ctx.beginPath(); ctx.ellipse(-eyeSpread,eyeY+eyeBob,5,6,0,0,Math.PI*2); ctx.fill();
                ctx.fillStyle='#ffffff'; ctx.beginPath(); ctx.arc(-eyeSpread+1,eyeY+eyeBob-1,2.2,0,Math.PI*2); ctx.fill();
                ctx.fillStyle='#ff0044'; ctx.beginPath(); ctx.arc(-eyeSpread+1,eyeY+eyeBob-1,1.2,0,Math.PI*2); ctx.fill();
                ctx.fillStyle='#111122'; ctx.beginPath(); ctx.ellipse(eyeSpread,eyeY+eyeBob,5,6,0,0,Math.PI*2); ctx.fill();
                ctx.fillStyle='#ffffff'; ctx.beginPath(); ctx.arc(eyeSpread+1,eyeY+eyeBob-1,2.2,0,Math.PI*2); ctx.fill();
                ctx.fillStyle='#ff0044'; ctx.beginPath(); ctx.arc(eyeSpread+1,eyeY+eyeBob-1,1.2,0,Math.PI*2); ctx.fill();
                ctx.strokeStyle=color; ctx.lineWidth=1.5; ctx.beginPath(); ctx.moveTo(-6,bodyTop); ctx.quadraticCurveTo(-10,bodyTop-12,-7,bodyTop-18); ctx.stroke();
                ctx.beginPath(); ctx.moveTo(6,bodyTop); ctx.quadraticCurveTo(10,bodyTop-12,7,bodyTop-18); ctx.stroke();
                ctx.fillStyle='#ffffcc'; ctx.beginPath(); ctx.arc(-7,bodyTop-18,2.5,0,Math.PI*2); ctx.fill();
                ctx.beginPath(); ctx.arc(7,bodyTop-18,2.5,0,Math.PI*2); ctx.fill();
                ctx.restore();
            }

            function drawBullet(bullet,isPlayer) {
                if(isPlayer) {
                    const grad = ctx.createLinearGradient(bullet.x,bullet.y-6,bullet.x,bullet.y+6); grad.addColorStop(0,'#ffffff'); grad.addColorStop(0.3,'#ffdd44'); grad.addColorStop(1,'#ff8800');
                    ctx.fillStyle=grad; ctx.fillRect(bullet.x-2,bullet.y-7,4,14);
                    ctx.fillStyle='rgba(255,255,200,0.7)'; ctx.fillRect(bullet.x-1,bullet.y-7,2,14);
                    ctx.shadowColor='#ffcc00'; ctx.shadowBlur=8; ctx.fillStyle='#ffffff'; ctx.fillRect(bullet.x-1,bullet.y-7,2,4); ctx.shadowBlur=0;
                } else {
                    const grad = ctx.createLinearGradient(bullet.x,bullet.y-5,bullet.x,bullet.y+5); grad.addColorStop(0,'#ff4444'); grad.addColorStop(0.5,'#ff0000'); grad.addColorStop(1,'#880000');
                    ctx.fillStyle=grad; ctx.fillRect(bullet.x-2,bullet.y-5,4,10);
                    ctx.fillStyle='rgba(255,100,100,0.8)'; ctx.fillRect(bullet.x-1,bullet.y-5,2,10);
                }
            }

            function drawHUD() { ctx.strokeStyle='rgba(0,255,136,0.4)'; ctx.lineWidth=1; ctx.setLineDash([8,15]); ctx.beginPath(); ctx.moveTo(0,H-20); ctx.lineTo(W,H-20); ctx.stroke(); ctx.setLineDash([]); }

            function startGame() { if(gameState===STATE.PLAYING||gameState===STATE.LEVELTRANSITION) return; gameState=STATE.PLAYING; player.x=W/2; player.cooldown=0; playerBullets=[]; enemyBullets=[]; particles=[]; spawnAliens(); scoreValueEl.textContent=score; livesValueEl.textContent=lives; levelValueEl.textContent=level; }
            function resetGame() { score=0; lives=3; level=1; player.x=W/2; player.cooldown=0; playerBullets=[]; enemyBullets=[]; particles=[]; aliens=[]; gameState=STATE.IDLE; transitionTimer=0; scoreValueEl.textContent='0'; livesValueEl.textContent='3'; levelValueEl.textContent='1'; }
            function gameOver() { gameState=STATE.GAMEOVER; spawnParticles(player.x,player.y,'#ff4444',30); spawnParticles(player.x,player.y,'#ffaa00',20); spawnParticles(player.x,player.y,'#ffffff',15); }
            function nextLevel() { level++; levelValueEl.textContent=level; gameState=STATE.LEVELTRANSITION; transitionTimer=TRANSITION_DURATION; playerBullets=[]; enemyBullets=[]; aliens=[]; player.cooldown=0; if(level%3===0&&lives<5) { lives++; livesValueEl.textContent=lives; } }

            function update() {
                if(gameState===STATE.LEVELTRANSITION) { transitionTimer--; updateParticles(); if(transitionTimer<=0) { gameState=STATE.PLAYING; spawnAliens(); player.x=W/2; } return; }
                if(gameState!==STATE.PLAYING) { updateParticles(); return; }
                if(keys['ArrowLeft']||keys['a']||keys['A']) player.x-=player.speed;
                if(keys['ArrowRight']||keys['d']||keys['D']) player.x+=player.speed;
                player.x=Math.max(player.width/2, Math.min(W-player.width/2, player.x));
                if(player.cooldown>0) player.cooldown--;
                if((keys[' ']||keys['Space']||keys['ArrowUp']||keys['w']||keys['W'])&&player.cooldown<=0) {
                    playerBullets.push({x:player.x, y:player.y-player.height/2, speed:BULLET_SPEED});
                    player.cooldown=player.cooldownMax;
                }
                alienMoveTimer++; if(alienMoveTimer>=alienMoveInterval) { alienMoveTimer=0; const leftEdge=getLeftmostAlien(); const rightEdge=getRightmostAlien(); let shouldDescend=false; if(alienDirection===1&&rightEdge+alienSpeed>=W-15){alienDirection=-1;shouldDescend=true;} else if(alienDirection===-1&&leftEdge-alienSpeed<=15){alienDirection=1;shouldDescend=true;} for(const alien of aliens){if(!alien.alive)continue; alien.x+=alienDirection*alienSpeed; if(shouldDescend) alien.y+=ALIEN_HEIGHT+ALIEN_PADDING_Y; alien.animTimer++; if(alien.animTimer>15){alien.animTimer=0;alien.animFrame++;} } if(getLowestAlienY()>=player.y-player.height/2-10){gameOver();return;} }
                alienShootTimer++; if(alienShootTimer>=alienShootInterval) { alienShootTimer=0; const aliveCount=countAliveAliens(); const shootsPerBurst=aliveCount<5?2:1; for(let i=0;i<shootsPerBurst;i++) alienShoot(); }
                for(let i=playerBullets.length-1;i>=0;i--){ playerBullets[i].y-=playerBullets[i].speed; if(playerBullets[i].y<-20){playerBullets.splice(i,1);continue;} let hit=false; for(const alien of aliens){ if(!alien.alive)continue; const bx=playerBullets[i].x,by=playerBullets[i].y; if(bx>alien.x&&bx<alien.x+alien.width&&by>alien.y&&by<alien.y+alien.height){ alien.alive=false; hit=true; const points=ALIEN_SCORES[alien.row]||10; score+=points; scoreValueEl.textContent=score; spawnParticles(alien.x+alien.width/2,alien.y+alien.height/2,ALIEN_COLORS[alien.row]||'#ff3366',18); break; } } if(hit) playerBullets.splice(i,1); }
                for(let i=enemyBullets.length-1;i>=0;i--){ enemyBullets[i].y+=enemyBullets[i].speed; if(enemyBullets[i].y>H+20){enemyBullets.splice(i,1);continue;} const bx=enemyBullets[i].x,by=enemyBullets[i].y; if(bx>player.x-player.width/2&&bx<player.x+player.width/2&&by>player.y-player.height/2&&by<player.y+player.height/2){ enemyBullets.splice(i,1); lives--; livesValueEl.textContent=lives; spawnParticles(player.x,player.y,'#ff4444',25); spawnParticles(player.x,player.y,'#ffffff',15); if(lives<=0){gameOver();return;} player.x=W/2; player.cooldown=30; } }
                if(countAliveAliens()===0) nextLevel();
                updateParticles();
            }

            function draw() {
                ctx.clearRect(0,0,W,H);
                const bgGrad = ctx.createLinearGradient(0,0,0,H); bgGrad.addColorStop(0,'#020212'); bgGrad.addColorStop(0.5,'#040818'); bgGrad.addColorStop(1,'#010108');
                ctx.fillStyle=bgGrad; ctx.fillRect(0,0,W,H);
                drawHUD();
                for(const alien of aliens) if(alien.alive) drawAlien(alien);
                if(gameState!==STATE.GAMEOVER) drawPlayer();
                for(const bullet of playerBullets) drawBullet(bullet,true);
                for(const bullet of enemyBullets) drawBullet(bullet,false);
                drawParticles(ctx);
                if(gameState===STATE.IDLE){ ctx.fillStyle='rgba(0,0,0,0.7)'; ctx.fillRect(0,0,W,H); ctx.fillStyle='#00ff88'; ctx.font='bold 32px "Courier New"'; ctx.textAlign='center'; ctx.shadowColor='#00ff88'; ctx.shadowBlur=20; ctx.fillText('SPACE INVADERS',W/2,H/2-50); ctx.shadowBlur=0; ctx.fillStyle='#ffffff'; ctx.font='18px "Courier New"'; ctx.fillText('Presiona ESPACIO para jugar',W/2,H/2+10); ctx.fillStyle='#aaccff'; ctx.font='14px "Courier New"'; ctx.fillText('← → mover   |   ESPACIO disparar   |   R reiniciar',W/2,H/2+45); ctx.textAlign='start'; }
                if(gameState===STATE.GAMEOVER){ ctx.fillStyle='rgba(0,0,0,0.75)'; ctx.fillRect(0,0,W,H); ctx.fillStyle='#ff3344'; ctx.font='bold 36px "Courier New"'; ctx.textAlign='center'; ctx.shadowColor='#ff0000'; ctx.shadowBlur=25; ctx.fillText('GAME OVER',W/2,H/2-30); ctx.shadowBlur=0; ctx.fillStyle='#ffcc00'; ctx.font='20px "Courier New"'; ctx.fillText(`Puntuación: ${score}   |   Nivel: ${level}`,W/2,H/2+20); ctx.fillStyle='#ffffff'; ctx.font='16px "Courier New"'; ctx.fillText('Presiona ESPACIO o R para reiniciar',W/2,H/2+55); ctx.textAlign='start'; }
                if(gameState===STATE.LEVELTRANSITION){ ctx.fillStyle='rgba(0,0,0,0.6)'; ctx.fillRect(0,0,W,H); const alpha=Math.abs(Math.sin(transitionTimer*0.08)); ctx.fillStyle=`rgba(255,204,0,${0.5+alpha*0.5})`; ctx.font='bold 30px "Courier New"'; ctx.textAlign='center'; ctx.shadowColor='#ffcc00'; ctx.shadowBlur=20; ctx.fillText(`¡NIVEL ${level}!`,W/2,H/2); ctx.shadowBlur=0; ctx.fillStyle='#ffffff'; ctx.font='14px "Courier New"'; ctx.fillText('Prepárate...',W/2,H/2+35); ctx.textAlign='start'; }
            }

            function gameLoop(time) { drawStars(time); update(); draw(); requestAnimationFrame(gameLoop); }
            spawnAliens(); gameState=STATE.IDLE; requestAnimationFrame(gameLoop);
        })();
    </script>
</body>
</html>

</details>
📸 Galería
Próximamente: capturas de pantalla del juego en funcionamiento.

🤝 Contribuciones
¿Ideas para mejorar? ¡Las contribuciones son bienvenidas! Puedes proponer cambios en el código, mejorar los gráficos o añadir nuevas funcionalidades.

¡Buena suerte, comandante! La humanidad confía en ti. 🌌
