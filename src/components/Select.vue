<template>
  <div class="container">
    <Selector 
    type="single"
    :rightItemList="rightItemList"
    :leftItemList="leftItemList" />
  </div>
</template>

<script>
  import Selector from '../common/Selector';
  export default {
    data () {
      return {
        leftItemList: [{
          name: '题目1',
          answer: []
        },{
          name: '题目2',
          answer: []
        },{
          name: '题目3',
          answer: []
        },{
          name: '题目4',
          answer: []
        },{
          name: '题目5',
          answer: []
        },{
          name: '题目6',
          answer: []
        },{
          name: '题目7',
          answer: []
        },{
          name: '题目8',
          answer: []
        },{
          name: '题目9',
          answer: []
        }],
        rightItemList: [{
          name: 1,
          isSelect: false
        },{
          name: 2,
          isSelect: false
        },{
          name: 3,
          isSelect: false
        },{
          name: 4,
          isSelect: false
        },{
          name: 5,
          isSelect: false
        },{
          name: 6,
          isSelect: false
        },{
          name: 7,
          isSelect: false
        },{
          name: 8,
          isSelect: false
        },{
          name: 9,
          isSelect: false
        },{
          name: 10,
          isSelect: false
        }],
        actAnswer: '',
        currentTopic: 0
      }
    },
    computed: {
      moveBoxLength(){
        let len = this.currentTopic*124
        return `translateY(${len}px)`
      }
    },
    components: {
      'Selector': Selector
    },
    methods : {
      answerTap(index){
        this.actAnswer = index
        this.leftItemList[this.leftItemList.length - this.currentTopic - 1].answer = this.rightItemList[index]
        if(this.currentTopic >= this.leftItemList.length-1){
          this.$alert(this.leftItemList, '答题结果', {
            confirmButtonText: '确定',
            callback: action => {
              this.$message({
                type: 'info',
                message: `action: ${ action }`
              });
            }
          });
          console.log(this.leftItemList)
          return
        }
        this.currentTopic++
        setTimeout(()=>{
          this.actAnswer = ''
        },300)
      }
    },
    mounted(){
      
    }
  }
</script>

<style lang="less" scoped>
  .container{
    display: flex;
    width: 100%;
    height: 100%;
    align-items: center;
    justify-content: center;
    .leftBox{
      padding: 20px 40px;
      width: 140px;
      height: 600px;
      overflow: hidden;
      position: relative;
      .moveBox{
        width: 100%;
        height: auto;
        position: absolute;
        left: 40px;
        bottom: 250px;
        transition: .3s;
        .leftItem{
          width: 100px;
          height: 100px;
          box-shadow: 2px 2px 4px rgba(0,0,0,.1);
          border: 2px solid rgba(0,0,0,.1);
          border-radius: 8px;
          text-align: center;
          font-weight: 600;
          margin-bottom: 20px;
          cursor: pointer;
          position: relative;
          .topicDesc{
            line-height: 100px;
            margin: 0;
          }
          .rightIcon{
            width: 30px;
            height: 30px;
            border-radius: 20px;
            box-sizing: border-box;
            padding: 8px;
            background-color: #999;
            position: absolute;
            right: -10px;
            top: -10px;
          }
        }
      }
      .arrow{
        width: 0;
        height: 0;
        border-width: 10px 0 10px 20px;
        border-color: transparent transparent transparent #999;
        border-style: solid;
        position: absolute;
        left: 25px;
        top: 300px;
      }
    }
    .rightBox{
      margin-left: 60px;
      .rightItem{
        width: 50px;
        height: 50px;
        border-radius: 7px;
        border: 1px solid rgba(0,0,0,.05);
        box-shadow: 0 3px 6px rgba(0,0,0,.1);
        margin-bottom: 10px;
        text-align: center;
        line-height: 50px;
        color: #333;
        font-weight: 600;
        cursor: pointer;
        background: linear-gradient(to top, rgba(0,0,0,.15), rgba(0,0,0,.09));
        transition: .2s;
      }
      .actrightItem{
        color: #fff;
        background-color: #8C8F95;
      }
    }
  }
</style>
