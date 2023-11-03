<script>

export default {
    name: "MainBoard",
    data() {
        return {
            idDrop: 0,
            DropY: 0,
            totalPoints: 0,
            dropsEscaped: 0,
            randomNumber: 0,
            numberSpokes: 4,
            insideGameSpace: false,
            levelPollution: 0

        }
    },
    methods: {
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

            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if (!entry.isIntersecting) {
                        this.dropsEscaped++
                        const livesContainer = document.querySelector(".lives_container")
                        if (livesContainer.children.length > 0 && !this.insideGameSpace) {
                            const lastChild = livesContainer.lastElementChild;
                            const lastChildX = lastChild.getBoundingClientRect().left + lastChild.clientWidth / 2;
                            const lastChildY = lastChild.getBoundingClientRect().top + lastChild.clientHeight / 2;
                            this.mojsLives(lastChildX, lastChildY)
                            livesContainer.removeChild(lastChild);
                            this.levelPollution += 0.4;
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
        pressDrop(dropID) {

            const drop = document.getElementById(`drop${dropID}`);
            let points = Math.floor((drop.getBoundingClientRect().top) / 100 - 1);
            this.totalPoints += points;
            this.dropsEscaped--
            const gameSpace = document.querySelector(".game_space")
            const distGamespaceFromTop = gameSpace.getBoundingClientRect().top
            if (drop.getBoundingClientRect().top - distGamespaceFromTop < gameSpace.clientHeight) {

                this.insideGameSpace = true;
            } else {
                this.insideGameSpace = false;
            }
            drop.classList.add("d-none")

        },


    },
    mounted() {
        const startGame = setInterval(() => {
            this.createDrop();
        }, 3000);
        setTimeout(() => {
            clearInterval(startGame);
        }, 24000);

    }

}

</script>

<template>
    <section class="main_board p-4">
        <div class="game_space h-100 container-xl position-relative p-3 cursor_none">
            <img id="cursor-image" src="../../public/1457.svg" class="custom_cursor"
                style="position: absolute; display: none;">
            <div class="d-flex justify-content-between align-items-center">
                <div class="lives_container text-start dropsEscaped fw-bold d-flex">
                    <div class="heart_container thrill me-1 position-relative">
                        <img width="30" src="../../public/pngwing.png" alt="heart_icon">
                    </div>
                    <div class="heart_container thrill2 me-1">
                        <img width="30" src="../../public/pngwing.png" alt="heart_icon">
                    </div>
                    <div class="heart_container thrill3 me-1">
                        <img width="30" src="../../public/pngwing.png" alt="heart_icon">
                    </div>
                </div>
                <div class="text-end totalPoints fw-bold"> {{ totalPoints }} Score</div>
            </div>
            <div class='sea sea1' :style="{ filter: `invert(${levelPollution})` }"></div>
            <div class='sea sea2' :style="{ filter: `invert(${levelPollution})` }"></div>
            <div class='sea sea3' :style="{ filter: `invert(${levelPollution})` }"></div>
            <div class='sea sea4' :style="{ filter: `invert(${levelPollution})` }"></div>
            <!-- <div class='sea sea2' :style="{ filter: 'invert(' + levelPollution + ')' }"></div> -->

        </div>
    </section>
</template>

<style lang="scss"></style>