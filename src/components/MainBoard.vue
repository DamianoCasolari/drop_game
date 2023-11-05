<script>
import { SeaPollutionSentences } from '../Js/ListSentences'

export default {
    name: "MainBoard",
    data() {
        return {
            idDrop: 0,
            firstGame: true,
            DropY: 0,
            timeGapDrops: 3000,
            totalPoints: 0,
            dropsEscaped: 0,
            randomNumber: 0,
            numberSpokes: 4,
            insideGameSpace: false,
            levelPollution: 0,
            levelGame: 20,
            newLevel: false,
            startGame: false,
            intervalId: null,
            pollutionSentence: "",

        }
    },
    methods: {
        // ANIMATIONS
        generateRandomNumber() {
            return Math.floor(Math.random() * 255);
        },
        generateRandomSentence() {
            const numberRandom = Math.floor(Math.random() * 23);
            return SeaPollutionSentences[numberRandom]
        },
        mojsLives(distX, distY) {
            const COLORS = {
                RED: '#FD5061',
                YELLOW: '#FFCEA5',
                BLACK: '#29363B',
                WHITE: 'white',
                VINOUS: '#A50710'
            }

            const DURATION = 500;

            const circle = new mojs.Shape({
                left: 0, top: 0,
                strokeWidth: 10,
                fill: 'none',
                radius: 150,
                scale: { 0: 1 },
                opacity: { 1: 0 },
                shape: 'circle',
                stroke: 'white',
                strokeWidth: 10,
                fill: 'none',
                duration: 1.5 * DURATION,
                isForce3d: true,
                isTimelineLess: true,
            });

            const cloud = new mojs.Burst({
                left: 0, top: 0,
                radius: { 4: 49 },
                angle: 45,
                count: 12,
                children: {
                    radius: 10,
                    fill: 'white',
                    scale: { 1: 0, easing: 'sin.in' },
                    pathScale: [.7, null],
                    degreeShift: [13, null],
                    duration: [500, 700],
                    isShowEnd: false,
                    isForce3d: true
                }
            });

            const burst = new mojs.Burst({
                left: 0, top: 0,
                radius: { 0: 280 },
                count: 2,
                angle: 90,
                children: {
                    shape: 'rect',
                    fill: COLORS.VINOUS,
                    radius: 15,
                    duration: DURATION,
                    isForce3d: true
                }
            });

            const burst2 = new mojs.Burst({
                left: 0, top: 0,
                count: 5,
                radius: { 0: 150 },
                angle: 90,
                children: {
                    shape: 'line',
                    stroke: COLORS.VINOUS,
                    strokeWidth: 5,
                    strokeLinecap: 'round',
                    radius: 25,
                    angle: { 0: 360 },
                    scale: 1,
                    scaleX: { 1: 0 },
                    duration: DURATION,
                    isForce3d: true
                }
            });



            burst
                .tune({ x: distX, y: distY })
                .generate()
                .replay();
            ;
            burst2
                .tune({ x: distX, y: distY })
                .generate()
                .replay();
            ;
            cloud
                .tune({ x: distX, y: distY })
                .generate()
                .replay();
            ;
            circle
                .tune({ x: distX, y: distY })
                .generate()
                .replay();
            ;
        },
        mojsPoints(distx, disty, random) {
            const DURATION = 400
            const LIGHTGRAY = random

            const smoke = new mojs.Burst({
                left: 0, top: 0,
                degree: 0,
                count: 3,
                radius: { 0: 100 },
                children: {
                    fill: LIGHTGRAY,
                    pathScale: 'rand(0.5, 1)',
                    radius: 'rand(12, 15)',
                    swirlSize: 'rand(10, 15)',
                    swirlFrequency: 'rand(2, 4)',
                    direction: [1, -1],
                    duration: `rand(${1 * DURATION}, ${2 * DURATION})`,
                    delay: 'rand(0, 75)',
                    easing: 'quad.out',
                    isSwirl: true,
                    isForce3d: true,
                }
            });



            smoke
                .tune({ x: distx, y: disty })
                .generate()
                .replay();

        },
        mojsAnimation(e) {
            const COLORS = {
                RED: '#FD5061',
                YELLOW: '#FFCEA5',
                BLACK: '#29363B',
                WHITE: 'white',
                VINOUS: '#A50710',
                GRREN: 'RGB(0, 255, 26)',
                BLUE: '#8a93c3',
            }


            const burst2 = new mojs.Burst({
                left: 0, top: 0,
                count: this.numberSpokes,
                radius: { 50: 250 },
                children: {
                    fill: 'white',
                    shape: 'line',
                    stroke: [COLORS.RED, COLORS.YELLOW, COLORS.GRREN, COLORS.BLUE],
                    strokeWidth: 12,
                    radius: 'rand(30, 60)',
                    radiusY: 0,
                    scale: { 1: 0 },
                    pathScale: 'rand(.5, 1)',
                    degreeShift: 'rand(-360, 360)',
                    isForce3d: true,
                }
            });

            burst2.tune({ x: e.pageX, y: e.pageY })
                .generate()
                .replay();
        },
        mojsLevel() {
            const meteors = new mojs.Burst({
                left: 0, top: 0,
                count: 3,
                radius: { 0: 250 },
                degree: 0,

                children: {
                    shape: 'line',
                    stroke: ['#F9DD5E', '#FC2D79', '#11CDC5'],
                    duration: 1000,
                    radius: 60,
                    strokeWidth: 8,
                    left: 'stagger(20)',
                    top: 'stagger(20)',
                    delay: [0, 200, 100],
                    isForce3d: true
                }
            });

            meteors
                .tune({ x: window.innerWidth / 2, y: window.innerHeight / 2 })
                .replay();
            ;

        },
        mojsFinishGame() {
            const COLORS = {
                RED: '#FD5061',
                YELLOW: '#FFCEA5',
                // BLACK: '#29363B',
                WHITE: 'white',
                VINOUS: '#A50710'
            }

            const burst1 = new mojs.Burst({
                left: 0, top: 0,
                count: 5,
                radius: { 50: 250 },
                children: {
                    fill: 'white',
                    shape: 'line',
                    stroke: [COLORS.WHITE, COLORS.VINOUS],
                    strokeWidth: 12,
                    radius: 'rand(30, 60)',
                    radiusY: 0,
                    scale: { 1: 0 },
                    pathScale: 'rand(.5, 1)',
                    degreeShift: 'rand(-360, 360)',
                    isForce3d: true,
                }
            });

            const burst2 = new mojs.Burst({
                top: 0, left: 0,
                count: 3,
                radius: { 0: 250 },
                children: {
                    shape: ['circle', 'rect'],
                    points: 5,
                    fill: [COLORS.WHITE, COLORS.VINOUS],
                    radius: 'rand(30, 60)',
                    scale: { 1: 0 },
                    pathScale: 'rand(.5, 1)',
                    isForce3d: true
                }
            });

            const CIRCLE_OPTS = {
                left: 0, top: 0,
                fill: COLORS.WHITE,
                scale: { .2: 1 },
                opacity: { 1: 0 },
                isForce3d: true,
                isShowEnd: false
            }

            const circle1 = new mojs.Shape({
                ...CIRCLE_OPTS,
                radius: 200,
            });

            const circle2 = new mojs.Shape({
                ...CIRCLE_OPTS,
                radius: 240,
                easing: 'cubic.out',
                delay: 150,
            });

            // document.addEventListener('click', function (e) {
            burst1
                .tune({ x: window.innerWidth / 2, y: window.innerHeight / 2 })
                .generate()
                .replay();

            burst2
                .tune({ x: window.innerWidth / 2, y: window.innerHeight / 2 })
                .generate()
                .replay();

            circle1
                .tune({ x: window.innerWidth / 2, y: window.innerHeight / 2 })
                .replay();

            circle2
                .tune({ x: window.innerWidth / 2, y: window.innerHeight / 2 })
                .replay();

            // bgBurst
            //     .tune({ x: window.innerWidth / 2, y: window.innerHeight / 2 })
            //     .replay();
            // });
        },
        mojsSmoke() {

            const BUST_OPTS = {
                parent: '.game_space',
                count: 'rand(3,5)',
                degree: 40,
                angle: -25,
                radius: { 25: 75 },
                children: {
                    // fill: ['#bbbbbb', '#040404', '#7f7f7f'],
                    fill: ['yellow', 'red', 'green', 'blue'],
                    fill: ['#fcfcfc', '#fefefe'],
                    radius: 'rand(8, 15)',
                    pathScale: ['rand(0, .5)', 'rand(.35, 1)'],
                    degreeShift: 'rand(.2, 1)',
                    swirlFrequency: 'rand(3, 5)',
                    direction: [1, -1],
                    isSwirl: true,
                    duration: 1000,
                    easing: 'cubic.out',
                    isForce3d: true
                }
            }

            const burst1 = new mojs.Burst({
                ...BUST_OPTS,
                left: '60 %',
                top: '60 %',
                y: { 0: -65 },
            });

            const burst2 = new mojs.Burst({
                ...BUST_OPTS,
                left: '55 %',
                top: '50 %',
                y: { 0: -50 }
            });

            const burst3 = new mojs.Burst({
                ...BUST_OPTS,
                left: '40 %',
                top: '44 %',
                y: { 0: -50 }
            });

            const burst4 = new mojs.Burst({
                ...BUST_OPTS,
                left: '45 %',
                top: '53 %',
                y: { 0: -35 }
            });


            burst1.replay();
            burst2.replay();
            burst3.replay();
            burst4.replay();


        },
        mojsConfetti() {
            const BUST_OPTS = {
                x: { 0: -300 },
                y: { 0: -50 },
                radius: { 50: 150 },
                angle: { 0: -25 },
                degree: 200,
                isForce3d: true
            }

            const CHILD_OPTS = {
                shape: ['circle', 'rect'],
                fill: ['white', 'yellow'],
                radius: 'rand(7, 10)',
                opacity: .75,
                isSwirl: true,
                pathScale: 'rand(.3, 1)',
                degreeShift: 'rand(-30, 30)',
                swirlFrequency: 5,
                duration: 1400,
                easing: 'linear.none',
                isForce3d: true,
            }

            const burst1 = new mojs.Burst({
                ...BUST_OPTS,
                parent: '.game_space',
                width: 320,
                height: 320,
                children: {
                    ...CHILD_OPTS
                }
            });

            const burst2 = new mojs.Burst({
                ...BUST_OPTS,
                parent: '.game_space',
                radius: { 30: 60 },
                count: 3,
                width: 140,
                height: 140,
                timeline: { delay: 585 },
                children: {
                    ...CHILD_OPTS
                }
            });


            burst1
                .generate()
                .replay();

            burst2
                .generate()
                .replay();


        },
        waveTitle() {
            this.$nextTick(() => {
                let titleCOntainer = document.querySelector('#gameOver_container');
                let titleContent = titleCOntainer.textContent
                const letters = titleContent.split('')
                titleCOntainer.innerHTML = '';

                letters.forEach((letter, index) => {
                    titleCOntainer.insertAdjacentHTML('beforeend', `<span class="letter" style="animation-delay: ${index * 100 + 1000}ms">${letter}</span>`)
                })

            })

        },
        customCursor() {
            const cursorImage = document.getElementById('cursor-image');
            const gameSpace = document.querySelector(".game_space");

            // Aggiungi un gestore di eventi all'elemento .game_space per rilevare il movimento del cursore
            gameSpace.addEventListener('mousemove', (e) => {
                // Calcola le coordinate relative all'elemento .game_space
                const posX = e.clientX - gameSpace.getBoundingClientRect().left;
                const posY = e.clientY - gameSpace.getBoundingClientRect().top;

                // Imposta le coordinazioni dell'immagine in base alla posizione del cursore
                cursorImage.style.left = posX - cursorImage.width / 2 + 'px';
                cursorImage.style.top = posY + 'px';
            });

            // Mostra l'immagine quando il mouse entra nell'area specifica
            gameSpace.addEventListener('mouseenter', () => {
                cursorImage.style.display = 'block';
            });

            // Nascondi l'immagine quando il mouse esce dall'area
            gameSpace.addEventListener('mouseleave', () => {
                cursorImage.style.display = 'none';
            });

        },
        // LOGIC SIDE METHODS 
        createDrop() {
            const SpecificID = this.idDrop;
            const container = document.querySelector('.game_space');
            const maxWidth = container.clientWidth;
            const randomValue = Math.random();
            let positionDrop = Math.abs(Math.floor(maxWidth * randomValue - 20));

            const drop = `<div id="drop${this.idDrop}" class="drop position-absolute z-3" style="right:calc(${positionDrop}px);"></div>`;

            container.insertAdjacentHTML('beforeend', drop);
            const lastDrop = document.getElementById(`drop${this.idDrop}`);

            const options = {
                root: container,
                rootMargin: '30px',
                threshold: 1.0
            };

            let observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if (!entry.isIntersecting) {
                        const livesContainer = document.querySelector(".lives_container")
                        if (livesContainer) {
                            if (livesContainer.children.length > 0 && !this.insideGameSpace && this.startGame) {
                                const lastChild = livesContainer.lastElementChild;
                                const lastChildX = lastChild.getBoundingClientRect().left + lastChild.clientWidth / 2;
                                const lastChildY = lastChild.getBoundingClientRect().top + lastChild.clientHeight / 2;
                                this.mojsLives(lastChildX, lastChildY)
                                livesContainer.removeChild(lastChild);
                                this.levelPollution += 0.4;
                                this.dropsEscaped++
                                if (this.dropsEscaped == 3) {
                                    this.startGame = false
                                    clearInterval(this.intervalId);
                                    this.mojsFinishGame()
                                    this.mojsSmoke()
                                    this.waveTitle()

                                }
                            }
                        }
                        this.insideGameSpace = false;
                        observer.unobserve(lastDrop);
                    }
                });
            }, options);

            observer.observe(lastDrop);



            lastDrop.addEventListener('mouseover', (e) => { this.pressDrop(SpecificID); this.mojsAnimation(e); });
            this.idDrop++;


        },
        pressDrop(dropID) {

            const drop = document.getElementById(`drop${dropID}`);
            let points = Math.floor((drop.getBoundingClientRect().top) / 100 - 1);
            this.totalPoints += points;
            const gameSpace = document.querySelector(".game_space")
            const distGamespaceFromTop = gameSpace.getBoundingClientRect().top
            if (drop.getBoundingClientRect().top - distGamespaceFromTop < gameSpace.clientHeight) {

                this.insideGameSpace = true;
            } else {
                this.insideGameSpace = false;
            }
            drop.remove()
            const displaypoints = document.querySelector(".totalPoints")
            const displaypointsX = displaypoints.getBoundingClientRect().left - displaypoints.clientWidth / 2;
            const displaypointsY = displaypoints.getBoundingClientRect().top + displaypoints.clientHeight + 15;
            const rundomColor = 'rgb(' + this.generateRandomNumber() + ',' + this.generateRandomNumber() + ',' + this.generateRandomNumber() + ')'
            this.mojsPoints(displaypointsX, displaypointsY, rundomColor)
            if (this.totalPoints >= this.levelGame) {
                this.mojsLevel()
                this.mojsConfetti()
                this.numberSpokes += 4
                this.levelGame += 20
                if (this.timeGapDrops > 1000) {
                    this.timeGapDrops -= 400
                } else {
                    this.timeGapDrops = this.timeGapDrops / 2
                }
                this.newLevel = true
                this.$nextTick(() => {
                    document.querySelector('.level_game').classList.add('fade_in_out')
                    setTimeout(() => {
                        this.newLevel = false
                        document.querySelector('.level_game').classList.remove('fade_in_out')
                    }, 1000)
                });
                clearInterval(this.intervalId);
                this.intervalId = setInterval(() => {
                    this.createDrop();
                }, this.timeGapDrops);

            }

        },
        startGameFunction() {
            this.firstGame = false
            this.startGame = true
            this.levelPollution = 0
            this.totalPoints = 0
            this.dropsEscaped = 0
            this.levelGame = 20
            this.levelGame = 20
            this.timeGapDrops = 3000
            this.numberSpokes = 4
            this.pollutionSentence = this.generateRandomSentence()



            this.$nextTick(() => {
                const lives_container = document.querySelector(".lives_container");

                if (lives_container) {
                    if (lives_container.children.length == 0) {

                        for (let index = 0; index < 3; index++) {
                            lives_container.insertAdjacentHTML('beforeend',
                                ` <div class="heart_container thrill${index + 1} me-1 position-relative">
                                <img width="30" src="../../public/pngwing.png" alt="heart_icon">
                              </div>`
                            )

                        }
                    }
                }
                this.intervalId = setInterval(() => {
                    this.createDrop();
                }, this.timeGapDrops);
            });
        },

    },
    mounted() {
        this.mojsConfetti()
        this.mojsSmoke()
        this.pollutionSentence = this.generateRandomSentence()
    }

}

