<template>
    <div>
            <div draggable="true"
                 v-for="(vo,index) in drawTargetEle"
                 :key="index"
                 :style="{width: vo.width+'px', height: vo.height+'px',
                            borderRadius: getType(vo.type),
                            left: vo.left+'px', top: vo.top+'px',
                            borderColor: cindex == index? 'red': '',
                            transform: getRotate(vo.jd)}"
                 style="border: 1px solid black;position: absolute"

                 @drag="move(vo,index)"
                 @dragend="moveEnd(vo,index)"
                 @click="setData(vo, index)"
            >
            </div>
    </div>
</template>

<script>
export default {
  name: 'HoldAll',
  props: {
      drawTargetEle: {
          type: Array,
          required: true
        },
      cindex: [Number, String],
      dLeft: [Number, String],
      dTop: [Number, String],
  },
  data(){
    return {
    }
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
      move(vo, index){
          this.$emit('move', {cindex: index, sx: this.dLeft + vo.width/2, sy:  this.dTop + vo.height/2})
      },
      moveEnd(vo, index){
          this.$emit('moveEnd', {cindex: index, sx: this.dLeft + vo.width/2, sy: this.dTop + vo.height/2})
      },
      setData(vo, index){
          this.$emit('setData', {cindex: index, vo: vo})
      },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
