<template>

    <g v-if="nodeData.nodeType=='rect'" :transform=" 'translate('+ nodeData.data.x +','+ nodeData.data.y +')'" style="user-select: none"
       @mousedown="downNode" >
        <rect height="50" width="100"></rect>

        <foreignObject width="100" height="40" style="overflow: visible">
            <!--<img width="100" height="40" src="./image_1.png">-->
        </foreignObject>
        <text x="50" y="25" dominant-baseline="central" text-anchor="middle">TEST</text>
    </g>

</template>

<script>
    export default {
        name: "BaseNode",
        props:['nodeData'],

        data(){
            return{
                nodeData: this.nodeData

            }
        },
        methods: {

            downNode(e) {
                console.log("down event")
                let dx = e.layerX - this.nodeData.data.x
                let dy = e.layerY - this.nodeData.data.y

                //按下鼠标的同时开启对鼠标移动的监听
                document.onmousemove = (e) => {
                    this.nodeData.data.x = e.layerX - dx
                    this.nodeData.data.y = e.layerY - dy
                }

                //鼠标抬起时 释放所有监听
                document.onmouseup = function () {
                    document.onmousemove = null
                    document.onmouseup = null
                }
            },

        }
    }
</script>

<style scoped>
    g rect {
        stroke-width: 1;
        stroke: #34b0ff;
        fill: #ffffff;
    }

</style>