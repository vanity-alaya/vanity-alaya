<template>
    <footer>
        <div class="container">
            <div class="row">
                <div class="col-12 col-lg-6 address">
                    赏杯奶茶:
                    <a class="main" :href="`https://scan.alaya.network/address-detail?address=${tipsAddress}`" target="_blank" v-text="tipsAddress">
                    </a>
                </div>
                <div class="col-12 col-lg-6 links">
                    <a href="https://github.com/vanity-alaya" target="_blank">
                        Github | 源码
                    </a>
                </div>
            </div>
        </div>
    </footer>
</template>

<script>
    export default {
        data: function () {
            return {
                tipsAddress: 'atp100050lk4a9dfuylpmjkk2waf5yza4gwnhtuv2l\n'
            };
        }
    };
</script>

<style lang="sass" scoped>
    @import "../css/variables"
    footer
        padding: 1rem 0 0.5rem
        background-color: $panel-background
        color: $text-alt
        a
            text-decoration: none
        .address
            margin-bottom: 20px
            position: relative
            color: $text
            a
                font-family: $monospace-font
                margin-left: 15px
                word-break: break-all

        .links
            text-align: right
            a
                margin-right: 30px
                padding-bottom: 2px
                i
                    font-size: 1.2em

    @media screen and (max-width: 480px)
        footer
            padding-bottom: 1em
</style>
