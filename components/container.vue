<template>
    <div class="container-wrapper">

        <!--Мобильная версия приложенияб форма для заполнения начало -->
    <div class="mobile-form-wrapper" v-show="$parent.formVisibility">
        <div class="mobile-form" >
            <div class="form1">
                <div class="title-mobile-form">
                    Наименование товара
                    <div class="red-circle1"></div>
                </div>
                <input class="form-small" type="text" 
                placeholder="Введите наименование товара"
                v-model="title"
                :class = "{error: ($v.title.$dirty && !$v.title.required)}" 
                >
                <small v-if="$v.title.$dirty && !$v.title.required"
                class="small invalid">Поле является обязательным</small>
            </div>
            <div class="form1">
                Описание товара
                <textarea class="form-big" type="text" placeholder="Введите описание товара"
                v-model="description" 
                ></textarea>
            </div>
            <div class="form1">
                <div class="image-mobile">
                    Ссылка на изображение товара
                    <div class="red-circle2"></div>
                </div>
                <input class="form-small" type="text" 
                placeholder="Введите ссылку"
                v-model="image"
                :class = "{error: ($v.image.$dirty && !$v.image.required)}" 
               >
               <small v-if="$v.image.$dirty && !$v.image.required"
                class="small invalid">Поле является обязательным</small>
            </div>
            <div class="form1">
                <div class="price-mobile">
                    Цена товара
                    <div class="red-circle3"></div>
                </div>
                <input class="form-small" type="text" 
                placeholder="Введите цену"
                v-model="price"
                :class = "{error: ($v.price.$dirty && !$v.price.required)}" 
                >
                <small v-if="$v.price.$dirty && !$v.price.required"
                class="small invalid">Поле является обязательным</small>
            </div>
            <button class="form-button" @click="addProd()"
            :class="{okForm: ($v.price.required && $v.image.required && $v.title.required)}"
            >Добавить товар</button>
        </div>
    </div>
        <!--Десктопная версия формы для заполнения начало -->
        <form class="menu">
            <div class="form1">
                Наименование товара
                <div class="red-circle1"></div>
                <input class="form-small" type="text" 
                placeholder="Введите наименование товара"
                v-model="title" 
                :class = "{error: ($v.title.$dirty && !$v.title.required)}"
                >
                <small v-if="$v.title.$dirty && !$v.title.required"
                class="small">Поле является обязательным</small>
            </div>
            <div class="form1">
                Описание товара
                <textarea class="form-big" type="text" placeholder="Введите описание товара"
                v-model="description" 
                ></textarea>
            </div>
            <div class="form1">
                <div class="img-product">
                    Ссылка на изображение товара
                    <div class="red-circle2"></div>
                </div>
                <input class="form-small" type="text" 
                placeholder="Введите ссылку"
                v-model="image" 
                :class = "{error: ($v.image.$dirty && !$v.image.required) }"
               >
               <small v-if="$v.image.$dirty && !$v.image.required"
                class="small invalid">Поле является обязательным</small>
            </div>
            <div class="form1">
                <div class="price-product">
                    Цена товара
                    <div class="red-circle3"></div>
                </div>
                
                <input class="form-small" type="text" 
                placeholder="Введите цену"
    
                :class = "{error: ($v.price.$dirty && !$v.price.required) }"
                v-model="price"
            
                >
                <small v-if="$v.price.$dirty && !$v.price.required"
                class="small invalid">Поле является обязательным</small>
            </div>
            <button class="form-button" type="button" @click="addData()"
            :class="{okForm: ($v.price.required && $v.image.required && $v.title.required)}"
            >Добавить товар</button>
        </form>
        <div class="cards-container">
            <ul class="cards-container-ul">
            <li class="cards-container-ul">
            <card
            v-for="(product,index) in products"
            :key = "product.id"
            @deLida="deLida(index)"
            :title="product.title"
            :description="product.description"
            :image="product.image"
            :price="product.price"
            />
            </li>
            </ul>
        </div>
        <div class="card-done" @click="cardDone = !cardDone"
        v-show="cardDone"
        >
            <div class="card-done-good">
                <div class="card-done-good-img">
                    <div class="img-good-card">
                        <img :src="image" width="332px"
                        height="200px">
                    </div>
                    <div class="title-good-card">{{title}}</div>
                    <div class="description-good-card">{{description}}</div>
                    <div class="price-good-card">{{price}}руб.</div>
                </div>
                <div class="card-done-good-text">
                   Новая карточка продукта!
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import {required} from 'vuelidate/lib/validators'
export default {
    
    data(){
        return{
            submitStatus: null,
            cardDone: false,
            
            title:'',
            description:'',
            image:'',
            price:null,
            productData:{},
            sorted:'',
            products:[]      
        }
    },
    validations:{
        title:{
            required  
        },
        image:{
            required
        },
        price:{
            required
        }
    },
    mounted(){
        const data = localStorage.getItem('products')
        if(data){
        this.products = JSON.parse(data)
        }
        
    },
    
    methods:{
        
        titlefunc() {
            function compare(a, b) {
                if (a.title < b.title){
                    return -1;}
                if (a.title > b.title){
                    return 1;}else{
                return 0;}
                    }
            return this.products.sort(compare);
        },
        minmaxfunc(){
            return this.products.sort(function(a, b) {
                        return a.price - b.price;
                    });     
        },
        maxminfunc(){
            return this.products.sort(function(a,b){
                return b.price-a.price
            })
        },
        addData(){
            if(this.$v.$invalid){
                this.$v.$touch()
                return
            }

            this.productData = {
                
                id: this.products.length+1,
                title: this.title,
                description:this.description,
                image:this.image,
                price:this.price
            }
            this.products.push(this.productData)
            console.log(this.products)
            this.cardDone = !this.cardDone

            localStorage.setItem('products', JSON.stringify(this.products))

            setTimeout(() => {
                this.title = ''
                this.price = ''
                this.description =''
                this.image = ''
                this.cardDone = false
            }, 1500)
        
        
        },
        deLida(index){
            this.products.splice(index,1) 
            localStorage.setItem('products', JSON.stringify(this.products))
        },
        addProd(){
            if(this.$v.$invalid){
                this.$v.$touch()
                return
            }
            
            let menu = document.querySelector('.mobile-title')
            menu.style.visibility='visible'
            menu.style.position = 'relative'
            this.$parent.formVisibility = ! this.$parent.formVisibility
            this.$parent.mobileOpen = !this.$parent.mobileOpen
            this.productData = {
                
                id: this.products.length+1,
                title: this.title,
                description:this.description,
                image:this.image,
                price:this.price
            }
            this.products.push(this.productData)
            console.log(this.products)
            this.cardDone = !this.cardDone

            localStorage.setItem('products', JSON.stringify(this.products))
            setTimeout(() => {
                this.title = ''
                this.price = ''
                this.description =''
                this.image = ''
                this.cardDone = false
            }, 1500)
            

        },
        borderErr(){
            let form = document.querySelector('.form-small')
            form.style.border='#FF8484'
        }
    },
    
}
</script>
<style lang="css" scoped>
/*Всплывающее сообщение при ошибках */
.small{
    color:#FF8484
}


