<script>
const morphTime = 0.125;
const cooldownTime = 0.25;

export default {
    name: "TextMorph",
    props: {
        inputText: String,
        morphSpeed: {
            type: Number,
            default: morphTime
        }
    },
    data() {
        return {
            currentText: "",
            time: new Date(),
            morph: 0,
            cooldown: cooldownTime,
        }
    },
    mounted() {
        this.animate();
    },
    watch: {
        inputText(newVal, oldVal) {
            this.time = new Date();
            this.cooldown = 0;
            this.animate();
        }
    },
    methods: {
        doMorph() {
            this.morph -= this.cooldown;
            this.cooldown = 0;

            let fraction = this.morph / this.morphSpeed;

            if (fraction > 1) {
                this.cooldown = cooldownTime;
                fraction = 1;
            }

            this.setMorph(fraction);
        },
        setMorph(fraction) {
            this.$refs.text2.style.filter = `blur(${Math.min(8 / fraction - 8, 100)}px)`;
            this.$refs.text2.style.opacity = `${Math.pow(fraction, 0.4) * 100}%`;

            fraction = 1 - fraction;
            this.$refs.text1.style.filter = `blur(${Math.min(8 / fraction - 8, 100)}px)`;
            this.$refs.text1.style.opacity = `${Math.pow(fraction, 0.4) * 100}%`;
        },
        doCooldown() {
            this.morph = 0;
            
            this.$refs.text2.style.filter = "";
            this.$refs.text2.style.opacity = "100%";
            
            this.$refs.text1.style.filter = "";
            this.$refs.text1.style.opacity = "0%";
        },
        animate() {
            let newTime = new Date();
            let dt = (newTime - this.time) / 1000;
            this.time = newTime;

            this.cooldown -= dt;

            if (this.cooldown <= 0.05) {
                requestAnimationFrame(this.animate);
                this.doMorph();
            }
            else {
                this.currentText = this.inputText;
                this.doCooldown();
            }
        }
    }
}
</script>

<template>
    <div>
        <div class="morphTextContainer">
        <span class="morphText" ref="text1">{{ currentText }}</span>
        <span class="morphText" ref="text2">{{ inputText }}</span>
    </div>

    <svg id="filters">
        <defs>
            <filter id="threshold">
                <feColorMatrix in="SourceGraphic" type="matrix" values="1 0 0 0 0
                                        0 1 0 0 0
                                        0 0 1 0 0
                                        0 0 0 255 -140" />
            </filter>
        </defs>
    </svg>
    </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css?family=Afacad");

.morphTextContainer {
    display: flex;
    justify-content: center;
    width: 100vw;
    filter: url(#threshold) blur(0.6px);
}

.morphText {
    position: absolute;
    display: inline-block;

    font-family: "Afacad", sans-serif;
    font-size: 80pt;

    text-align: center;

    user-select: none;
    color: #fff;
}

</style>