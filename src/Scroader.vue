<template>
  <div class="c-scroader" :class="{ 'is-fixed' : isHeaderFixed }" :style="{ height: headerHeight + 'px' }">
    <div class="c-scroader__header"
    v-show="isHeaderVisible"
    :transition="headerTransition"
    v-el:header>
      <slot></slot>
    </div>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        headerHeight: 0,
        topOffset: 0,
        lastTopOffset: 0,
        isScrollingTop: false,
        isHeaderFixed: false
      }
    },
    props: [
      {
        name: 'headerOffset', // space between top and header
        type: Number,
        default: 0
      }, {
        name: 'scroaderOffset', // space between header and the out offset zone
        type: Number,
        default: 200
      }, {
        name: 'headerTransition',
        type: String,
        default: 'scroader'
      }
    ],
    computed: {
      isHeaderInOffset () {
        if (this.topOffset > (this.headerOffset + this.scroaderOffset)) {
          // update isHeaderFixed
          this.isHeaderFixed = true
          return false
        }
        return true
      },
      isHeaderVisible () {
        if (!this.isHeaderInOffset && !this.isScrollingTop) {
          return false
        }
        return true
      }
    },
    methods: {
      scrollHandler () {
        // update topOffset
        this.topOffset = document.body.scrollTop

        // update isScrollingTop
        if (this.topOffset > this.lastTopOffset) {
          this.isScrollingTop = false
        } else {
          this.isScrollingTop = true
        }
        this.lastTopOffset = this.topOffset

        // update isHeaderFixed
        if (this.topOffset <= (0 + this.headerOffset)) {
          this.isHeaderFixed = false
        }
      },
      windowResizeHandler () {
        // update header height
        this.headerHeight = this.$els.header.offsetHeight
      }
    },
    ready () {
      // listener windows width
      window.addEventListener('resize', this.windowResizeHandler)
      // listener scroll
      window.addEventListener('scroll', this.scrollHandler)

      this.scrollHandler()
      this.windowResizeHandler()
    },
    beforeDestroy () {
      // remove listener windows width
      window.removeEventListener('resize', this.windowResizeHandler)
      // remove listener scroll
      window.removeEventListener('scroll', this.scrollHandler)
    }
  }
</script>

<style lang="scss">
  @import '~sass-bem-constructor/dist/bem-constructor';


  //  Component
  //  --------------------------------------------------------------------------

  @include component('scroader') {
    display: block;
    width: 100%;

    @include element('header') {
      display: block;
      width: 100%;
    }

    @include state('fixed') {
      @include modifies-element('header') {
        left: 0;
        position: fixed;
        top: 0;
      }
    }
  }


  //  Transition
  //  --------------------------------------------------------------------------

  .scroader {
    &-transition {
      transition: transform .3s, opacity .3s;
    }

    &-enter,
    &-leave {
      transform: translate3d(0, -100%, 0);
      opacity: 0;
    }

    &-leave {
      transition: none;
    }
  }
</style>
