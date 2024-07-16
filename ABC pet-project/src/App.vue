<template>
  <div v-show="showSidebar" class="sidebar-section-outer">
    <div class="sidebar-section-inner inner">
      <div class="sidebar-section-menu">
        <div class="sidebar-content menu-text">
          <p style="text-align: end; padding: 0 10px; box-sizing: border-box;" @click="showSidebar = false">
            <span class="material-symbols-outlined">
              close
            </span>
          </p>
          <h3 @click="selectSection('main')">
            <a href="#mainPage" style="text-decoration: none;" :class="{'text-active': currentMainPage === 'main'}">
              Главная
            </a>
          </h3>
          <h3 @click="selectSection('testInfo')" >
            <a href="#infoPage" style="text-decoration: none;" :class="{'text-active': currentMainPage === 'testInfo'}">
              Информация о тесте
            </a>
          </h3>
          <h3 @click="selectSection('startTest')" >
            <a href="#middleInfoPage" style="text-decoration: none;" :class="{'text-active': currentMainPage === 'startTest'}">
              Пройти тест
            </a>
          </h3>
        </div>
        <div class="sidebar-close"></div>
      </div>
      
    </div>
  </div>

  <div class="navbar-section-outer outer">
    <div class="navbar-section-inner inner">
      <template v-if="testActive">
        <div style="display: flex; justify-content: center; align-items: center; gap: 10px; position: relative;">
          <img
            src="./assets/icons/burger-menu.png"
            alt="burger-menu"
            style="cursor: pointer; height: 12px; position: absolute; left: 10px"
            @click="showSidebar = true"
          >
          <div style="display: flex; align-items: center; gap: 5px">
            <img src="./assets/icons/brain_icon.png" alt="brain-icon" style="width: 48px; height: 47px;">
            <h3 class="test-header">ТЕСТ НА ОПРЕДЕЛЕНИЕ IQ</h3>
          </div>
        </div>
      </template>

      <template v-else>
        <img
          class="burger-menu-icon"
          src="./assets/icons/burger-menu.png"
          alt="burger-menu"
          style="cursor: pointer;"
          @click="showSidebar = true"
        >
        <div class="navbar-menu-text menu-text">
          <h3 @click="selectSection('main')" :class="{'text-active': currentMainPage === 'main'}">
            <a href="#mainPage" style="text-decoration: none;" :class="{'text-active': currentMainPage === 'main'}">
              Главная
            </a>
          </h3>
          <h3 @click="selectSection('testInfo')" :class="{'text-active': currentMainPage === 'testInfo'}">
            <a href="#infoPage" style="text-decoration: none;" :class="{'text-active': currentMainPage === 'testInfo'}">
              Информация о тесте
            </a>
          </h3>
          <h3 @click="selectSection('startTest')" :class="{'text-active': currentMainPage === 'startTest'}">
            <a href="#middleInfoPage" style="text-decoration: none;" :class="{'text-active': currentMainPage === 'startTest'}">
              Пройти тест
            </a>
          </h3>
        </div>
      </template>
    </div>
  </div>

  <template v-if="!testActive">
    <div class="homepage-outer outer" id="mainPage">
      <div class="inner" style="display: flex; justify-content: center;">
        <HomePage @scrollUp="(value)=>{handleScrollEvent(value)}" @testStatus="(value)=>{handleTestSection(value)}"/>
      </div>
    </div>
  
    <div class="info-outer outer" id="infoPage" ref="targetSection">
      <div class="inner" style="display: flex; justify-content: center; ">
        <InfoPage/>
      </div>
    </div>
    
    <div class="middle-info-outer outer" id="middleInfoPage">
      <div class="inner" style="display: flex; justify-content: center; ">
        <MiddleInfoPage @testStatus="(value)=>{handleTestSection(value)}"/>
      </div>
    </div>
  
    <div class="footer-info-outer outer" id="footerInfoPage">
      <img src="./assets/icons/lightning_img.png" alt="lightning" class="first-light">
      <img src="./assets/icons/lightning_img.png" alt="lightning" class="second-light">
  
      <div class="inner" style="display: flex; justify-content: center; ">
        <FooterInfoPage @testStatus="(value)=>{handleTestSection(value)}"/>
      </div>
    </div>
  </template>

  <template v-else>
    <div class="testpage-outer outer" style="min-height: 100vh;">
      <div class="inner">
        <TestPage/>
      </div>
    </div>
  </template>
  
</template>
<script>
import HomePage from './components/HomePage.vue'
import InfoPage from './components/InfoPage.vue'
import TestPage from './components/TestPage.vue'
import MiddleInfoPage from './components/MiddleInfoPage.vue'
import FooterInfoPage from './components/FooterInfoPage.vue'
export default{
  components:{
    HomePage,
    InfoPage,
    TestPage,
    MiddleInfoPage,
    FooterInfoPage,
    FooterInfoPage
},
  data() {
    return {
      showSidebar: false,
      currentMainPage: 'main',
      testActive: false
    }
  },
  methods:{
    selectSection(status){
      this.testActive = false
      this.showSidebar = false
      this.currentMainPage = status
    },
    handleScrollEvent(val){
      if(val){
        const target = this.$refs.targetSection;
        if (target) {
          target.scrollIntoView({ behavior: 'smooth' });
        }
      }
    },
    handleTestSection(val){
      console.log(val, 'val')
      if(val){
        this.testActive = true
      }
    }
  }
}
</script>
<style scoped>
.outer{
  width: 100%;
  height: 500px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.inner{
  width: 60%;
  padding-inline: 5px;
  box-sizing: border-box;
}
.sidebar-section-outer{
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  background-color: grey;
  display: flex;
  justify-content: center;
  background-color: #54535396;
  z-index: 99;
}
.sidebar-section-inner{
  height: 560px;
  background-color: rgba(24, 24, 24, 1);
}
.sidebar-section-menu{
  width: 100%;
}
.navbar-section-outer{
  height: 46px;
  background-color: rgba(39, 39, 39, 1);
  position: sticky;
  top: -2px;
}
.burger-menu-icon{
  display: none
}
.navbar-menu-text{
  display: flex;
  justify-content: center;
  gap: 30px;
}
.menu-text {
  color: white;
  font-family: "Roboto", sans-serif;
  font-weight: 100;
  font-style: normal;
  text-align: left;
  cursor: pointer;
}
.menu-text a{
  color: white;
}
.text-active{
  color: rgba(244, 206, 12, 1) !important;
}
.burger-menu-icon{
  /*need to set to block in a particular key frame  */
  display: none;
  margin-top: 15px;
  position: relative;
}
.homepage-outer{
  height: 730px !important
}
.homepage-outer,
.testpage-outer{
  background-image: url('./assets/icons/background_dotted_img.png');
}
.info-outer{
  background-image: url('./assets/icons/descr_bg.png');
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  padding-block: 30px;
  box-sizing: border-box;
}
.footer-info-outer{
  background-image: url('./assets/icons/background_dotted_img.png');
  position: relative;
  height: 400px;
}
.first-light{
  position: absolute;
  left: 0;
  bottom: -10px
}
.second-light{
  position: absolute;
  transform: scaleX(-1);
  right: 0;
}
.test-header{
  font-family: "Yeseva One", serif;
  font-weight: 400;
  font-style: normal;
  color: rgba(255, 199, 0, 1);
}

@media screen and (max-width: 768px) {
  .inner {
    width: 100%;
  }
  .navbar-menu-text{
    display: none
  }
  .burger-menu-icon{
    display: block;
  }
  .test-header{
    font-size: 12px;
  }
}
</style>