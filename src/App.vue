<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup

import '../src/utils/tagcanvas.min.js'
import { nextTick, onMounted, ref } from 'vue';
const names = ref('')
const tags = ref([])
const groupa = ref([])
const groupb = ref([])
const total = ref(0)
const mylabels = ref([])
const namechange = (val)=>{
  //  let arr = names.value.split(' ')
  // let arr2 = arr.filter((v)=>{
  //   return v && v.trim()
  //  })
  //  tags.value = arr2
  //  nextTick(()=>{
  //   TagCanvas.Update("myCanvas",'tags',{
  //     initial:[0,1]
  //   })
  //  })
}
const gogroup = ()=>{
  console.log(Math.floor(Math.random()*6+0))
  let arr = names.value.split(' ')
  let arr2 = arr.filter((v)=>{
    return v && v.trim()
   })
   tags.value = arr2
   total.value = arr2.length
   mylabels.value=initlabelsArr()
   nextTick(()=>{
    TagCanvas.Update("myCanvas",'tags',{
      initial:[0,1]
    })
   })
  groupb.value=[]
  groupa.value=[]
  let tagsbackup = tags.value
  tagsbackup = randomArr(tagsbackup)
   for(let i in tagsbackup)
   {
    let item = tagsbackup[i]
    getButton(item,i)
   }
}
const initlabelsArr=()=>{
   let arr=[]
   let pergruop = Math.floor(total.value/2)
   for(let i=0;i<total.value;i++)
   {
   
     if(i==total.value-1 && i%2==0)
     {
      continue
     }
     let key = i>=pergruop  ? 'A':'B'
     arr.push(key)
   }
   return randomArr(arr)
}
const randomArr=(arr)=>{
        let length= arr.length;
        //while执行至条件不成立则跳出循环
        while(length > 1){
            // --length 先自减再执行表达式
            // length-- 先执行表达式再自减
            //length 先执行random和floor得到index后再自减
            let index = Math.floor(Math.random() * length--);
            //es6的解构赋值，等号的左右两边模式相同，就会将右边的值赋给左边的变量
            [arr[length], arr[index]] = [arr[index], arr[length]];
        }
        return arr;
    }
const getButton=(label,i)=>{
  setTimeout(()=>{
      let item = label
      tags.value= tags.value.filter((v)=>{
        return v != item
      })
      nextTick(()=>{
            TagCanvas.Update("myCanvas",'tags',{
              initial:[0,1]
            })
          })
      if(myrandom(i)){
        groupb.value.push(item)
      }else{
        groupa.value.push(item)
      }
     },1500*i)
}
const myrandom = (peopleindex)=>{
  if(peopleindex==total.value-1 && peopleindex%2==0)
  {
    let num = Math.floor(Math.random()*1000+1)
    return num%2==0
  }
  let labels =mylabels.value 
  labels = randomArr(labels)
  console.log('peopleindex',peopleindex)
  console.log(labels)
  let max = labels.length
  let randomindex = Math.floor(Math.random()*max+1)-1
  let item = labels[randomindex]
  mylabels.value.splice(randomindex,1)
  //console.log('labels',mylabels.value)
  return item == 'A'
 
}
onMounted(()=>{
  TagCanvas.Start('myCanvas','tags',{
            textColour: 'yellow',
            outlineColour: '#ff00ff',
            reverse: true,
            depth: 0.8,
            textHeight: 50,
            maxSpeed: 0.05,
            minSpeed: 0.05, 
           
          });
})
</script>

<template>
  <div class="wrap">
     <div class="namewrap">
      <el-input v-model="names" placeholder=""  clearable @keyup.enter.native="namechange"></el-input>
      <el-button type="primary"  @click="gogroup">分组</el-button>
      
     </div>
     <div id="myCanvasContainer">
      <canvas width="800" height="500"  id="myCanvas">
        <p>Anything in here will be replaced on browsers that support the canvas element</p>
      </canvas>
    </div>
    <div id="tags">
      <ul>
        <li v-for="(item,index)  in tags" :key="index"><a>{{item}}</a></li>
      </ul>
    </div>
    <div class="group">
      <div class="listwrap">
        <div>第一组</div>
        <div v-for="(item,index) in groupa" :key="index" >
          <el-button class="mybtn" type="primary" size="large">{{item}}</el-button>
        </div>
      </div>
      <div class="listwrap">
        <div>第二组</div>
        <div  v-for="(item,index) in groupb" :key="index">
          <el-button class="mybtn"  type="primary" size="large">{{item}}</el-button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.wrap{
  display: flex;
  width: 100%;
  height: 100%;
  flex-direction: column;
}
.namewrap{
  display: flex;
  flex-direction: row;
}
.group{
  display: flex;
  flex-direction: row;
  align-items: top;
  justify-content: center;
}
.listwrap{
  width: 300px;
}
.mybtn{
  /* transition: all 1s ease-in;
  transform: translateX(-100%); */
  margin-bottom: 15px;
  transform: translateX(-100%);
  animation-name: animation1;
  animation-duration: 2s;
  animation-fill-mode:forwards;
}
.mybtn.on{
  transform: translateX(0);
}
@keyframes animation1 {
  0% {
    transform: translateX(-100%);
  }
  100% {
    transform: translateX(0);
  }
}
/* #myCanvasContainer{
  height: 80vh;
} */

</style>
