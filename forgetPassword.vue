<script setup>
import { Form, Field, ErrorMessage } from "vee-validate";
import Button from "~/components/Button.vue";
const useremail = ref("");
 
const handleSubmit = async () => {
  try {
    const response = await fetch("http://localhost:8080/user/forgetpassword", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(useremail.value), // Send email as string
    });
    const data = await response.text();
    console.log(data); // Log the response
  } catch (err) {
    console.error("Error during form submission:", err); 
  }
};
</script>

<template>
  <div class="min-h-screen flex justify-center items-center bg-blue-200">
    <div class="max-w-sm w-full p-8 bg-white shadow-lg rounded-lg">
      <h1 class="flex justify-center items-center text-2xl font-bold">
        Enter Email
      </h1>
 
      <Form @submit="handleSubmit">
        <div class="flex flex-col">
          <label for="email">Email</label>
       
          <Field
            name="email"
            type="email"
            v-model="useremail"
            rules="required|email"
            class="px-4 py-2 text-base border-b-2 border-slate-500 focus:outline-none bg-white text-slate-800 w-full"
          />
          <ErrorMessage name="email" class="text-red-600" />
        </div>
        <div class="flex justify-center items-center mt-2">
          <Button label="Submit"/>
        </div>
      </Form>
    </div>
  </div>
</template>
