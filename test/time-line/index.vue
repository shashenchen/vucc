<template>
    <v-time-line :data="timeLineData"></v-time-line>

    <a class="demo-link" href="../components/time-line.html">组件连接</a>
</template>

<script>
    import vTimeLine from '../../src/components/time-line';

    export default {
        components: {
            vTimeLine
        },
        data() {
            return {
                timeLineData: ['我们发布了alpha \n\r 再试试', '我们发布了beta']
            }
        }
    }
</script>