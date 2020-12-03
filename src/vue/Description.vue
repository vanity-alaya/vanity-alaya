<template>
    <div class="panel">
        <p>
            Vanity-Alaya is an open source tool using your web browser to generate Alaya vanity addresses.<br>
            Enter a short prefix/suffix of your choice, and click ‘generate’ to start.
        </p>
        <div class="shortcut">
            <button type="button" class="button-large" @click="scrollDown">开始</button>
        </div>

        <h2>什么是Alaya地址生成器?</h2>
        <p>
            Vanity address可以理解为靓号地址, Alaya地址生成器通过生成大量的地址来匹配指定的前缀或后缀, 指定的字符越长所需时间越久
            <br>
            例如: <span class="monospace">atp1000000000000000000000000000000000abcde</span>, 或
            <span class="monospace">atp1888888888888888888888888888888888abcde</span>
        </p>

        <h2>Alaya地址生成器是否安全?</h2>
        <p>
            Alaya地址生成器<span class="monospace">不会保存私钥所以请做好备份, 建议您打开网页后在断网情况下运行</span>, 代码完全开源
        </p>
    </div>
</template>

<script>
    export default {
        data: function () {
            return {
                scrollTimeOut: null
            }
        },
        methods: {
            scrollDown() {
                this.scrollTo(document.getElementById('input-panel'), -1);
                this.$root.$emit('event', 'Start now');
            },
            scrollTo(element, lastValue) {
                let currentValue = window.scrollY;
                let diff = element.getBoundingClientRect().top / 6;
                if (Math.abs(diff) > 1 && currentValue > lastValue) {
                    window.scrollTo(0, (window.scrollY + diff));
                    this.scrollTimeOut = setTimeout(this.scrollTo, 30, element, currentValue)
                } else if (currentValue >= lastValue) {
                    document.getElementById('input').focus();
                }
            }
        }
    };
</script>

<style lang="sass" scoped>
    @import "../css/variables"
    p
        margin: 15px 0 20px
        color: $text-alt
        overflow-x: hidden
        text-overflow: ellipsis
        .monospace
            font-family: $monospace-font
            font-size: 0.85em
            color: green
    .shortcut
        text-align: center
        .button-large
            width: 150px
            margin: 15px 0 35px
</style>
