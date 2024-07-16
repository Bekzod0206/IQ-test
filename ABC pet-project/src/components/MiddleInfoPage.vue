<template>
  <div class="main-container">
    <div class="main-text-section">
      Также по результатам теста <b>вы получите</b> подробные <b>советы</b> по определению 
      наиболее перспективной <b>для вашего типа</b> интеллекта <b>сферы деятельности,</b>  
      которая принесет вам скорейший финансовый результат.
    </div>
    <div style="text-align: center; margin-top: 20px;">
      <img src="../assets/icons/colored_brain_img.png" alt="colored-brain">
    </div>
    <div style="text-align: center;">
      <CustomButton :def-text="'ПРОЙТИ ТЕСТ'" @click="this.$emit('testStatus', true)"/>
    </div>
  </div>
</template>
<script>
import CustomButton from './CustomComponents/CustomButton.vue'
export default{
  components:{
    CustomButton
  },
  data() {
    return {
      
    }
  },
}
</script>
<style scoped>
.main-container{
  padding-block: 20px;
  box-sizing: border-box;
}
.main-text-section{
  text-align: left;
  font-family: "PT Serif", serif;
  font-weight: 400;
  font-style: normal;
  text-wrap: pretty;
  font-size: 16px;
  line-height: 24px;
  letter-spacing: 0.05em;
}
b{
  text-transform: uppercase;
}



</style>