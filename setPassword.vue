<script setup>
import { Form, Field, ErrorMessage } from "vee-validate";
   const route=useRoute();
   const token=ref("")
   const cookies=useCookie("Token")
   if(route.query.token)
   {
    token.value = route.query.token;
   }
   else
   {
  token.value=cookies.value
   }
console.log(token.value);

const newPassword = ref("");
const handleSubmit=async()=>{
    try{
        const response=await fetch("http://localhost:8080/user/setpassword",{
        method:"POST",
        headers: {
        "Content-Type": "application/json",
        "Authorization": `Bearer ${token.value}`,
      },
      body: JSON.stringify(newPassword.value),
    })
    const data=await response.text();
    console.log(data);
    }
    catch(err)
    {
        console.log(err);
    }
    
}
</script>

<template>
  <div class="min-h-screen flex justify-center items-center bg-blue-200">
    <div class="max-w-sm w-full p-8 bg-white shadow-lg rounded-lg">
      <h1 class="flex justify-center items-center text-2xl font-bold p-2">
        Enter New Password
      </h1>
      <Form @submit="handleSubmit">
        <div>
          <label for="">Password</label>
          <Field
            name="password"
            type="password"
            rules="required|min:6"
            v-model="newPassword"
            class="px-4 py-2 border-b-2 w-full border-slate-500 focus:outline-none bg-white text-slate-800"
          />
          <ErrorMessage name="password" class="text-red-600" />
          
        </div>
        <div class="flex justify-center items-center p-2">
          <Button label="Submit"/>
        </div>
      </Form>
    </div>
  </div>
</template>
