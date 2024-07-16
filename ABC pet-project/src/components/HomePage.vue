<template>
  <div style="text-align: center;">
    <h3 style="color: white;">Пройдите точный и быстрый</h3>
    <h1 style="color: rgba(255, 199, 0, 1); text-wrap: pretty;">Тест на определние IQ</h1>
    <img src="../assets/icons/brain_icon.png" alt="brain-icon">
    <div>
      <CustomButton :def-text="'ПРОЙТИ ТЕСТ'" @click="this.$emit('testStatus', true)"/>
    </div>
    <h3 class="description text-style">
      <span style="color: rgba(255, 199, 0, 1); text-wrap: pretty;">И получите рекомендации по развитию своего интеллекта</span>
      <br>
      <span style="color: white; text-wrap: pretty;">и улучшению финансового благосостояния и личной жизни</span>
    </h3>
    <div class="text-style" style="text-align: center;">
      <button class="material-symbols-outlined arrow-btn" @click="this.$emit('scrollUp', true)">
        arrow_upward
      </button>
      <p>Подробнее</p>
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
.description{
  font-weight: 700;
}
.text-style{
  font-family: "PT Serif", serif;
  font-style: normal;
  color: white;
}
.arrow-btn{
  border: none;
  cursor: pointer;
  width: 40px;
  height: 40px;
  border-radius: 20px;
}
</style>