</script>

<template>
    <section class="main_board py-4 mx-4">

        <div class="game_space h-100 m-auto position-relative cursor_none">

            <!-- START/END GAME SIDE -->
            <div v-if="!startGame"
                class="overlay_start flex-column position-absolute h-100 w-100 d-flex justify-content-center align-items-center fade_in">
                <div v-if="!firstGame" class="fw-bold mb-5">
                    <strong id="gameOver_container" class="fs-1 me-2 ">
                        Game Over
                    </strong>
                </div>
                <div v-if="!firstGame" class="fw-bold mb-5">
                    <strong class="fs-1 me-2 thrill1 d-inline-block ">
                        {{ totalPoints }}
                    </strong>
                    <span class="fs-5">
                        Score
                    </span>
                </div>
                <div v-if="firstGame"
                    class="d-flex justify-content-center align-items-center mb-3 fs-5 lh_2 mx-3 text-center fade_in">
                    {{ pollutionSentence }}
                </div>
                <div @click="firstGame = true" v-if="!firstGame" @mouseover="mojsConfetti(); mojsSmoke()"
                    class="fade_in hover_scale click_effect button_start mx-3 text-center fs-5 lh_2">
                    Did you know that...
                </div>
                <div @click="startGameFunction()" v-if="firstGame" @mouseover="mojsConfetti(); mojsSmoke()"
                    class="fade_in hover_scale click_effect button_start mx-3 text-center fs-5 lh_2">
                    Let's protect
                    the <strong class="">sea</strong>
                </div>
            </div>
            <!-- LEVEL GAME SIDE -->
            <div
                class="level_game flex-column position-absolute h-100 w-100 d-flex justify-content-center align-items-center z-1">
                <div v-show="newLevel" class=" mx-3 text-center fs-1 position-absolute z-1">Level {{ levelGame / 20 }}</div>
            </div>

            <!-- HEART/POINTS GAME SIDE -->
            <div class="fade_in d-flex justify-content-between align-items-center m-3" v-if="startGame">

                <div class="lives_container text-start dropsEscaped fw-bold d-flex">
                    <div class="heart_container thrill1 me-1 position-relative">
                        <img width="30" src="../../public/pngwing.png" alt="heart_icon">
                    </div>
                    <div class="heart_container thrill2 me-1">
                        <img width="30" src="../../public/pngwing.png" alt="heart_icon">
                    </div>
                    <div class="heart_container thrill3 me-1">
                        <img width="30" src="../../public/pngwing.png" alt="heart_icon">
                    </div>
                </div>

                <div class="text-end points_container fw-bold"><strong class="fs-3 me-2 totalPoints">{{
                    totalPoints
                }}</strong><span class="fs_8">
                        Score</span></div>
            </div>

            <!-- WAVES SIDE -->
            <div class='sea sea1' :style="{ filter: `invert(${levelPollution})` }"></div>
            <div class='sea sea2' :style="{ filter: `invert(${levelPollution})` }"></div>
            <div class='sea sea3' :style="{ filter: `invert(${levelPollution})` }"></div>
            <div class='sea sea4' :style="{ filter: `invert(${levelPollution})` }"></div>
            <div v-if="startGame" class="fade_left z_10000 display_level position-absolute end-0 bottom-0 p-3"
                :class="levelPollution > 0 ? 'text-white' : 'text-dark '">Level {{ levelGame / 20 }}
            </div>

        </div>

    </section>
</template>

<style lang="scss"></style>