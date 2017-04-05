<template>
    <div class="header">
      <div class="content-wrapper">
        <div class="avatar">
          <img width="64" height="64" v-bind:src="seller.avatar" alt="商家头像">
        </div>
        <div class="content">
          <div class="title">
            <span class="brand"></span>
            <span class="name">{{seller.name}}</span>
          </div>
          <div class="description">
            {{seller.description}}/{{seller.deliveryTime}}分钟送达
          </div>
          <div v-if="seller.supports" class="support">
            <span class="icon" :class="classMap[seller.supports[0].type]"></span>
            <span class="text">{{seller.supports[0].description}}</span>
          </div>
        </div>
        <div v-if="seller.supports" class="support-count" @click="showDetail">
          <span class="count">{{seller.supports.length}}个</span>
          <i class="icon-key_arrow_right">&gt;</i>
        </div>
      </div>
      <div class="bulletin-wrapper" @click="showDetail">
        <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
        <i class="icon-key_arrow_right">&gt;</i>
      </div>
      <div class="background">
        <img width="100%" height="100%" v-bind:src="seller.avatar" alt="背景图片">
      </div>
      <transition name="fade">
        <div class="detail" v-show="detailShow">
        <!--sticky footer布局-->
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul v-if="seller.supports" class="supports">
              <li class="support-item" v-for="(item,index) in seller.supports">
                <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                <span class="text">{{seller.supports[index].description}}</span>
              </li>
            </ul>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              <p class="content">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="hideDetail">
          <i class="icon-close"></i>
        </div>
        <!--sticky footer布局结束-->
      </div>
      </transition>
    </div>
</template>
<script type="text/ecmascript-6">
  import star from '../star/star';

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        detailShow: false
      };
    },
    methods: {
      showDetail() {
        this.detailShow = true;
      },
      hideDetail() {
        this.detailShow = false;
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    components: {
      star
    }
  };
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";
  .header
    position: relative
    background-color: rgba(7,17,27,.3)
    color: #fff
    overflow: hidden
    .content-wrapper
      position: relative
      padding:24px 12px 18px 24px
      font-size:0
      .avatar
        display:inline-block
        vertical-align top
        img
          -webkit-border-radius: 2px
          -moz-border-radius: 2px
          border-radius: 2px
      .content
        display:inline-block
        font-size: 14px
        margin-left: 16px
        .title
          margin:2px 0 8px 0
          .brand
            display: inline-block
            vertical-align:top
            width: 30px
            height: 18px
            bg-img('brand')
            background-size:30px 18px
            background-repeat: no-repeat
          .name
            margin-left: 6px
            font-size: 16px
            line-height: 18px
            font-weight:bold
        .description
          margin-bottom: 10px
          line-height: 12px
          font-size: 12px
        .support
          .icon
            display: inline-block
            width: 12px
            height: 12px
            margin-right: 4px
            background-size:12px 12px
            background-repeat:no-repeat
            &.decrease
              bg-img('decrease_1')
            &.discount
              bg-img('discount_1')
            &.guarantee
              bg-img('guarantee_1')
            &.invoice
              bg-img('invoice_1')
            &.sprcial
              bg-img('special_1')
          .text
            line-height: 12px
            font-size: 12px
            vertical-align top
      .support-count
        position: absolute
        right: 12px
        bottom: 14px
        padding:0 8px
        height: 24px
        line-height: 24px
        border-radius: 14px
        background-color: rgba(0,0,0,.2)
        text-align: center
        .count
          font-size: 10px
        .icon-key_arrow_right
          margin-left: 6px
          font:200 12px/24px simsun

    .bulletin-wrapper
      position: relative
      height: 28px
      line-height: 28px
      padding:0 22px 0 12px
      white-space:nowrap
      overflow:hidden
      -ms-text-overflow: ellipsis
      text-overflow: ellipsis
      background-color:rgba(7,17,27,.2)
      .bulletin-title
        display: inline-block
        width: 22px
        height: 12px
        bg-img('bulletin')
        background-size:22px 12px
        background-repeat:no-repeat
      .bulletin-text
        font-size: 12px
        margin:0 4px
        vertical-align: top
      .icon-key_arrow_right
        position: absolute
        right: 12px
        top: 0
        font:200 12px/28px simsun

    .background
      position: absolute
      z-index: -1
      width: 100%
      height: 100%
      top:0
      left:0
      filter:blur(10px)
    .detail
      position: fixed
      z-index:100
      width:100%
      height:100%
      top:0
      left: 0
      overflow: auto
      /*transition: all 0.5s*/
      opacity: 1
      background-color: rgba(7,17,27,.8)
      -webkit-backdrop-filter blur(10px)
      &.fade-enter-active,&.fade-leave-active
        transition: all 0.5s
      &.fade-enter, &.fade-leave-to
        opacity: 0
        background-color: rgba(7,17,27,0)
      .detail-wrapper
        min-height:100%
        width:100%
        .detail-main
          margin-top: 64px
          padding-bottom: 64px
          .name
            line-height: 16px
            text-align: center
            font-size: 16px
            font-weight:700
          .star-wrapper
            margin-top: 18px
            padding: 2px 0
            text-align: center
          .title
            display: flex
            width: 80%
            margin: 28px auto 24px auto
            .line
              flex: 1
              position: relative
              top: -6px
              border-bottom: 1px solid rgba(255,255,255,.2)
            .text
              padding: 0 12px
              font-weight: 700
              font-size: 14px
          .supports
            width:80%
            margin:0 auto
            .support-item
              padding:0 12px
              margin-bottom: 12px
              font-size:0
              &:last-child
                margin-bottom:0
              .icon
                display: inline-block
                width: 16px
                height: 16px
                vertical-align: top
                margin-right: 6px
                background-size: 16px 16px
                background-repeat: no-repeat
                &.decrease
                  bg-img('decrease_2')
                &.discount
                  bg-img('discount_2')
                &.guarantee
                  bg-img('guarantee_2')
                &.invoice
                  bg-img('invoice_2')
                &.sprcial
                  bg-img('special_2')
              .text
                line-height: 16px
                font-size: 12px
          .bulletin
            width: 80%
            margin: 0 auto
            .content
              padding: 0 12px
              line-height: 24px
              font-size: 12px
  .detail-close
        position: relative
        width: 32px
        height: 32px
        margin: -64px auto 0 auto
        clear: both
        font-size: 32px
</style>
