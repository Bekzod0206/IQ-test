<template>
  <div class="main-content" style="color: white;">

    <!-- progress bar -->
    <div style="display: flex; justify-content: center;" class="fade" :class="{'fade-hidden': testEndStatus}">
      <div style="width: 90%; height: 11px; background-color: grey; border-radius: 15px;">
        <div :style="dynamicStyle"></div>
      </div>
    </div>

    <!-- question section -->
    <div style="margin: 30px; text-align: center;" class="fade" :class="{'fade-hidden': testEndStatus}">
      {{ questionsArr[progressCounter] ? questionsArr[progressCounter].question : '' }}
    </div>

    <div style="min-height: 350px; margin: 20px 0;">

      <div class="fade" :class="{'fade-hidden': testEndStatus}">
        <template v-if="loaderState">
          <div class="loading-section" style="display: flex; justify-content: center; align-items: center;">
            <div>
              <div class="loading-section-header" style="text-align: center;">
                Обработка результатов
              </div>
              <div style="text-align: center; margin: 70px 0;">
                <img class="loader-icon" src="../assets/icons/loader_img.png" alt="">
              </div>
              <div class="loading-section-decr" style="text-align: center;">
                Определение стиля мышления...........
              </div>
            </div>
          </div>
        </template>
  
        <template v-if="answersArr[progressCounter] && answersArr[progressCounter].type === 'list'">
          <ul style="list-style: none; padding: 0;">
            <li 
              v-for="(str, id) in answersArr[progressCounter].answers"
              :key="id"
              :class="{'active': currentVal === str}"
              class="answerListParent"
              @click="handleListValue(str)"
            >
              <div class="answerListEl">
                <input 
                  type="radio"
                  :name="answersArr[progressCounter].name"
                  :value="str"
                  v-model="selectedAnswer"
                >
                <label>{{ str }}</label>
              </div>
            </li>
          </ul>
        </template>
        <template v-else-if="answersArr[progressCounter] && answersArr[progressCounter].type === 'palette'">
          <div class="palette-outer-box">
            <div class="palette-inner-box">
              <div 
                v-for="(color, id) in answersArr[progressCounter].answers"
                :key="id" class="palette-box"
                :style="{ backgroundColor: color }"
                @click="handlePaletteValue(color)"
                :class="{'active-palette': currentVal === color}"
              >
              </div>
            </div>
          </div>
        </template>
  
        <template v-else-if="answersArr[progressCounter] && answersArr[progressCounter].type === 'image'">
          <div class="image-outer-box">
            <div class="image-inner-box">
              <div class="image-section">
                <img v-if="answersArr[progressCounter].imgName === 'people'" src="../assets/icons/eighth-question-img.png" alt="questionImg">
                <img v-if="answersArr[progressCounter].imgName === 'triangle'" src="../assets/icons/tenth-question-img.png" alt="questionImg">
                <img v-if="answersArr[progressCounter].imgName === 'star'" src="../assets/icons/eleventh-question-img.png" alt="questionImg">
              </div>
              <div :class="{'image-answers-section': answersArr[progressCounter].imgName === 'people' || answersArr[progressCounter].imgName === 'star'}">
                <div 
                  v-for="(str, id) in answersArr[progressCounter].answers"
                  :key="id"
                  class="image-answer-box"
                  @click="handleImgValue(str)"
                  :class="{'active': currentVal === str}"
                >
                  {{ str }}
                </div>
              </div>
            </div>
          </div>
        </template>
      </div>
      <template v-if="testEndStatus">
          <div class="resultInfoBox">
            <div class="resultInfoHeader">
              <h3 style="line-height: 35px;">Ваш результат рассчитан:</h3>
              <p> <u>Вы относитесь к 3%</u> респондентов, чей уровень интеллекта более чем на 
                15 пунктов отличается от среднего в большую или меньшую сторону!</p>
            </div>
            <div class="resultInfoNotification">
              <h3 style="color: #3BDE7C; text-transform: uppercase; line-height: 35px;">
                Скорее получите свой результат!
              </h3>
            </div>
            <div class="resultInfoCard">
              В целях защиты персональных данных результат теста, их подробная интерпретация и рекомендации 
              доступны в виде голосового сообщения по звонку с вашего мобильного телефона
            </div>
            <div class="resultInfoTimer" style="color: #3BDE7C; margin-block: 20px;">
              Звоните скорее, запись доступна всего
              <br>
              <b>{{ formattedTime }}</b> минут
            </div>
            <div class="resultInfoSubmit" @click="fetchData">
              <div :class="{'disabledBtnStatus': !timerActiveStatus}">
                <img src="../assets/icons/phone_img.png" alt="phoneImg">
                <p>Позвонить и прослушать результат</p>
              </div>
            </div>

            <div v-if="fetchResultData" class="resultDataContent">

              <table class="responseTable">
                <tr v-for="(str, id) in dataTableHeader" :key="id">
                  <td>
                    {{ str }}
                  </td>
                  <td>
                    <template v-if="typeof fetchResultData[str] === 'string' && fetchResultData[str].includes('https')">
                      <a :href="fetchResultData[str]">{{fetchResultData[str]}}</a>
                    </template>
                    <template v-if="typeof fetchResultData[str] === 'object'">
                      <p v-for="(url, id) in fetchResultData[str]" :key="id" >
                        <a :href="url">{{url}}</a>
                        <br>
                      </p>
                    </template>
                    <template v-else-if="typeof fetchResultData[str] === 'string' && !fetchResultData[str].includes('https')">
                      {{fetchResultData[str]}}
                    </template>
                  </td>
                  
                </tr>
              </table>

            </div>

          </div>
      </template>
    </div>

    <div style="text-align: center; margin-bottom: 15px;" class="fade" :class="{'fade-hidden': testEndStatus}">
      <CustomButton 
        :def-text="'ДАЛЕЕ'"
        :disabled-status="currentVal ? false : true"
        @click="currentVal ? viewNextQuestion() : ''"/>
    </div>

    

  </div>
