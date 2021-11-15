<template>
    <div class="preloader" v-show="preloaderVisible">
        <div class="title-preloader">
            Приложение для добавления товара  
        </div>
        <div class="animated-blocks">
            <div class="animated-block number1"></div>
            <div class="animated-block number2"></div>
            <div class="animated-block number3"></div>
            <div class="animated-block number4"></div>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return {
            preloaderVisible: true
        }
    },
    methods:{
        showPreloader(){
            setTimeout(()=> {
                this.preloaderVisible = false
            },2000)
        }
    },
    mounted(){
        this.showPreloader()
    }
}
</script>
<style lang="css" scoped>


.preloader{
    color: #B4B4B4;
    font-size: 40px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    font-family: 'Poiret One', cursive;
    position:fixed;
    overflow: hidden;
    width: 100vw;
    height: 100vh;
    z-index: 9999;
    background: #FFFEFB;
    top:0;
    left:0;
}
.title-preloader{
    padding-left:40px;
}
.animated-blocks{
    display: flex;
    flex-direction: row;
}
.animated-block{
    background: #B4B4B4;
    width:30px;
    height: 30px;
    border-radius:5px;
    margin-left:10px;
    margin-top: 20px;
}
.number1{
    animation: loading 1s ease-in;
}
.number2{
    animation: loading 1.3s ease-in;
}
.number3{
    animation: loading 1.6s ease-in;
}
.number4{
    animation: loading 2s ease-in;
}

@keyframes loading{
    from {
        width: 1px;
        height: 1px;
    }
    15%{
        width:10px;
        height: 10px;
    }
    30%{
        width:30px;
        height:30px;
    }
    45%{
        width:50px;
        height: 50px;
    }
    60%{ width:80px;
        height:80px;
    }
    75%{ width: 70px;
        height:70px;
    }
    90%{
        width:60px;
        height: 60px;
    }
    to{
        width: 30px;
        height:30px;
    }
}

</style>