<template>
    <div>
        <!--放一个checkbox，如果变化了，同时是否沟选用thecheck记录，同时也做双击关联-->
        <input type="checkbox" :checked="thecheck" v-model="thecheck"/>
        <span>已经勾选：{{selectCount}}</span> | <span>全部：{{allCount}}</span>
        <!--放一个删除按钮，当点击时调用deleteall-->
        <button @click="deleteAll">删除</button>

    </div>
</template>

<script>
    import pubsub from "pubsub-js"
    export default {
        name: "demoFooter",

        props:["allredords"],                 //接收传参来的参数，可能是对象可能是事件
        computed:{
            selectCount(){                                                  //声明动态元素选择的数
                return this.allredords.filter(Item=>Item.done===true).length//取一下Done为true的总数
            },
            allCount(){                                                     //声明一个元素为总数
                return this.allredords.length                               //取总数
            },
            thecheck(){                                                     //声明一个是否
                return this.selectCount===this.allCount                     //这里是否沟选的符号位，同时也是双向关联
            }
        },
        methods:{
            deleteAll(){                                    //如果是删除事件
                pubsub.publish("deleteAppAll")                        //调用总线的函数来的删除全部
            },
            checkAll(){                                     //如果是全选事件
                pubsub.publish("selectappAll",this.thecheck==true)      //调用总线的函数的全选参数
            }
        }
    }

</script>

<style scoped>

</style>