/*Всплывающее сообщение при ошибках */
.container-wrapper{
    
    display: flex;
    flex-direction: row;
}
.menu{
    position: fixed;
    left: 32px;
    top: 83px;
    padding-top:8px;
    display: flex;
    flex-direction: column;
    background: #FFFEFB;
    
    width: 332px;
    box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
    border-radius: 4px;
    font-style: normal;
    font-weight: normal;
    font-size: 10px;
    line-height: 13px;
    align-items: center;

    letter-spacing: -0.02em;
}
.red-circle1{
    position: absolute;
    width: 4px;
    height: 4px;
    left: 117px;
    top: 25px;
    background: #FF8484;
    border-radius: 4px;
}
.red-circle2{
    position: absolute;
    width: 4px;
    height: 4px;
    bottom:8px;
    left:133px;
    background: #FF8484;
    border-radius: 4px;
}
.red-circle3{
    position: absolute;
    width: 4px;
    height: 4px;
    bottom:8px;
    left:53px;
    background: #FF8484;
    border-radius: 4px;
}
.form1{
    display: flex;
    flex-direction: column;
    margin-top:16px;
}
.img-product{
    position: relative;
}
.price-product{
    position:relative;
}
.form-small{
    padding-left: 16px;
    width: 268px;
    height: 36px;
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
    border-radius: 4px;
    border-style: solid;
    border-color: #FFFEFB;
    border-width: 1px;
    margin-top: 4px;
    font-family: Source Sans Pro;
    font-style: normal;
    font-weight: normal;
    font-size: 12px;
    line-height: 15px;  
}
.error{
    border-color: #FF8484;
}
.form-big{
    padding-left: 16px;
    resize: none;
    width: 268px;
    height: 108px;
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
    border-radius: 4px;
    border: none;
    margin-top: 4px;
    font-family: Source Sans Pro;
    font-style: normal;
    font-weight: normal;
    font-size: 12px;
    line-height: 15px;

    
}
.form-button{
    width: 284px;
    height: 36px;

    border:none;
    background: #EEEEEE;
    border-radius: 10px;
    font-family: Inter;
    font-style: normal;
    font-weight: 600;
    font-size: 12px;
    line-height: 15px;

    text-align: center;
    letter-spacing: -0.02em;

    margin-top: 24px ;
    margin-bottom: 24px;

    color: #B4B4B4;
    cursor: pointer;
}
.okForm{
    background: #7BAE73;
    color: #FFFFFF;
}
.cards-container{
    margin-top: 43px;
    padding: 0;
    display: flex;
    flex-wrap: wrap;
    margin-left: 340px;
    max-width: 1500px;
}
.cards-container-ul{
    padding-top:0;
    margin-top:0;
    padding: 0;
    display: flex;
    flex-wrap: wrap;
}
li {
  list-style-type: none;
  /* Убираем маркеры */
}