</template>
<script>
import CustomButton from './CustomComponents/CustomButton.vue';
export default{
  components:{
    CustomButton
  },
  data() {
    return {
      questionsArr: [
        {
          name: 'first',
          id: 0,
          question: 'Ваш пол:'
        },
        {
          name: 'second',
          id: 1,
          question: 'Укажите ваш возраст: '
        },
        {
          name: 'third',
          id: 2,
          question: 'Выберите лишнее:'
        },
        {
          name: 'fourth',
          id: 3,
          question: 'Продолжите числовой ряд: 18  20  24  32'
        },
        {
          name: 'fifth',
          id: 4,
          question: 'Выберите цвет, который сейчас наиболее Вам приятен:'
        },
        {
          name: 'sixth',
          id: 5,
          question: 'Отдохните пару секунд, еще раз Выберите цвет, который сейчас наиболее Вам приятен:'
        },
        {
          name: 'seventh',
          id: 6,
          question: 'Какой из городов лишний?'
        },
        {
          name: 'eighth',
          id: 7,
          question: 'Выберите правильную фигуру из четырёх пронумерованных.'
        },
        {
          name: 'nineth',
          id: 8,
          question: 'Вам привычнее и важнее:'
        },
        {
          name: 'tenth',
          id: 9,
          question: 'Какое определение, по-Вашему, больше подходит к этому геометрическому изображению: '
        },
        {
          name: 'eleventh',
          id: 10,
          question: 'Вставьте подходящее число'
        }
      ],
      answersArr: [
        {
          name: 'first',
          id: 0,
          answers:['Мужчина', 'Женщина'],
          type: 'list'
        },
        {
          name: 'second',
          id: 1,
          answers:['До 18', 'От 18 до 28', 'от 29 до 35', 'От 36'],
          type: 'list'
        },
        {
          name: 'third',
          id: 2,
          answers:['Дом', 'Шалаш', 'Бунгало', 'Скамейка', 'Хижина'],
          type: 'list'
        },
        {
          name: 'fourth',
          id: 3,
          answers:['62', '48', '74', '57', '60', '77'],
          type: 'list'
        },
        {
          name: 'fifth',
          id: 4,
          answers:['#A8A8A8', '#0000A9', '#00A701', '#F60100', '#FDFF19', '#A95403', '#000000', '#850068', '#46B2AC'],
          type: 'palette'
        },
        {
          name: 'sixth',
          id: 5,
          answers:['#A8A8A8', '#46B2AC', '#A95403', '#00A701', '#000000', '#F60100', '#850068', '#FDFF19', '#0000A9'],
          type: 'palette'
        },
        {
          name: 'seventh',
          id: 6,
          answers:['Вашингтон', 'Лондон', 'Париж', 'Нью-Йорк', 'Москва', 'Оттава'],
          type: 'list'
        },
        {
          name: 'eighth',
          id: 7,
          answers:['1', '2', '3', '4'],
          type: 'image',
          imgName: 'people'
        },
        {
          name: 'nineth',
          id: 8,
          answers:['Наслаждаться каждой минутой проведенного времени', 'Быть устремленными мыслями в будущее', 'Учитывать в ежедневной практике прошлый опыт'],
          type: 'list',
        },
        {
          name: 'tenth',
          id: 9,
          answers:['Оно остроконечное', 'Оно устойчиво', 'Оно-находится в состоянии равновесия'],
          type: 'image',
          imgName: 'triangle'
        },
        {
          name: 'eleventh',
          id: 10,
          answers:['34', '36', '53', '44', '66', '42'],
          type: 'image',
          imgName: 'star'
        }
      ],
      selectedAnswer: '',
      chosenAnswersObj: {},
      progressCounter: 1,
      currentVal: '',
      loaderState: false,
      testEndStatus: false,
      timeRemaining:  1 * 15,// 10 * 60,
      timer: null,
      timerActiveStatus: true,
      fetchResultData: '',
      dataTableHeader: [],
    }
  },
  computed:{
    dynamicStyle() {
      const totalWidth = 100 / 11 * this.progressCounter;
      return {
        width: `${totalWidth}%`,
        height: '100%',
        backgroundColor: '#3BDE7C',
        borderRadius: '15px'
      };
    },
    formattedTime() {
      const minutes = Math.floor(this.timeRemaining / 60);
      const seconds = this.timeRemaining % 60;
      return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
    }
  },
  watch: {
    selectedAnswer(value){
      this.chosenAnswersObj[this.answersArr[this.progressCounter - 1].name] = value
      this.currentVal = value
    },
    progressCounter(value){
      if(value === this.questionsArr.length){
        this.applyLoaderState()
      }
    }
  },
  methods:{
    handleListValue(value){
      this.selectedAnswer = value
    },
    viewNextQuestion(){
      this.currentVal = ''
      this.selectedAnswer = ''
      this.progressCounter++
    },
    handlePaletteValue(value){
      this.chosenAnswersObj[this.answersArr[this.progressCounter - 1].name] = value
      this.currentVal = value
    },
    handleImgValue(value){
      this.chosenAnswersObj[this.answersArr[this.progressCounter - 1].name] = value
      this.currentVal = value
    },
    applyLoaderState(){
      this.loaderState = true
      setTimeout(()=>{
        this.testEndStatus = true
        this.loaderState = false
        this.startTimer()
      }, 5000)
    },
    startTimer() {
      if (this.timer) {
        clearInterval(this.timer);
      }
      this.timer = setInterval(() => {
        if (this.timeRemaining > 0) {
          this.timeRemaining--;
        } else {
          clearInterval(this.timer);
          this.timer = null;
          this.timerActiveStatus = false
        }
      }, 1000);
    },
    async fetchData(){
      if(!this.timerActiveStatus){
        return
      }
      this.timerActiveStatus = false
      try {
        const response = await fetch('https://swapi.dev/api/people/1/'); // Replace with your URL
        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }
        const result = await response.json();
        this.fetchResultData = result
        this.dataTableHeader = Object.keys(this.fetchResultData);

        console.log(result, 'result')
      } catch (err) {
        alert('Error' + err.message)
      }
    }
  },
  beforeDestroy() {
    if (this.timer) {
      clearInterval(this.timer);
    }
  },
}
</script>
<style scoped>
.main-content{
  font-family: "PT Serif", serif;
  font-weight: 400;
  font-style: normal;
  font-size: 20px;
  line-height: 24px;
  letter-spacing: 0.05em;
}
.answerListParent{
  cursor: pointer;
}
.answerListEl{
  width: 100%;
  min-height: 50px;
  padding-inline: 10px;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  background-color: #80808096;
  margin: 10px 0;
}
.answerListEl input{
  margin-right: 50px;
  height: 20px;
  width: 20px;
  cursor: pointer;
}
.palette-outer-box,
.palette-inner-box{
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
}
.palette-outer-box{
  height: 100%;
  width: 100%;
}
.palette-inner-box{
  gap: 20px;
  width: 350px;
}
.palette-box{
  width: 75px;
  height: 75px;
  cursor: pointer;
}
.active{
  background-color: rgba(255, 199, 0, 1) !important;
}
.active-palette{
  border: 5px solid rgba(255, 199, 0, 1);;
}
.image-outer-box{
  display: flex;
  justify-content: center;
  align-items: center;
}
.image-section,
.image-answers-section{
  display: flex;
  justify-content: center;
  align-items: center;
}
.image-section img{
  width: 100%;
}
.image-answers-section{
  margin: 10px 0;
  gap: 15px;
}
.image-answer-box{
  min-width: 45px;
  min-height: 45px;
  background-color: #80808096;
  text-align: center;
  margin: 10px 0;
  padding: 10px;
  box-sizing: border-box;
  cursor: pointer;
}

