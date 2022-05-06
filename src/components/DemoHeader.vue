<template>
    <div>
        <!--这里有一个输入框，同时有一个双向关联的参数-->
        <input type="text" v-model="thejob" class="inputa"/>
        <!--点击事件-->
        <button @click="addit">新建</button><br />
    </div>
</template>

<script>
    import pubsub from "pubsub-js"
    export default {
        name: "DemoHeader",
        props:["recordN"],        //接收传参来的接收和总数的二个参数
        data(){
            return{
                thejob:""                   //定义了一个输入的内容的双向关联的东西
            }
        },
        methods:{
            addit(){
                //如果点击了，我们增加一条记录，这条记录的ID是总数加1，不考虑删除了会ID重复的事
                var aobj={id:this.recordN+1,itemStr:this.thejob,itemTitle:this.thejob+"Title",done:true}
                pubsub.publish("recieve",aobj)    //这里是调用了前面自定义的函数IgetIt约定的办法。传参是AOBJ
                this.thejob=""              //清除输入框
            }
        }
    }
</script>

<style scoped>
    .inputa{
        border: 1px solid #cccccc;
        border-radius: 10px;
        padding: 5px;
    }
</style>