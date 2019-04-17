<template>
  <div class="container">
    <div class="rotateBox">
      <template v-for="(item,index) in brandList" >
        <div class="rotateItem" v-bind:key="index" :style="`transform: rotate(${index*angle}deg)`">
          <div @click="handleBrand({index})" 
          :class="{'actrotateTxt':index===actIdx}"
          class="rotateTxt" :style="`transform: rotate(${-index*angle}deg)`">{{item}}</div>
        </div>
      </template>
    </div>
    <div class="addBox">
      <input type="text" v-model="inpVal" name="" id="">
      <div class="confirm" @click="addBrand">添加</div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data () {
      return {
        brandList: ['安踏','李宁','班尼路','优衣库','HM','阿依莲'],
        actIdx: '',
        inpVal: ''
      }
    },
    methods : {
      handleBrand({index}){
        this.actIdx = index
      },
      addBrand(){
        if(!!this.inpVal){
          this.brandList.push(this.inpVal)
        }
      }
    },
    computed: {
      angle(){
        return 360/this.brandList.length
      }
    }
  }
</script>

<style lang="less" scoped>
.container{
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  .rotateBox{
    width: 200px;
    height: 200px;
    position: relative;
    .rotateItem{
      width: 100px;
      height: 250px;
      position: absolute;
      top: 0;
      left: 0;
      box-sizing: border-box;
      transform-origin: bottom;
      .rotateTxt{
        width: 100px;
        height: 100px;
        border: 1px solid #C6CBD2;
        border-radius: 6px;
        color: #888;
        line-height: 100px;
        text-align: center;
        position: absolute;
        top: 0;
        left: 0;
        box-sizing: border-box;
        transform-origin: center;
        transition: .2s;
        &:hover{
          background : #879;
          cursor: pointer;
          color: #fff;
          box-shadow: 0 0 20px rgba(0,0,0,.2);
        }
      }
      .actrotateTxt{
        background : #879;
        cursor: pointer;
        color: #fff;
        box-shadow: 0 0 20px rgba(0,0,0,.2);
      }
    }
  }
  .addBox{
    width: 200px;
    height: 60px;
    position: absolute;
    top: 90px;
    right: 50px;
    display: flex;
    align-items: center;
    input{
      height: 20px;
    }
    .confirm{
      width: 70px;
      height: 30px;
      color: #fff;
      background: #879;
      border-radius: 5px;
      text-align: center;
      line-height: 30px;
      margin-left: 10px;
      cursor: pointer;
    }
  }
}
</style>
