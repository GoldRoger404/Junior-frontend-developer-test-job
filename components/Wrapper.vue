<template>
  <div class="wrapper">

    <!--Версия для мобильных телефонов, начало-->

    <div class="mobile-title" @click="openMobile()">
      <div class="string1"></div>
      <div class="string2"></div>
      <div class="string3"></div>
    </div>
    <div class="mobile-open-menu" v-show="mobileOpen">
      <div class="cross-block">
        <div class="mobile-cross" @click="closeMobile()">
        </div>
      </div>
      <div class="add-product mobile-block" @click="formVisibility = ! formVisibility">
        Добавить товар
      </div>
      <div class="mobile-price-max mobile-block"
      @click="mobilePriceMax()">
        {{priceToZero}}
      </div>
      <div class="mobile-price-min mobile-block"
      @click="mobilePriceMin()">
        {{priceFromZero}}
      </div>
      <div class="mobile-name-filter mobile-block"
      @click="mobileTitle()">
        {{sortByTitle}}
      </div>
    </div>

    <!--Десктопная версия, начало -->

    <div class="title">
      Добавление товара
    </div>
    <div class="filter"
    @click="isOpen = !isOpen"
    >
      {{defaultSort}}
          <div class="svg">
      <svg width="8" height="6" viewBox="0 0 8 6" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M7.48532 1.24264L4.24268 4.48528L1.00003 1.24264" stroke="#B4B4B4"/>
      </svg>
          </div>
    </div>
    <div class="categories"
    v-show="isOpen"
    >
      <div class="price-max filter-box" @click="openPriceMax()">
        {{priceToZero}}
      </div>
      <div class="price-min filter-box" @click="openPriceMin()">
        {{priceFromZero}}
      </div>
      <div class="name-filter filter-box" @click="openTitle()">
        {{sortByTitle}}
      </div>
    </div>
    <container ref="contain"/>
  </div>
</template>
<script>
export default {
  data(){
    return{
      formVisibility: false,
      mobileOpen:false,
      isOpen: false,
      defaultSort:'По умолчанию',
      priceFromZero:'По цене от 0',
      priceToZero:'По цене до 0',
      sortByTitle:'По наименованию',
    }
  },
  methods:{
    // Функции для десктопной версии приложения, начало
    openPriceMax(){
      this.$refs.contain.maxminfunc()
      let sortedValue = document.querySelector('.filter')
      sortedValue.textContent = this.priceToZero
      this.isOpen = !this.isOpen
    },
    openPriceMin(){
      this.$refs.contain.minmaxfunc()
      let sortedValue = document.querySelector('.filter')
      sortedValue.textContent = this.priceFromZero
      this.isOpen = !this.isOpen
    },
    openTitle(){
      this.$refs.contain.titlefunc()
      let sortedValue = document.querySelector('.filter')
      sortedValue.textContent = this.sortByTitle
      this.isOpen = !this.isOpen
    },
    // Функции для мобильной версии приложения, начало
    openMobile(){
      let menu = document.querySelector('.mobile-title')
      menu.style.visibility = 'hidden'
      menu.style.position = 'absolute' 

      this.mobileOpen = !this.mobileOpen
    },
    closeMobile(){
      let menu = document.querySelector('.mobile-title')
      menu.style.visibility='visible'
      menu.style.position = 'relative'

      this.mobileOpen = !this.mobileOpen
    },
    mobilePriceMax(){
      let menu = document.querySelector('.mobile-title')
      menu.style.visibility='visible'
      menu.style.position = 'relative'
      this.$refs.contain.maxminfunc()
      let sortedValue = document.querySelector('.filter')
      sortedValue.textContent = this.priceToZero
      this.mobileOpen = !this.mobileOpen
    },
    mobilePriceMin(){
      let menu = document.querySelector('.mobile-title')
      menu.style.visibility='visible'
      menu.style.position = 'relative'
      this.$refs.contain.minmaxfunc()
      let sortedValue = document.querySelector('.filter')
      sortedValue.textContent = this.priceFromZero
      this.mobileOpen = !this.mobileOpen
    },
    mobileTitle(){
      let menu = document.querySelector('.mobile-title')
      menu.style.visibility='visible'
      menu.style.position = 'relative'
      this.$refs.contain.titlefunc()
      let sortedValue = document.querySelector('.filter')
      sortedValue.textContent = this.sortByTitle
      this.mobileOpen = !this.mobileOpen
    },
    mobileAdd(){
      let menu = document.querySelector('.mobile-title')
      menu.style.visibility='visible'
      menu.style.position = 'relative'
      this.formVisibility = ! this.formVisibility
      this.mobileOpen = !this.mobileOpen
    },
  }
}
</script>
<style lang="css" scoped>
.wrapper{
  font-family: 'Source Sans Pro', sans-serif;
  padding: 32px 32px 32px 32px;
  background: #E5E5E5;
  color: #3F3F3F;
  margin:0px;
  min-height: 100vh;

}
.title{
  
  position: absolute;
  top:32px;
  left:32px;
  font-style: normal;
  font-weight: 600;
  font-size: 28px;
  line-height: 35px;
}
.filter{
  position: absolute;
  right: 32px;
  top: 31px;
  width: 121px;
  height: 36px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #B4B4B4;
  font-family: Source Sans Pro;
  font-style: normal;
  font-weight: normal;
  font-size: 12px;
  line-height: 15px;
  background: #FFFEFB;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
  cursor: pointer;
  z-index: 99;
}
.filter:hover{
  background: #e6e5e2;
}
.svg{
  padding-left: 5px;
}
.categories{
  
  position: absolute;
  right:32px;
  top:67px;
  background: #FFFEFB;
  width: 121px;
  height: 108px;
  font-family: Source Sans Pro;
  font-style: normal;
  font-weight: normal;
  font-size: 12px;
  line-height: 15px;
  color: #B4B4B4;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
  display: flex;
  flex-direction: column;
  z-index: 80;
}

