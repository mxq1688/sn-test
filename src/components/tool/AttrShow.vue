<template>
    <div>
        <div class="m3">
            <div style="height: 20px; border-bottom: 1px solid lightgray">
                属性查看器
            </div>
            <div>
                <div>位置</div>
                <div>
                    <div>
                        x <input type="number" :value="sx" @input="changeData('sx',$event.target.value)">
                    </div>
                    <div>
                        y <input type="number" :value="sy" @input="changeData('sy',$event.target.value)">
                    </div>
                </div>
            </div>
            <div v-if="ctype !=1 ">
                旋转 <input type="number" :value="sj" @input="changeData('sj',$event.target.value)">
            </div>
            <div v-if="ctype ==1 ">
                半径 <input type="number" :value="sr" @input="changeData('sr',$event.target.value)">
            </div>
            <div v-if="ctype !=1 ">
                <div>
                    宽 <input type="number" :value="sw" @input="changeData('sw',$event.target.value)">
                </div>
                <div>
                    高 <input type="number" :value="sh" @input="changeData('sh',$event.target.value)">
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'AttrShow',
  props: {
    ctype: {
        type: [Number, String],
        required: true,
        default: 1,
    },
  sx: {
      type: [Number, String],
      default: 0,
  },
  sy: {
      type: [Number, String],
      default: 0,
  },
  sr: {
      type: [Number, String],
      default: 0,
  },
  sw: {
      type: [Number, String],
      default: 0,
  },
  sh: {
      type: [Number, String],
      default: 0,
  },
  sj: {
      type: [Number, String],
      default: 0,
  },
  },
  data(){
    return {
        type: this.ctype,
    }
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
      changeData(type, value){

          switch (type) {
              case 'sx':
                  this.$emit('update:sx', value);
                  this.$emit('changeAttr', {type:'sx', ov: this.sx});
                  break;
              case 'sy':
                  this.$emit('update:sy', value);
                  this.$emit('changeAttr', {type:'sy', ov:this.sy});
                  break;
              case 'sw':
                  this.$emit('update:sw', value);
                  this.$emit('changeAttr', {type:'sw', ov:this.sw});
                  break;
              case 'sh':
                  this.$emit('update:sh', value);
                  this.$emit('changeAttr', {type:'sh', ov:this.sh});
                  break;

              case 'sj':
                  this.$emit('update:sj', value);
                  this.$emit('changeAttr', {type:'sj', ov:this.sj});
                  break;
              case 'sr':
                  this.$emit('update:sr', value);
                  this.$emit('changeAttr', {type:'sr', ov:this.sr});

                  break;
          }
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .m3{
        width: 100%;
        height: 100%;

    }
</style>
