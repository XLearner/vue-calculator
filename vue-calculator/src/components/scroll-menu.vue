
<template>
  <div class="top-scroll clear">
    <div class="btn-con left-btn fl">
      <Button type="text" @click="handleScroll(120)">
        <!-- <Icon :size="18" type="ios-arrow-back"/> -->
        <i class="arrow-btn arrow-btn-left"></i>
      </Button>
    </div>
    <div ref="scrollOuter" @DOMMouseScroll="handlescroll" @mousewheel="handlescroll" class="tags-outer-scroll-con fl">
      <div ref="scrollBody" class="tags-inner-scroll-body" :style="{left: tagBodyLeft + 'px'}">
        <transition-group name="taglist-moving-animation">
          <Tag
            v-for="(item,index) in pageTagsList"
            ref="tagsPageOpened"
            :key="index"
            :class="{on: index==currentIndex}"
            :name="item.title"
            @click.native="selectItem(item, index)"
            @contextmenu.prevent.native="contextMenu(item, $event)"
            color="default"
          >{{ item.title }}</Tag>
        </transition-group>
      </div>
    </div>
    <div class="btn-con right-btn fr">
      <Button type="text" @click="handleScroll(-120)">
        <!-- <Icon :size="18" type="ios-arrow-forward"/> -->
        <i class="arrow-btn arrow-btn-right"></i>
      </Button>
    </div>
  </div>
</template>

<script>
  export default {
    name: "scrollMenu",
    props: {
      // 标签列表
      values: {
        type: Array,
        required: true,
        default: () => []
      }
    },
    computed: {
      // 标签列表
      pageTagsList() {
        return this.values;
      }
    },
    data() {
      return {
        currentIndex: 0,
        tagBodyLeft: 0,
        contextMenuLeft: 0,
        contextMenuTop: 0,
      }
    },
    watch: {
      'currentIndex'(val) {
        this.$emit('on-change', val || 0);
      }
    },
    methods: {

      // 右键事件
      contextMenu(item, e) {
        if (item.meta.name === 'Home') {
          return;
        }
        this.contextTag = item;
        this.visible = true;
        const offsetLeft = this.$el.getBoundingClientRect().left;
        this.contextMenuLeft = e.clientX - offsetLeft + 10;
        this.contextMenuTop = e.clientY - 90;
      },
      /**
       * 处理滚动方法
       * @param  {[type]} e [description]
       * @return {[type]}   [description]
       */
      handlescroll(e) {
        let type = e.type;
        let delta = 0;
        if (type === 'DOMMouseScroll' || type === 'mousewheel') {
          delta = e.wheelDelta ? e.wheelDelta : -(e.detail || 0) * 40;
        }
        this.handleScroll(delta);
      },
      handleScroll(offset) {
        const outerWidth = this.$refs.scrollOuter.offsetWidth;
        const bodyWidth = this.$refs.scrollBody.offsetWidth;
        if (offset > 0) {
          this.tagBodyLeft = Math.min(0, this.tagBodyLeft + offset);
        } else {
          if (outerWidth < bodyWidth) {
            if (this.tagBodyLeft < -(bodyWidth - outerWidth)) {
              this.tagBodyLeft = this.tagBodyLeft;
            } else {
              this.tagBodyLeft = Math.max(
                this.tagBodyLeft + offset,
                outerWidth - bodyWidth
              );
            }
          } else {
            this.tagBodyLeft = 0;
          }
        }
      },

      /**
       * 选择单个标签
       */
      selectItem(value, selectIndex) {
        this.$emit('on-select', value, selectIndex);
        this.currentIndex = selectIndex;
      }
    }

  }
</script>

<style scoped lang="less">
  @deep: ~">>>";
  .top-scroll {
    height: 42px;
    box-shadow: 0 0 5px #ddd;

    .sbox{
      white-space: nowrap;
      overflow-x: auto;
      width: 100%;
    }
    li{
      float: left;
      line-height: 18px;
      padding: 16px 8px;
      margin-right: 8px;
    }
  }
  .top-scroll{
    .btn-con{
      width: 24px;
      button{
        padding: 0;
        height: 28px;
        line-height: 28px;
        box-shadow: 0 0 5px #ddd;
      }
      &.right-btn{
        text-align: right;
      }
    }
    .tags-outer-scroll-con{
      width: calc(100% - 48px);
      height: 28px;
      position: relative;
      overflow: hidden;
      .tags-inner-scroll-body{
        position: absolute;
        left: 0;
        top: 0;
        white-space: nowrap;
        width: auto;
        transition: all .8s;
        @{deep} .ivu-tag {
          height: 28px;
          line-height: 28px;
          padding: 0 10px;
          background-color: transparent;
          border: 0;
          cursor: pointer;
          background-image: inherit;
          background-repeat: repeat-y;
          background-size: contain;
          border-radius: 16px;
          margin: 0;
          transition: background-image 3s;
          .ivu-tag-text{
            color: #003e75;
          }
          &.on{
            background-image: url(../../images/risklimit/selected_bg.png);
            .ivu-tag-text{
              color: #fff;
            }
          }
        }
      }
    }
    // .ivu-btn-text:focus{
    //   box-shadow: none;
    // }
  }

  .arrow-btn{
    padding: 8px;
    background-image: url(../../images/labelmanage/down_arrow_icon.png);
    background-repeat: no-repeat;
    background-position: center;
    background-size: 20px;
    display: inline-block;
    vertical-align: middle;
  }
  .arrow-btn-left {
    transform: rotate(90deg);
  }
  .arrow-btn-right {
    transform: rotate(-90deg);
  }
</style>