.filter-box{
  height: 36px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
.filter-box:hover{
  background: #dbdbd9;
  color:#3F3F3F;
}

/* Мобильная версия приложения начало*/

@media (max-width: 415px){
  .filter{
    visibility: hidden;
    z-index: 0;
  }

  .wrapper{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding:0;
    background: #FFFEFB;
  }
  .title{
    visibility: hidden;
  }
  .mobile-title{
    margin-top:20px;
    width:332px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    
    z-index: 99;
    background: #FFFEFB;
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
    border-radius: 4px;
    cursor: pointer;
  }
  .string1{
    background: #3F3F3F;
    width: 40px;
    height: 3px;
    margin-bottom:5px;
    margin-top:18px;
    border-radius: 5px;
  }
  .string2{
    background: #3F3F3F;
    width: 40px;
    height: 3px;
    margin-bottom: 5px;    
    border-radius: 5px;
  }
  .string3{
    background: #3F3F3F;
    width: 40px;
    height: 3px;
    margin-bottom: 18px;
    border-radius: 5px;
  }
  .mobile-open-menu{
    width: 332px;
    display: flex;
    flex-direction: column;
    z-index: 100;
    background: #FFFEFB;
    margin: 0;
    margin-top:20px;
    box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.1);
    border-radius: 4px;
  }
  .mobile-block{
    display: flex;
    justify-content: center;
    margin-top: 8px;
    padding-bottom: 8px;
  }
  .mobile-block:hover{
    background: #dad9d7;
  }
  .cross-block{
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    padding-top: 18px;
    padding-bottom: 18px;
    cursor: pointer;
  }
  .mobile-cross{
    margin-right: 10%;
    padding-top: 8px;
    position: relative;
  }
  .mobile-cross:before {
    content: "";
    position: absolute;
    -webkit-transform: rotate(45deg);
    width: 40px;
    height: 3px;
    background-color: #3F3F3F;
    transform: rotate(45deg);
  }

  .mobile-cross:after {
    content: "";
    position: absolute;
    -webkit-transform: rotate(-45deg);
    width: 40px;
    height: 3px;
    background-color: #3F3F3F;
    transform: rotate(-45deg);
  }

}
</style>
