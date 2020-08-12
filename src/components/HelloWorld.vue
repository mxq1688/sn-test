<template>
  <div class="hello">

      <div class="title">
        图形变换
      </div>
      <div class="body">
        <div class="m1">
          <hold-all @dropEnd="dropEnd($event)" />
        </div>

        <div ref="m2" class="m2">
            <result-show
                    @move="move($event)"
                    @moveEnd="moveEnd($event)"
                    @setData="setData($event)"
                    :drawTargetEle="drawTargetEle"
                    :cindex="cindex" :dLeft="dLeft" :dTop="dTop"/>
        </div>
        <div class="m3">
            <attr-show @changeAttr="changeAttr($event)" v-bind.sync="attrData" :ctype="type" />
        </div>
      </div>

  </div>
</template>

<script>
  import HoldAll from './tool/HoldAll'
  import AttrShow from "./tool/AttrShow";
  import ResultShow from "./tool/ResultShow";
export default {
  name: 'HelloWorld',
  components: {HoldAll, AttrShow, ResultShow},
  props: {
    // msg: String
  },
  data(){
    return {
      drawTargetEle: [],
      dLeft: 0,
      dTop: 0,
      dWidth: 0,
      dHeight: 0,

      wx: 0,
      wy: 0,
      type: 1,

      attrData: {
        sx: 0,
        sy: 0,

        sr: 0,

        sw: 0,
        sh: 0,
        sj: 0,
      },
      cindex: '',
    }
  },
  mounted() {
      this.dLeft = this.$refs.m2.offsetLeft;
      this.dTop = this.$refs.m2.offsetTop;
      this.dWidth = this.$refs.m2.clientWidth;
      this.dHeight = this.$refs.m2.clientHeight;
  },
  methods: {

    getType(type){
      switch (type) {
          case 1:
            return '50%';
          case 2:
            return '0';
          case 3:
            return '15%';
      }
    },
    dropEnd(params){

      this.wx = window.event.clientX
      this.wy = window.event.clientY
      this.type = params.type
      if(this.judgeInside()){
        var draw = {
          type: this.type,
          width: params.width,
          height: params.height,
          left: this.wx - this.dLeft - params.width/2,
          top: this.wy - this.dTop - params.width/2,
          jd: 0,
        };
        this.drawTargetEle.push(draw)
        this.setAttr(draw.left, draw.top, draw.width, draw.height, 0);
        this.cindex = this.drawTargetEle.length - 1;
      }
    },
      changeAttr(data){

          switch (data.type) {
            case 'sx':
                this.drawTargetEle[this.cindex].left = this.attrData.sx;
                break;
            case 'sy':
                this.drawTargetEle[this.cindex].top = this.attrData.sy;
                break;
              case 'sw':
                  this.drawTargetEle[this.cindex].width = this.attrData.sw;
                  this.drawTargetEle[this.cindex].left = this.drawTargetEle[this.cindex].left - (this.attrData.sw-data.ov)/2
                  break;
              case 'sh':
                  this.drawTargetEle[this.cindex].height = this.attrData.sh;
                  this.drawTargetEle[this.cindex].top = this.drawTargetEle[this.cindex].top - (this.attrData.sh-data.ov)/2
                  break;

              case 'sj':
                  this.drawTargetEle[this.cindex].jd = this.attrData.sj;
                  break;
              case 'sr':
                  this.drawTargetEle[this.cindex].width = this.attrData.sr * 2;
                  this.drawTargetEle[this.cindex].height = this.attrData.sr * 2;
                  this.drawTargetEle[this.cindex].left = this.drawTargetEle[this.cindex].left - (this.attrData.sr-data.ov)
                  this.drawTargetEle[this.cindex].top  = this.drawTargetEle[this.cindex].top - (this.attrData.sr-data.ov)
                  break;
          }
      },
    setAttr(sx, sy, sw, sh, sj){

      this.attrData.sx = sx
      this.attrData.sy = sy
      if(this.type == 1){
        this.attrData.sr = sw/2;
      }else{
        this.attrData.sw = sw
        this.attrData.sh = sh
        this.attrData.sj = sj;
      }
    },
    setData(data){
      this.cindex = data.cindex;
      this.type = data.vo.type;
      this.setAttr(data.vo.left,data.vo.top,data.vo.width, data.vo.height, data.vo.jd)
    },
    judgeInside() {
      var wx = this.wx;
      var wy = this.wy;
      var d_left = this.dLeft;
      var d_top = this.dTop;
      var d_width = this.dWidth;
      var d_height = this.dWidth;
      if(wx < d_left || wy<d_top || wx > (d_left + d_width) || wy > (d_top + d_height))
        return false;
      else
        return true;
    },
    move(data){
      this.cindex = data.cindex;
      this.attrData.sx = window.event.clientX - data.sx;
      this.attrData.sy = window.event.clientY - data.sy;
        this.$forceUpdate()
    },
    moveEnd(data){
        this.cindex = data.cindex;
        this.attrData.sx = window.event.clientX - data.sx;
        this.attrData.sy = window.event.clientY - data.sy;
      this.drawTargetEle[this.cindex].left = this.attrData.sx
      this.drawTargetEle[this.cindex].top = this.attrData.sy
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .hello{
    height: 100%;
  }
  .title{
    height: 30px;
    background: #d8d8d8;
    text-align: start;
    padding: 10px;
    display: flex;
    align-items: center;
  }
  .body{
    display: flex;
    /*align-items: center;*/
    flex-flow: row nowrap;
    height: calc( 100% - 30px );
  }
  .m1{
    width: 80px;
    height: 100%;
    background: #eeeff0;
  }
  .m1 .xz{
    width: 20px;
    height: 20px;
    border: 2px solid black;
    margin: 0 auto;
    margin-top: 40px;
  }
  .m2{
    flex: 1;
    box-shadow:inset 10px 0px 15px -15px #000,
    inset -10px 0px 15px -15px #000;
    position: relative;
  }
  .m3{
    width: 300px;
    height: 100%;

  }
</style>
