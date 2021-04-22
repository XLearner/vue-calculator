<template>
  <div class="calculator-wrap clear" @click="clickCell">
    <ul class="symbol-box fl">
      <li>+</li>
      <li>-</li>
      <li>%</li>
      <li>*</li>
      <li>/</li>
      <li>^</li>
      <li>(</li>
      <li>)</li>
      <li>.</li>
    </ul>
    <ul class="num-box fr">
      <li>1</li>
      <li>2</li>
      <li>3</li>
      <li>4</li>
      <li>5</li>
      <li>6</li>
      <li>7</li>
      <li>8</li>
      <li>9</li>
      <li>0</li>
      <li class="backspace"><i>backspace</i></li>
    </ul>
  </div>
</template>

<script>
  export default {
    name: "calculator",
    data() {
      return {

      }
    },
    methods: {

      /** 
       * 外部监听 on-select 即可
       */
      clickCell(e) {
        let word = void 0; 
        // 兼容IE
        if (navigator.userAgent.includes('MSIE') || navigator.userAgent.includes('Windows NT')) {
          if (e.target.innerText.length != 1) {
            return ;
          }
          word = e.target.innerText;
        } else {
          if (e.toElement.textContent.length != 1) {
            return ;
          }
          word = e.toElement.textContent;
        }
        this.$emit('on-select', word);

      }
    }
  }
</script>

<style scoped lang="less">
  @deep: ~">>>";
  
  ul, li {
    list-style: none;
  }
  .calculator-wrap{
    width: 300px;
    height: 165px;
    font-size: 0;
    line-height: 0;
    border: 1px solid #eee;
    border-radius: 5px;
    box-shadow: 0 0 5px #eee;
    overflow: hidden;
    margin: 0 auto;
    -webkit-touch-callout: none; /* iOS Safari */
      -webkit-user-select: none; /* Chrome/Safari/Opera */
      -khtml-user-select: none; /* Konqueror */
        -moz-user-select: none; /* Firefox */
          -ms-user-select: none; /* Internet Explorer/Edge */
              user-select: none;

    .symbol-box{
      width: 50%;
      background: #75abdc;
      
      li {
        width: 33.2%;
        height: 55px;
        line-height: 55px;
        color: #fff;
        border-left: .1px solid rgba(220,220,220,1);
        border-bottom: .1px solid rgba(220,220,220,1);
      }
    }

    .num-box {
      width: 49.5%;

      li {
        width: 33.2%;
        border-left: .5px dashed rgba(200,200,200,1);
        border-bottom: .5px dashed rgba(200,200,200,1);
        height: 41px;
        &:nth-child(3n-2) {
          border-left: 0;
        }
      }
      .backspace {
        width: 66.6%;
        i {
          display: block;
          background: url('../assets/images/backspace.png') no-repeat center center;
          height: 100%;
          background-size: 22px;
          font-size: 0;
        }
      }
    }

    li {
      display: inline-block;
      font-size: 14px;
      width: 25%;
      height: 40px;
      line-height: 40px;
      text-align: center;
      vertical-align: middle;
      cursor: pointer;
      transition: all .5s;
      &:hover {
        box-shadow: 0 0 5px #eee;
      }
      &:active {
        box-shadow: 0 0 5px #999;
      }
    }

    &:after {
      content: "";
      display: block;
      clear: both;
    }
  }

  .symbol-box {
    padding: 0;
  }
  .num-box {
    padding: 0;
  }
  
</style>