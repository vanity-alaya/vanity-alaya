<template>
    <div class="panel">
        <div>难度: <span class="output" v-text="formatNum(difficulty)">1</span></div>
        <div>预估: <span class="output" v-text="probability50">0 addresses</span></div>
        <div>已生成:
            <span class="output" v-text="formatNum(count) + (count === 1 ? ' address' : ' addresses')">0 addresses</span>
        </div>
        <div>速度: <span class="output" v-text="speed + ' addr/s'">0 addr/s</span></div>
        <div>状态: <span class="output" v-text="status">Waiting</span></div>

        <!--Probability-->
        <div class="probability">
            <div class="probability-bar" :style="'width:' + probability + '%'"></div>
        </div>
        <div class="percentage">
            <h4 v-text="probability + '%'">0%</h4>
        </div>
    </div>
</template>

<script>
    const computeDifficulty = function (pattern, isChecksum) {
        const ret = Math.pow(36, pattern.length);
        return isChecksum ? (ret * Math.pow(2, pattern.replace(/[^a-f]/gi, '').length)) : ret;
    };

    const computeProbability = function (difficulty, attempts) {
        return 1 - Math.pow(1 - (1 / difficulty), attempts);
    };

    export default {
        data: function () {
            return {
                speed: 0,
                count: 0
            };
        },
        props: {
            hex: String,
            checksum: Boolean,
            status: String,
            firstTick: {}
        },
        watch: {
            hex() {
                this.count = 0;
            },
            checksum() {
                this.count = 0;
            }
        },
        computed: {
            difficulty: function () {
                return this.inputError ? 'N/A' : computeDifficulty(this.hex, this.checksum);
            },
            probability50: function () {
                return this.inputError ? 'N/A' : this.formatNum(Math.floor(Math.log(0.5) / Math.log(1 - (1 / this.difficulty)))) + ' addresses';
            },
            probability: function () {
                return Math.round(10000 * computeProbability(this.difficulty, this.count)) / 100;
            }
        },
        methods: {
            formatNum: function (num) {
                return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ' ');
            }
        },
        created: function () {
            this.$parent.$on('increment-counter', (incr) => {
                this.count += (incr > 0 ? incr : -this.count);
                this.speed = incr > 0 ? Math.floor(1000 * this.count / (performance.now() - this.firstTick)) : 0;
            });
        }
    };

</script>

<style lang="sass" scoped>
    @import "../css/variables"
    .panel > div:not(:last-child)
        margin-bottom: 17px

    .panel
        min-height: 280px
        padding-bottom: 3.2em
        > div:not(.percentage)
            clear: both

    .probability
        width: 85%
        margin: 5px 0
        height: 18px
        background: $panel-background-alt
        float: left

    .probability-bar
        height: 100%
        width: 0
        display: block
        background-color: $primary

    .percentage
        float: right
        width: 15%
        text-align: center
        position: relative
        top: 0px
        left: 15px
        div
            font-size: 12px
        h5
            color: $text
            font-weight: 500

    .output
        font-family: $monospace-font
        color: $text-alt
        margin-left: 15px
        word-break: break-all

    @media screen and (max-width: 480px)
        .percentage
            left: -5px
        .probability
            width: 80%
</style>
