<template>
    <div>
      <div class="m1">
        <div style="font-size: 14px;color: gray;padding: 8px;border-bottom: 1px solid lightgray">工具箱</div>
        <div>
          <div v-for="(vo, index) in shapes" draggable="true" @dragend="dropEnd(vo)" class="xz" :style="{borderRadius: getType(vo.type)}" :key="index">

          </div>
        </div>
      </div>
    </div>
</template>

<script>
export default {
  name: 'HoldAll',
  props: {
    shapes: {
      type: Array,
      required: false,
      default: ()=>[
        {
          type: 1,
          width: 40,
          height: 40,
        },
        {
          type: 2,
          width: 40,
          height: 40,
        },
        {
          type: 3,
          width: 40,
          height: 40,
        },
      ]
    },
  },
  data(){
    return {
    }
  },
  mounted() {

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
    dropEnd(item){
      this.$emit('dropEnd', {type: item.type, width: item.width, height: item.height})
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .m1{
    width: 100%;
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
</style>
