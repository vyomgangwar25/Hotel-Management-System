<template>
  <h1>this is dashboard</h1>
  <div class="flex items-center justify-center">
    <!-- <input type="text" v-model="value" class="border-2" name="" id="" /> -->
    <!-- <h1>{{ newfuc  }}</h1> -->
    
     <button  @click="dec" class="bg-red-200 rounded p-2 mr-2">Dec</button>
     {{ count }}
     <button @click="inc" class="bg-green-200 rounded p-2 ml-2">Inc</button>

    <!-- <Button label="click" @clicked="handleLogin()"/> -->
  </div>
</template>
<script setup>
const value = ref("");
const count=ref(0)
const dec=async()=>{
  if(count.value >0)
  {
   count.value=count.value-1;
  }
}
const inc=async()=>{
    count.value=count.value+1;

}
//  import Button from '~/components/Button.vue';
//  const handleLogin=async()=>{
//    console.log("button clicked")
//  }
// const newfuc=computed(() => {
//    if(value.value)
// {
//    return value.value==="" ?"value is empty":`input value is ${value.value}`
// }

// });
// watch(value, (value) => {
//   if (value == "") {
//     console.log("chal gyi");
//   }
// });
</script>
