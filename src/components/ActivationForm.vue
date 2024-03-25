<template>
    <div class="form">
        <p class="form__thx"> Спасибо за установку!</p>
        <p class="form__lastStep">Остался последний шаг</p>
        <form  class="activation" >
            <h3 class="activation__title">Активация тестового доступа</h3>
            <p class="activation__condition">Для активации 14-дневного тестового доступа заполните форму ниже.</p>
            <p class="activation__attention"> 
                <strong>Внимание!</strong> В целях безопасности обслуживание будет осуществляться только по указанному номеру телефона. </p>
            
            <PhonePickHelper @input-phone="setPhone" :class="{error:errorPhone}" />
            <input class="activation__userName" @focus='(e)=>e.target.classList.remove("error")' type="text" v-model = 'userName' ref="nameToSend" placeholder="Как к Вам обращаться?*">
            
            <select class="activation__userStatus" @focus='(e)=>e.target.classList.remove("error")' type="select" ref="statusToSend" v-model = 'userStatus' >
            <option disabled hidden  value="null">Ваша роль в компании?*</option>
            <option  value="owner">Собственник</option>
            <option value="head-of-sales">Руководитель отдела продаж</option>
            <option value="technician">Технический специалист</option>
            <option value="integrator">Интегратор amoCRM</option>
        </select>
        <div class="checkBoxes">
            <label :class="{checked:freeHelp}" class="checkBoxes__freeHelp">
                <img v-if='freeHelp' src="@/assets/checked.svg" >
                <img v-else src="@/assets/unchecked.svg" >

                <input type="checkbox" style="display: none;"  v-model="freeHelp">Хочу получить бесплатную помощь в настройке
            </label>
            <label class="checkBoxes__promo" :class="{checked:promoCheck}" @click="focusPromo">
                <img v-if='promoCheck' src="@/assets/checked.svg" >
                <img v-else src="@/assets/unchecked.svg" >
                <input type="checkbox"  v-model="promoCheck" style="display: none;">У меня есть промокод</label>
                <input   v-if="promoCheck" v-model="promoText" ref="promoInputText" type="text" class="checkBoxes__promoNumber" /></div>
        <button class="activation__submitButton" @click="sendForm">Активировать доступ</button>
        <p class="activation__questions">Есть вопросы? <a href="mailto::example@email.com">Напишите нам!</a></p>
    </form>
    <div class="send" :class="{hide:isHide}">
        Данные успешно отправлены
    </div>
    </div>
</template>
<script>
import PhonePickHelper from '@/helper/PhonePickHelper.vue'
export default{
    name:'ActivationForm',
    components:{
        PhonePickHelper,
       
    },
    
    data(){
        return {
            userName:'',
            userStatus:'null',
            promoCheck:false,
            freeHelp:true,
            promoText:'',
            phoneNumber:'',
            errorPhone:false,
            fetchError:'',
            isHide:true
        }
    },
    methods:{
        focusPromo(){
            if(!this.promoCheck)
                
               setTimeout(()=>this.$refs.promoInputText.focus(),100)
        
    },
    setPhone(payload){
        this.errorPhone=false
        this.phoneNumber=payload
    },
    async sendForm(e){
        e.preventDefault()
        if(this.userName.trim()==''){
            this.$refs.nameToSend.classList.add('error')
            
        }
        if(this.userStatus=='null'){
            this.$refs.statusToSend.classList.add('error')
        }
        if(this.phoneNumber.trim().length<11)
        {
            this.errorPhone=true
        }
       if(this.userName.trim()==''||this.userStatus=='null'||this.phoneNumber.trim().length<11){
        return
       }
       this.isHide=false
       setTimeout(()=>{this.isHide=true},4000)
      //Если все удачно, отправляем данные на сервер
      //Можно мокнуть jest
      /*
       fetch( 'someURl', {
    method: 'POST',
    headers: {
        'Authorization': this.token,
        'Accept': 'application/json',
        'Content-Type': 'application/json;charset=utf-8'
    },
    body: JSON.stringify( {phone:this.phoneNumber,name:this.userName,status:this.userStatus,isHelp:this.freeHelp,isPromo:this.promoCheck,promoCode:this.promoText} )
} )
.then( function( response ){
    if( response.status != 201 ){
        this.fetchError = response.status;
    }else{
        response.json().then( function( data ){
            this.fetchResponse = data;
        }.bind(this));
    }
}.bind(this));

*/
}

}
}
</script>
<style lang="scss">
.send{
    width: 250px;
    height: 250px;
    background-color: grey;
    display: flex;
    align-items: center;
    justify-content: center;
    position: absolute;
    top: 70%;
    right: 40%;
    opacity: 1;
    transition: 2s;
}
.hide{
    width: 250px;
    height: 250px;
    background-color: grey;
    position: absolute;
    top: -50%;
    right: 40%;
    opacity: 0;
    transition: 2s;
    
}
.error{
    border:1px solid #eb5757;
}
.checked{
    font-weight: 600;
}

.form{
    
    display: flex;
flex-direction: column;
align-items: center;

&__thx{
    margin: 0;
    margin-top: 40px;
    font-family: var(--font-family);
font-weight: 600;
font-size: 24px;

color: #303133;
}
&__lastStep{
    margin: 0;
    font-family: var(--font-family);
font-weight: 600;
font-size: 16px;
line-height: 100%;
color: #17505b;
}
}
.activation{
    
    width: 500px;
    height: 600px;
    margin: 0;
    margin-top: 40px;
border-radius: 4px 4px 0 0;
border: 2px solid #bacbce;
display: flex;
flex-direction: column;
align-items: center;

padding: 0 30px;
&__title{
 width: calc(100% + 60px);
text-align: center;
padding: 16px 0;
font-weight: 600;
font-size: 24px;
color: #303133;
background-color: #bacbce;
}
&__condition{
    margin-top: 30px;
    margin-top: 40px;
    font-size: 16px;
    font-weight: 600;


}
&__attention{
    strong{
        font-family: var(--font-family);
    }
    padding: 8px 12px;
    color: #17505b;
    background-color: #e8eeef;
    border-radius: 4px;
    font-size: 12px;

}
&__userName, &__userStatus{
    margin-top: 10px;
    width: 440px;
    padding:13px 15px;

}
&__submitButton{
    margin-top: 30px;
        border-radius: 4px;
padding: 8px;
width: 440px;
height: 40px;
    }
    &__questions{
       text-align: center;
        
        font-size: 12px;
        a{
            font-weight: 600;
        }
    }
}
.checkBoxes{
    letter-spacing: 1.25;
    color: #17505b;
    font-size: 14px;
   margin-top: 20px;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    gap: 10px;
    align-items: center;
    img{
        margin-right: 10px;
    }
    &__freeHelp{ 
        
        display: flex;
        align-items: center;
        }
    &__promo{
        display: flex;
        align-items: center;
        height: 40px;

    }
    &__promoNumber{
       
        display: flex;
        align-items: center;
       
        height: 32px;
    }
   
}
</style>