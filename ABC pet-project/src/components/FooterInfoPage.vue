<template>
  <div class="main-container">
    <div class="main-text-section">
      Прохождение теста займет у вас не более <span class="unique-text">12 минут</span>, а его результаты вы сможете <span class="unique-text">использовать всю жизнь.</span>
    </div>

    <div class="info-box-card">
      Профессиональная интерпретация и детально проработанные рекомендации позволят вам качественно изменить 
      все аспекты своей жизни: от финансового до любовного.
    </div>

    <div style="margin: ;">
      <CustomButton :def-text="'ПРОЙТИ ТЕСТ'" @click="this.$emit('testStatus', true)" style="background-color: transparent; color: white;"/>
    </div>
    <footer style="width: 100%; margin-top: 40px;">
      <div style="color: rgba(255, 255, 255, 0.5); display: flex; justify-content: center; align-items: center; gap: 5px;">
        <span class="material-symbols-outlined">
          copyright
        </span> <span>2019</span>
      </div>
    </footer>
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
  position: relative;
  padding-top: 20px;
  box-sizing: border-box;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap:15px;
}
.main-text-section{
  text-align: left;
  font-family: "Merriweather", serif;
  font-weight: 300;
  font-style: normal;
  text-wrap: pretty;
  font-size: 16px;
  line-height: 24px;
  letter-spacing: 0.05em;
  color: white;
}
.unique-text{
  color: rgba(255, 199, 0, 1);
}
.info-box-card{
  width: 98%;
  padding: 15px 10px;
  box-sizing: border-box;
  background-color: rgb(219 219 219 / 79%);
  border-radius: 15px;
}

</style>