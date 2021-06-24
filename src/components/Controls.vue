<template>
    <div>
        <div class="game-of-life__controls--instructions">
            <p><span style="color: black">BLACK</span> = alive | <span style="color: green">GREEN</span> = dead</p>
            <p>Click on cells to toggle their states. Toggle slider to change generation frequency.</p>
        </div>
        <div class="game-of-life__controls">
            <b-button class="game-of-life__button--pause" @click="playClicked"><b-icon icon="play" /></b-button>
            <input class="game-of-life__slider" type="range" :min="minSlider" :max="maxSlider" step="1" v-model="sliderVal" @change="sliderChanged" />
            <b-button class="game-of-life__button--play" @click="pauseClicked"><b-icon icon="pause" /></b-button>
        </div>
    </div>
</template>

<script>

export default {
    name: 'Controls',
    data() {
        return {
            sliderVal: 0,
            interval: null
        };
    },
    props: {
        minSlider: {
            type: Number,
            default: 0
        },
        maxSlider: {
            type: Number,
            default: 20
        }
    },
    computed: {
        //
    },
    methods: {
        pauseClicked() {
            this.sliderVal = 0;
            clearInterval(this.interval);
            this.$emit('paused');
        },
        playClicked() {
            this.sliderVal = this.maxSlider / 2;
            this.setInterval();
        },
        sliderChanged() {
            this.setInterval();
            this.$emit('slider-val-changed', this.sliderVal);
        },
        setInterval() {
            clearInterval(this.interval);
            if (this.sliderVal > 0) {
                const numPerSec = 1000 / this.sliderVal;
                this.interval = setInterval(() => this.$emit('iterate-invoked'), numPerSec);
            }
        }
    },
    created() {
    },
    beforeDestroy() {
        clearInterval(this.interval);
    }
}
</script>

<style lang="scss" scoped>
.game-of-life {
    &__controls {
        display: flex;
        justify-content: center;

        &--instructions {
            margin-top: 10px;
            text-align: center;
        }
    }

    &__button--pause {
        margin: 5px;
    }

    &__button--play {
        margin: 5px;
    }
}
</style>