.fade {
  opacity: 1;
  visibility: visible;
  transition: opacity 0.5s ease, visibility 0.5s ease;
}

.fade-hidden {
  opacity: 0;
  visibility: hidden;
}
@keyframes infinite-rotation {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(-360deg);
  }
}
.loader-icon {
  display: inline-block;
  animation: infinite-rotation 2s linear infinite;
}
.resultInfoBox  div{
  text-align: center;
}
.resultInfoCard{
  background-color: #1C2741;
  border-radius: 10px;
  padding: 10px;
  box-sizing: border-box;
  margin-inline: 5px;
}
.resultInfoSubmit{
  display: flex;
  justify-content: center;
  align-items: center;
}
.resultInfoSubmit div{
  display: flex;
  align-items: center;
  justify-content: center;
  width: 290px;
  height: 90px;
  background-color: #EB1B00;
  padding: 5px;
  border-radius: 10px;
  cursor: pointer;
}
.resultInfoSubmit p{
  text-align: left;
  padding: 10px;
  box-sizing: border-box;
}
.disabledBtnStatus{
  background-color: grey !important;
  color: white;
  cursor: not-allowed !important;
  opacity: 0.5;
}
.resultDataContent{
  height: 260px;
  overflow: auto;
  text-align: left;
  background-color: #1C2741;
  padding: 5px;
  margin: 10px;
}
.resultDataContent ul{
  list-style: none;
}
.responseTable{
  width: 100%;
  border-collapse: collapse;
  margin: 20px 0;
  font-size: 18px;
  text-align: left;
}
.responseTable th, .responseTable td {
  padding: 12px 15px;
  border: 1px solid #dddddd;
}
@media screen and (max-width: 768px) {
  .resultInfoHeader p{
    font-size: 15px;
  }
  .resultInfoCard{
    font-size: 15px;
  }
}
</style>