<script>

export default {
    name: "AppHeader",
    data() {
        return {
            randomNumber: 0
        }
    },
    methods: {
        generateRandomNumber() {
            this.randomNumber = Math.floor(Math.random() * 360);
        },
    },
    mounted() {
        setInterval(() => {
            this.generateRandomNumber()
        }, 8000)

    }

}
</script>

<template>
    <header class="d-flex justify-content-end me-4 me-md-3 justify-content-end align-items-center position-relative">
        <div class="game_title fs-4">Block <br> pollution</div>
        <div class="header_drop position-absolute" :style="{ filter: `hue-rotate(${randomNumber}deg)` }">
        </div>
        <div class="img_container h-100">
            <img class="h-100 position-absolute" src="../../public/8707358.png" alt="">
        </div>
    </header>
</template>

<style lang="scss"></style>