ul {
  margin-left: 0;
  /* Отступ слева в браузере IE и Opera */
  padding-left: 0;
  /* Отступ слева в браузере Firefox, Safari, Chrome */
}


/* После успешного заполнения формы и нажатия на кнопку */

.card-done{
    left:0;
    top:0;
    display: flex;
    justify-content: center;
    align-items: center;
    position: fixed;
    z-index: 100;
    background: #FFFEFB;
    height: 100%;
    width: 100%;
    cursor: pointer;
    font-style: normal;
    font-weight: 600;
    font-size: 28px;
    line-height: 35px;
    font-family: 'Source Sans Pro', sans-serif;
    padding: 0;
    margin: 0;
    box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
    border-radius: 4px;
}
.card-done-good-text{
    margin-top:20px;
    color:#FF8484;
}

.card-done-good-img{
    width: 332px;
    position: relative;
    display: flex;
    flex-direction: column;
    background: #FFFEFB;
    box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
    border-radius: 4px;
    animation: GoodJob 1s ease-in-out;
}
@keyframes GoodJob{
    from {
        visibility: hidden;
        left:-100%;
        width:5px;
    }
    15%    {
        
        width:10px;
    }
    25%{
        width:50px;
    }
    50%{
        width:100px;
    }
    to {
        left:0
    }
}
.title-good-card{
    margin-left: 16px;
    margin-right: 16px;
    margin-top: 16px;
    font-family: Source Sans Pro;
    font-style: normal;
    font-weight: 600;
    font-size: 20px;
    line-height: 25px;
}
.description-good-card{
    margin-top: 16px;
    margin-left: 16px;
    margin-right: 16px;
    padding-bottom: 86px;
    font-family: Source Sans Pro;
    font-style: normal;
    font-weight: normal;
    font-size: 16px;
    line-height: 20px;
}
.price-good-card{
    position: absolute;
    left:16px;
    bottom: 24px;
    font-family: Source Sans Pro;
    font-style: normal;
    font-weight: 600;
    font-size: 24px;
    line-height: 30px;
}
.img-good-card{
    border-top-left-radius: 4px;
    border-top-right-radius: 4px;
    overflow: hidden;
}

@media (max-width: 415px){
    .menu{
        visibility:hidden
    }
    .container-wrapper{
        display: flex;
        flex-direction: column;
    }
    .cards-container{
        display: flex;
        justify-content: center;
        align-items: center;
        padding-left: 0;
        margin-left: 0;
    }
    .cards-container-ul{
        justify-content: center;
        align-items: center;
        margin-left: 0;
    }
    .mobile-form-wrapper{
        top:0;
        left:0;
        position: fixed;
        overflow: auto;
        z-index: 999;
        background: #FFFEFB;
        height: 100vh;
        width: 100vw;
        align-content: center;
        padding: 0 auto;
        margin: 0 auto;
    }
    .mobile-form{
        
        margin-top:120px;
        margin-left:40px;
        position: absolute;
        
    
        background: #FFFEFB;
    }
    .title-mobile-form{
        position: relative;
    }
    .image-mobile{
        position: relative;
    }
    .price-mobile{
        position: relative;
    }
    .red-circle1{
        position:absolute;
        top:4px;
        left:156px;
    }
    .red-circle2{
        bottom: 12px;
        left:221px;
    }
    .red-circle3{
        bottom:12px;
        left:87px;
    }
}

</style>