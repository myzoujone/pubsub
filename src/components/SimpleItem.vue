<template>
    <div>

        <li style="text-align: left;    padding: 10px;list-style: none;">
            <!--在LI里面的前面加一个输入框，绑定DONE的值决定是否沟选，同时发生变化触发事件change1-->
            <input type="checkbox" :checked="p1.done" @change="change1(p1.id)">
            <!--显示我要显示的内容 -->
            {{p1.itemStr}} - {{p1.itemTitle}}
            <!--增加一个按钮，在CLASS里是隐藏的，如果点击调用deleteit事件-->
            <button class="delclass" @click="deleteit(p1.id)" style="float:right;">删除</button>
        </li>
    </div>
</template>

<script>
    import pubsub from "pubsub-js"
    export default {
        name: "SimpleItem",
        props:["p1"],            //调用三个参数，第一个是一个OBJECT的实例，第二是沟选变化，第三是删除一个
        methods:{
            change1(id){                                    //如果你沟选了，
                pubsub.publish("change1",id)                             //就去让OBJ的DONE取反，这和CHECKBOX会同步
            },
            deleteit(id){                                   //如果点击了删除
                pubsub.publish("deleteOneRecord",id)                    //会通过传参来的deleteonerecord去删除一条记录
            }
        }
    }
</script>

<style scoped>
    .delclass{

        display: none;
        background-color: red;
        color:white;
    }
    li:hover{
        background-color: #cccccc;
    }
    li:hover button{
        display: inline-block;
    }
</style>