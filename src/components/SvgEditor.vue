<template>
    <div style="width: 100%;height: 100%">
        <h1>{{ msg }}</h1>
        <!--工具栏-->
        <button @click="save">save</button>
        <div style="width: 200px;height: 100%;display: inline-block;float: left">
            <ul>

                <li v-for="(item,index) in toolList" :key="index" draggable="true" @dragstart="startDragTool($event,item)" >
                    {{item.name}}
                </li>
            </ul>
        </div>
        <!--画板-->
        <div style="width: 800px;height:600px;border: 1px solid;display: inline-block;float: left;">
            <svg style="width: 100%;height: 100%" @dragover.prevent @drop.prevent="dropNode"
                 @contextmenu.prevent>
                <BaseNode v-for="(item,index) in nodeList" :nodeData="item" :key="'node'+index" ></BaseNode>


            </svg>
        </div>

    </div>
</template>

<script>
    import BaseNode from './BaseNode'
    export default {
        name: 'SvgEditor',
        components: {BaseNode},
        props: {
            msg: String
        },

        setup(){

        },

        data() {
            return {
                toolList: [
                    {
                        type: 'rect',
                        name: '方形'
                    },
                    {
                        type: 'circle',
                        name: '圆形'
                    }
                ],
                nodeList:[
                    {
                        nodeType: 'rect',
                        name: '测试',
                        data: {
                            x:50,
                            y:100
                        }

                    }
                ]

            }
        },

        methods: {
            startDragTool(e,i){
                console.log("拖放事件开始：",e,i);
                //通过dataTransfer在事件中传递数据，拖起时存入，放置时取出
                e.dataTransfer.setData("text",JSON.stringify(i));
            },
            dropNode(e){
                console.log("放置事件",e)
                let tool = JSON.parse(e.dataTransfer.getData("text"));
                console.log("放置事件dataTransfer获取数据",tool)
                //工具类型
                let type = tool.type
                //放置位置
                let nodeX = e.layerX
                let nodeY = e.layerY

                if (type == 'rect'){
                    this.nodeList.push({'nodeType':type,'data':{'x':nodeX,'y':nodeY}})
                }

            },

            save(){
                console.log('ss'+JSON.stringify(this.nodeList))
            }



        }

    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h3 {
        margin: 40px 0 0;
    }

    ul {
        min-height: 500px;
        border: 1px ridge;
        list-style-type: none;
        padding: 0;
        margin: 0px;
    }

    li {
        height: 20px;
        user-select: none;
        border: double;
        display: block;
        margin: 5px 10px;
    }

    a {
        color: #42b983;
    }
</style>
