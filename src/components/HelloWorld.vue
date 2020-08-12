<template>
  <div class="hello">

      <div class="title">
        图形变换
      </div>
      <div class="body">
        <div class="m1">
          <div style="font-size: 14px;color: gray;padding: 8px;border-bottom: 1px solid lightgray">工具箱</div>
          <div>
            <div draggable="true" @dragend="dropEnd($event, 1)" class="xz" style="border-radius: 50%">

            </div>
            <div draggable="true"  @dragend="dropEnd($event, 2)" class="xz">

            </div>
            <div draggable="true"  @dragend="dropEnd($event, 3)" class="xz" style="border-radius: 15%">

            </div>
          </div>
        </div>
        <div ref="m2" class="m2">
          <div draggable="true" @drag="move(vo,index, $event)" @dragend="moveEnd(vo,index, $event)" @click="setData(vo, index)" v-for="(vo,index) in drawTargetEle" :key="index" :style="{width: vo.width+'px', height: vo.height+'px',borderRadius: getType(vo.type), left: vo.left+'px', top: vo.top+'px', borderColor: cindex == index? 'red': '', transform: getRotate(vo.jd)}" style="border: 1px solid black;position: absolute">

          </div>
        </div>
        <div class="m3">
          <div style="height: 20px; border-bottom: 1px solid lightgray">
            属性查看器
          </div>
          <div>
            <div>位置</div>
            <div>
              <div>
                x <input type="number" v-model="sx">
              </div>
              <div>
                y <input type="number" v-model="sy">
              </div>
            </div>
          </div>
          <div v-if="type !=1 ">
            旋转 <input type="number" v-model="sj">
          </div>
          <div v-if="type ==1 ">
            半径 <input type="number" v-model="sr">
          </div>
          <div v-if="type !=1 ">
            <div>
              宽 <input type="number" v-model="sw">
            </div>
            <div>
              高 <input type="number" v-model="sh">
            </div>
          </div>
        </div>
      </div>

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
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

      sx: 0,
      sy: 0,

      sr: 0,

      sw: 0,
      sh: 0,
      sj: 0,

      cindex: '',
    }
  },
  mounted() {
      this.dLeft = this.$refs.m2.offsetLeft;
      this.dTop = this.$refs.m2.offsetTop;
      this.dWidth = this.$refs.m2.clientWidth;
      this.dHeight = this.$refs.m2.clientHeight;
  },
  watch:{
    sx(nv){
      this.drawTargetEle[this.cindex].left = nv;
    },
    sy(nv){
      this.drawTargetEle[this.cindex].top = nv;
    },
    sw(nv, ov){
      this.drawTargetEle[this.cindex].width = nv;
      this.drawTargetEle[this.cindex].left = this.drawTargetEle[this.cindex].left - (nv-ov)/2
    },
    sh(nv, ov){
      this.drawTargetEle[this.cindex].height = nv;
      this.drawTargetEle[this.cindex].top = this.drawTargetEle[this.cindex].top - (nv-ov)/2
    },
    sj(nv){
      this.drawTargetEle[this.cindex].jd = nv;
    },
    sr(nv, ov){
      this.drawTargetEle[this.cindex].width = nv * 2;
      this.drawTargetEle[this.cindex].height = nv * 2;
      this.drawTargetEle[this.cindex].left = this.drawTargetEle[this.cindex].left - (nv-ov)
      this.drawTargetEle[this.cindex].top = this.drawTargetEle[this.cindex].top - (nv-ov)
    },
  },
  methods: {
    getRotate(jd){
      return 'rotate('+ jd +'deg )';
    },
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
    dropEnd(e, type){
      this.wx = e.x;
      this.wy = e.y;
      this.type = type
      if(this.judgeInside()){
        var draw = {
          type: this.type,
          width: 40,
          height: 40,
          left: this.wx - this.dLeft - 20,
          top: this.wy - this.dTop - 20,
          jd: 0,
        };
        this.drawTargetEle.push(draw)
        this.setAttr(draw.left, draw.top, draw.width, draw.height, 0);
        this.cindex = this.drawTargetEle.length - 1;
      }
    },
    setAttr(sx, sy, sw, sh, sj){

      this.sx = sx
      this.sy = sy
      if(this.type == 1){
        this.sr = sw/2;
      }else{
        this.sw = sw
        this.sh = sh
        this.sj = sj;
      }
    },
    setData(vo, index){
      this.cindex = index;
      this.type = vo.type;
      this.setAttr(vo.left,vo.top,vo.width, vo.height, vo.jd)
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
    move(vo, index, e){
      this.cindex = index;
      this.sx = e.x - this.dLeft - vo.width/2
      this.sy = e.y - this.dTop - vo.height/2

    },
    moveEnd(vo, index, e){
      this.cindex = index;
      this.sx = e.x - this.dLeft - vo.width/2
      this.sy = e.y - this.dTop - vo.height/2
      this.drawTargetEle[index].left = this.sx
      this.drawTargetEle[index].top = this.sy
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
