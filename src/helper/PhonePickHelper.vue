<template>
    <div class="phonePickHelper">
        
        <VueCountryDropDown
    @onSelect="onSelect"
   
    :preferredCountries="['RU']"
    :disabledFetchingCountry="true"
    :immediateCallSelectEvent="true"
    :enabledFlags="true"
    :enabledCountryCode="true"
    :showNameInput="false"
    :showNotSelectedOption="true"
    class="countryDropDown"></VueCountryDropDown>
    <input v-if="showInput" type="text" ref="inputPhone" v-model="dialNumber" v-mask="'(___) ___ __ __'"
    placeholder="Введите номер телефона" class="phoneNumber" >
    <div v-else @click="showHandler" class="phoneNumber">Введите номер телефона*</div>
    </div>
</template>
<script >

import VueCountryDropDown from 'vue-country-dropdown'

export default{

    name:'phone-pick-helper',
    data(){
        return{
            showInput:0,
            dialCode: 0,
            dialNumber:''
        }
    },
    computed:{
        formatedDialNumber:function(){
            
return this.dialNumber.replace(/[(,),\s,_]+/g,'').slice(0,10)
        }
    },
    methods:{
        onSelect({dialCode:dl}){
            this.dialCode = dl
        },
        showHandler(){
            this.showInput = true;
            setTimeout(()=>this.$refs.inputPhone.focus(),100)
        }
    },
    components:{VueCountryDropDown},
    directives:{
        ['mask']: {
        inserted: function (el, binding) {
          
          let mask = binding.value,
              first = mask.indexOf('_'),
             
              clean = mask.replace(/[^0-9_]/gm, ''),
              indexes = []
          for(let i = 0; i < clean.length; i++){
            if(!isNaN(clean[i])){
              indexes.push(i)
            }
          }
          el.value = mask
          function maskIt(){
            let value = el.value,
                filtred = value.replace(/[^0-9]/gm, ''),
                result = ''
            if(value.length < first){
              value = mask + value
              filtred = value.replace(/[^0-9]/gm, '')
            }
            for(let i = 0; i < filtred.length; i++){
              if(indexes.indexOf(i) == -1){
                result += filtred[i]
              }
            }
            value = ''
            let cursor = 0
            for(let i = 0; i < mask.length; i++){
              if(mask[i] == '_' && result){
                value += result[0]
                result = result.slice(1)
                cursor = i + 1
              }else{
                value += mask[i]
              }
            }
            if(cursor < first){
              cursor = first
            }
            el.value = value
            el.setSelectionRange(cursor,cursor)
          }
          el.addEventListener('focus', function(event){
            event.preventDefault()
          })
          el.addEventListener('click', function(event){
            event.preventDefault()
            let start = el.value.indexOf('_')
            
            if(start == -1){
              start = el.value.length
            }
            el.setSelectionRange(start,start)
          })
          el.addEventListener('paste', function(){
            let start = el.selectionStart
            if(start !=null)
            if(start < first){
              el.value = '_' + el.value
            }
          })
          el.addEventListener('input', function(event){
               
            maskIt()
          })
        }
    }
},
}
</script>
<style lang="scss" scoped>
.phonePickHelper{
  display: flex;
  margin-top: 5px;
  
}
.phoneNumber{
  width: 370px;
  display: flex;
  align-items:center;
  justify-content: start;
  padding: 0;
  padding-left: 10px;
  border:1px solid #dcdfe6;
  color:#dcdfe6
}

.countryDropDown:deep{
  position: relative;
  height: 40px;
    &::before{
      position: absolute;
        content: 'Страна';
        font-family: var(--font-family);
      font-weight: 400;
    font-size: 8px;
    left: 5px;
    letter-spacing: -0.02em;
    color: #c0c4cc;
    }
    &>div{
      padding:0;
      &:hover{
        background-color:transparent;
      }
    }
    
}

</style>