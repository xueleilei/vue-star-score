<template>
  <div>
    <div class="star-box" :class="{'disabled':disabled}">
      <div class="star-score" ref="starScore">
        <i v-for="i in count" class="iconfont"
           @mouseenter="disabled ? '' : curScore = i"
           @mouseleave="disabled ? '' : curScore = ''"
           @click="disabled ? '' : setScore(i)" :class="getClass(i)">
        </i>
      </div>
      <div v-if="showText" class="text">{{curScore || score}}分</div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "vue-star-score",
    props: {
      count:{
        type: Number,
        default: 5,
      },
      type: {
        type: String,
        default: 'star'
      },
      score: {
        type: Number,
        default: 0,
        //required: true
      },
      color:{
        type: String,
        default: '#ffcc60'
      },
      fontSize: {
        type: String,
        default: '16px'
      },
      disabled: {
        type: Boolean,
        default: false,
      },
      showText: {
        type: Boolean,
        default: false,
      },
    },
    data() {
      return {
        curScore: '',
      }
    },
    watch:{
      score(newVal, oldVal){
        this.curScore = newVal;
      }
    },
    created: function () {
      this.curScore = this.score;
    },
    mounted (){
      const style = this.$refs.starScore.style;
      style.setProperty('--color', this.color);
      style.setProperty('--font-size', this.fontSize)
    },
    methods: {
      getClass(i) {
        if (this.score === '') {
          return i <= this.score ? 'icon-'+this.type+'-on' : i >= this.score+1 ? 'icon-'+this.type+'-off' : 'icon-'+this.type+'-half'
        } else {
          return i <= this.curScore ? 'icon-'+this.type+'-on' : i >= this.curScore+1 ? 'icon-'+this.type+'-off' : 'icon-'+this.type+'-half'
        }
      },
      setScore(i){
        if(!this.disabled){
          this.curScore = i;
          this.$emit('on-change',i);//使用`.sync`修饰符，对score 进行“双向绑定
        }
      }
    }
  }
</script>

<style lang="stylus" scoped>
  @import "assets/iconfont.css";

  * {
    margin: 0;
    padding: 0;
  }

  .star-box{
    display inline-block;

    .iconfont{
      color:var(--color);
      font-size var(--font-size)
      margin-left:4px;
      transition: all 0.3s ease-in-out;

      &:first-child{
        margin-left:0;
      }
    }
  }



</style>
