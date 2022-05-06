<template>
  <div id="app">

    <!--使用Header组件。-->
    <DemoHeader  :recordN="ItemLists.length"></DemoHeader>
    <!--中间这里放了一个UL的列表，将数组ppp送出去了，-->
    <ItemList :ppp="ItemLists"></ItemList>
    <!--再声明了一下底部，-->
    <demoFooter  :allredords="ItemLists"></demoFooter>


  </div>
</template>

<script>


import demoFooter from "@/components/demoFooter";
import DemoHeader from "@/components/DemoHeader";
import ItemList from "@/components/ItemList";
import pubsub from "pubsub-js"
export default {
  name: 'App',
  data(){
    return {
      tempStr:"aaaa",
      ItemLists:[
        {id:1,itemStr:"aaaa1",itemTitle:"aaaaTitle1",done:true},
        {id:2,itemStr:"aaaa2",itemTitle:"aaaaTitle2",done:true},
        {id:3,itemStr:"aaaa3",itemTitle:"aaaaTitle3",done:false},
        {id:4,itemStr:"aaaa4",itemTitle:"aaaaTitle4",done:true},
      ]
    }
  },

  components:{demoFooter,DemoHeader,ItemList},                                    //这里是声明了三个组件

  mounted() {                                                                     //当加载完成之后
            this.recieveID=pubsub.subscribe("recieve",(funcName,data)=>{          //给总线加上接收
              console.log(data)
              this.ItemLists.unshift(data)                                        //如果接收到了OBJ的对象，加入数组
            }),
            this.changeID=pubsub.subscribe("change1",(funcName,data)=>{           //给总线加上变化
              this.ItemLists.forEach((obj)=>{                                     //循环全部数组
                if (obj.id==data){                                                //如果某一个的ID等于传值的ID
                  obj.done=!obj.done                                              //勾选的标识取反
                }
              })
            }),
            this.deleteOneRecordID=pubsub.subscribe("deleteOneRecord",(funcName,data)=>{    //给消息订阅上删除一条记录
              //数组删除，删除的数据开始位是ID出现的那个的索引的位置，取一位删除。
              this.ItemLists.splice(this.ItemLists.findIndex(item => item.id === data), 1)
            }),
            this.selectappAllID=pubsub.subscribe("selectappAll",(funcName,data)=>{          //给消息订阅上加上选择全部
              this.ItemLists.forEach((obj)=>{obj.done=data})                                //循环将全部标识更换
            }),
            this.deleteAppAllID=pubsub.subscribe("deleteAppAll",()=>{                       //给消息订阅上加上删除全部
              this.ItemLists=this.ItemLists.filter(item => item.done === false)             //将数组等于数组过滤之后的，过滤内容是done等于假
            })

  },
  beforeDestroy() {                                                                         //在消毁之前的消息订阅
    pubsub.unsubscribe(this.recieveID)                                                      //在消毁之前的消息订阅
    pubsub.unsubscribe(this.changeID)                                                       //在消毁之前的消息订阅
    pubsub.unsubscribe(this.deleteOneRecordID)                                              //在消毁之前的消息订阅
    pubsub.unsubscribe(this.selectappAllID)                                                 //在消毁之前的消息订阅
    pubsub.unsubscribe(this.deleteAppAllID)                                                 //在消毁之前的消息订阅
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
