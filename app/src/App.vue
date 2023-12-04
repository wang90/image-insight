<template>
  <div class="">
    <input type="file" @input="getImage">
    <img class="origin-image" :src="image" alt="">
    <div>
      <h1>{{ getStatus }}</h1>
      <pre>{{ message }}</pre>
    </div>
  </div>
</template>
<script setup lang="ts">
console.log(' yes ');
import Tesseract from 'tesseract.js';
import { ref, computed } from 'vue';
console.log(Tesseract);
const message = ref('');
const status = ref( 0 ) // 0 没有内容 ， 2 完成，-1失败
const image = ref('');


const getStatus = computed(() => {
  switch( status.value ) {
    case 0:
      return '';
    case 1:
      return '识别中';
    case 2:
      return '以下识别出来的内容';
    case -1:
      return '识别失败'
    default:
      return '识别失败';
  }

  return
})



const getImageMessage = ( url:string ) => {
  status.value = 1
  
  Tesseract.recognize(
    url,
    'chi_sim',
    ).then( res => {
      console.log(res.data.text);
      message.value = res.data.text;
      status.value = 2;
    }).catch(() => {
      status.value = -1;
    })
}

const getImage = ($event:any)=> {
  console.log($event);
  var file = $event.target.files[0];
  var filepath = URL.createObjectURL(file);
  console.log(filepath);

  image.value = filepath;

  getImageMessage( filepath );
}



</script> 

<style  scoped>
.origin-image {
  width: 350px;
}
</style>