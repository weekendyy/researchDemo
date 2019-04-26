<template>
  <div class="selectorBox">
    <!-- 单选 -->
    <template v-if="type === 'single'">
      <div class="leftBox">
        <div class="moveBox" :style="{transform: moveBoxLength}">
          <div 
            class="leftItem" 
            @click="reform(index)"
            :style="{opacity: currentTopic === leftItemList.length-index-1?1:1-Math.abs(currentTopic-leftItemList.length+index+1)*0.4}"
            v-for="(item, index) in leftItemList">
           <p class="topicDesc">{{item.name}}</p>
           <img :class="{showIcon: leftItemList[index].answer.length !== 0}" src="../assets/right.png" class="rightIcon" alt="">
          </div>
        </div>
        <div class="arrow">
        </div>
      </div>
      <div class="rightBox">
        <div class="rightItem" 
            @click="answerTap(index)"
            :class="{actrightItem: index === actAnswer}" 
            v-for="(item, index) in rightItemList">
          {{item.name}}
        </div>
      </div>
    </template>
    <!-- 多选 -->
    <template v-if="type === 'multiple'">
      <div class="leftBox" >
        <div class="moveBox" :style="{transform: moveBoxLength }">
          <div 
            class="leftItem" 
            @click="reform(index)"
            :style="{opacity: currentTopic === leftItemList.length-index-1?1:1-Math.abs(currentTopic-leftItemList.length+index+1)*0.4}"
            v-for="(item, index) in leftItemList">
           <p class="topicDesc">{{item.name}}</p>
           <img :class="{showIcon: leftItemList[index].answer.length !== 0}" src="../assets/right.png" class="rightIcon" alt="">
          </div>
        </div>
        <div class="arrow">
        </div>
      </div>
      <div class="rightBox">
        <div class="rightItem" 
            @click="answerTapSec(index)"
            :class="{actrightItem: item.isSelect}" 
            v-for="(item, index) in rightItemList">
          {{item.name}}
        </div>
      </div>
      <el-button class="nextBtn" @click="nextBtnTab" type="primary" >下一题</el-button>
    </template>
  </div>
</template>

<script>
  export default {
    props: {
      leftItemList: Array,
      rightItemList: Array,
      type: String    //单选--single   多选--multiple
    },
    data () {
      return {
        actAnswer: null,
        currentTopic: 0,
        isMoving: false
      }
    },
    computed: {
      moveBoxLength(){
        let len = this.currentTopic*124
        return `translateY(${len}px)`
      }
    },
    methods : {
      // 判断是否有选择
      _hasSelect(){
        let result = false
        for(let item of this.rightItemList){
          if(item.isSelect){
            result = true
          }
        }
        return result
      },
      // 清除所有答案
      _clearRightItem(){
        this.actAnswer = null
        for(let item of this.rightItemList){
          item.isSelect = false
        }
      },
      // 把存储的答案赋值给右边选项
      _assign(index){
        for(let leftItem of this.leftItemList[index].answer){
          for(let rightkey in this.rightItemList){
            if(leftItem === this.rightItemList[rightkey].name){
              this.rightItemList[rightkey].isSelect = true
              console.log(rightkey)
              this.actAnswer = Number(rightkey)
            }
          }
        }
      },
      answerTap(index){
        this.actAnswer = index
        this.leftItemList[this.leftItemList.length - this.currentTopic - 1].answer.length = 0
        this.leftItemList[this.leftItemList.length - this.currentTopic - 1].answer.push(this.rightItemList[index].name)
        if(this.currentTopic >= this.leftItemList.length-1){
          this.$alert(this.leftItemList, '答题结果', {
            confirmButtonText: '确定',
          });
          return
        }
        this.currentTopic++
        setTimeout(()=>{
          this._clearRightItem()
          let index = this.leftItemList.length-this.currentTopic-1
          this._assign(index)
        },300)
      },
      // 重做
      reform(index){
        this._clearRightItem()
        this.actAnswer = null
        this.currentTopic = this.leftItemList.length-index-1
        console.log(index)
        console.log(this.leftItemList[index])
        this._assign(index)
      },
      answerTapSec(index){
        this.rightItemList[index].isSelect = !this.rightItemList[index].isSelect
      },
      nextBtnTab(){
        if(!this._hasSelect()){
          this.$message({
            message: '请至少选择一个答案',
            type: 'warning'
          });
          return
        }
        for(let item of this.rightItemList){
          if(item.isSelect){
            this.leftItemList[this.leftItemList.length - this.currentTopic - 1].answer.push(item.name)
          }
        }
        if(this.currentTopic >= this.leftItemList.length-1){
          this.$alert(this.leftItemList, '答题结果', {
            confirmButtonText: '确定',
          });
          return
        }
        this.currentTopic++
        setTimeout(()=>{
          this._clearRightItem()
          let index = this.leftItemList.length-this.currentTopic-1
          this._assign(index)
        },300)
      },
      // 拖动
      // move(e){
      //   let odiv = e.target;        //获取目标元素

      //   //算出鼠标相对元素的位置
      //   let disX = e.clientX - odiv.offsetLeft;
      //   let disY = e.clientY - odiv.offsetTop;
      //   document.onmousemove = (e)=>{       //鼠标按下并移动的事件
      //       //用鼠标的位置减去鼠标相对元素的位置，得到元素的位置
      //       let left = e.clientX - disX;    
      //       let top = e.clientY - disY;
      //       //移动当前元素
      //       // odiv.style.left = left + 'px';
      //       odiv.style.top = top + 'px';
      //       console.log(e)
      //   };
      //   document.onmouseup = (e) => {
      //     document.onmousemove = null;
      //     document.onmouseup = null;
      //   };
      // }
    },
    
    mounted(){
      this.leftItemList.reverse()
    }
  }
</script>

<style lang="less" scoped>
  .selectorBox{
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
        bottom: 260px;
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
            opacity: 0;
            transform: scale(0);
            transition: .3s .2s;
          }
          .showIcon{
            opacity: 1;
            transform: scale(1);
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
      margin-left: 20px;
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
    .nextBtn{
      margin-left: 30px;
    }
  }
</style>
