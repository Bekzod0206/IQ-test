<template>
  <button 
    class="uniqueBtn"
    :class="{disabled: btnActiveStatus, active: !btnActiveStatus}" 
    @click="handleClick"
  >
    {{ btnText }}
  </button>
</template>
<script>
export default{
  props:{
    defText:{
      type: String,
      default: ''
    },
    disabledStatus:{
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      btnText: this.defText,
      btnActiveStatus: this.disabledStatus,
    }
  },
  watch:{
    disabledStatus(newValue){
      this.btnActiveStatus = newValue
    }
  },
  methods: {
    handleClick(e){
      if(this.btnActiveStatus){
        return
      }
      e.target.style.opacity = '0.7'
      setTimeout(()=>{
        e.target.style.opacity = ''
      }, 100)
    }
  },
}
</script>
<style scoped>
.uniqueBtn{
  border: 1px solid grey;
  width: 189px;
  height: 41px;
  border-radius: 20px;
  font-family: "Merriweather", serif;
  font-weight: 300;
  font-style: normal;
  line-height: 17.6px;
  letter-spacing: 0.1em;
  text-align: center;
}
.disabled{
  background-color: rgba(218, 218, 218, 1);
  box-shadow: 0px 4px 10px 0px rgba(0, 0, 0, 0.25) inset;
  color: rgba(142, 142, 142, 1);
}
.active{
  background-color: rgba(255, 199, 0, 1);
  cursor: pointer
}
</style>