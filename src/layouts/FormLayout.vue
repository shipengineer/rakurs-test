<template>
    <div class="layout">
        <div class="bookmarks" >
            <button v-for="mark in bookmarks" class="bookmark" @click="changeBookmark(mark.num)" :class="{checkedBookmark:bookmark==mark.num}" :key="mark.num">{{ mark.title }}</button>
            
        </div>
        <ActivationForm v-if="bookmark==1"/>
        <div v-if="bookmark==2" >Здесь все кому передали доступ</div>
        <div class="formFooter">
            <p class="formFooter__firstLine">Получите бесплатную консультацию - свяжитесь с нами удобным способом: <a href="tel::+79214377886"><img src="@/assets/phone.svg">+7 (921) 437-78-86</a> <a href="vk"><img src="@/assets/vk.svg"></a><a href="whatsapp"><img src="@/assets/wup.svg"></a><a href="telegram"><img src="@/assets/tg.svg"></a></p>
            <p class='formFooter__secondLine'><a href="widjets">Все виджеты Rakurs</a><a href="instruction">Инструкция к виджету</a><a href="restore">Восстановить доступ</a> <img src="@/assets/rakurs_logo.svg"></p>
        </div>
    </div>
</template>
<script >
import ActivationForm from '@/components/ActivationForm.vue'
export default {
    name:'FormLayout',
    components:{
        ActivationForm
    },
    data(){
        return{
            bookmark:1,
            bookmarks:[
                {num:1,title:"Описание"},
                {num:2,title:"Выданные доступы"},
            ]

        }
    },
    methods:{
        changeBookmark(payload){
            this.bookmark=payload
        }
    }
}
</script>
<style lang="scss">
.layout{
width: 733px;
height: 896px;
display: flex;
flex-direction: column;
align-items: center;
}
.bookmarks{
    align-self: flex-start;
    .bookmark{
    background-color: transparent;
    border: 0;
    border-radius: 0;
    width: fit-content;
    border-bottom: 1px solid #2f80ed;
    &.checkedBookmark{
        color: black;
        border-width: 3px;
    }
    }
}
.formFooter{
font-family: var(--font-family);
font-weight: 400;
font-size: 12px;
line-height: 120%;
color: #303133;
a{
    font-family: var(--font-family);
font-weight: 400;
font-size: 12px;
line-height: 120%;
color: #17505b;
img{
    margin-left: 6px;
}
}
&__firstLine{
    display: flex;
    align-items: center;
    img{
        position: relative;
        top: 2px;
        &:first-child{
            margin-right: 5px;
        }
    }
}
&__secondLine{
    display: flex;
    gap: 20px;
    justify-content: start;

    img{
        margin-left: auto;
    }
}

}


</style>