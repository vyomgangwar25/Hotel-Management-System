<script setup>
import { Form, Field, ErrorMessage } from "vee-validate";
import Button from "~/components/Button.vue";
const name = ref("");
const useremail = ref("");
const password = ref("");

const handleSubmit = async () => {
  try {
    const response = await fetch("http://localhost:8080/user/registration", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        username: name.value,
        email: useremail.value,
        password: password.value,
      }),
    });

    const data = await response.json();
    console.log(data);
    localStorage.setItem("Token", data);
  } catch (err) {
    console.log(err);
  }
};
</script>

<template>
  <div class="min-h-screen flex items-center justify-center bg-blue-200">
    <div class="max-w-sm w-full p-8 bg-white shadow-lg rounded-lg">
      <h1 class="text-2xl font-bold flex items-center justify-center">
        Registration Form
      </h1>
      <Form @submit="handleSubmit">
        <div>
          <label for="Name">Name</label>
          <Field
            name="name"
            type="text"
            v-model="name"
            class="px-4 py-2 text-base border-b-2 border-slate-500 focus:outline-none bg-white text-slate-800 w-full"
            rules="required"
          />
        
        </div>
        <div >
          <label for="">Email</label>
          <Field
            name="useremail"
            type="email"
            v-model="useremail"
            class="px-4 py-2 border-b-2 border-slate-500 focus:outline-none bg-white text-slate-800 w-full"
            rules="required|email"
          />
          <ErrorMessage name="useremail" class="text-red-600" />
        </div>
        <div>
          <label for="">Password</label>
          <Field
            name="password"
            type="password"
            v-model="password"
            class="px-4 py-2 border-b-2 border-slate-500 focus:outline-none bg-white text-slate-800 w-full"
            rules="required|min:6"
          />
          <ErrorMessage name="password" class="text-red-600" />
        </div>

        <div class="flex justify-center items-center">
          <Button label="Register" />
        </div>
      </Form>
      <div class="flex justify-center mt-2">
        <NuxtLink to="/login"
          ><p>
            Already have an account? <span class="text-blue-500">sign in</span>
          </p></NuxtLink
        >
      </div>
    </div>
    <div></div>
  </div>
</template>
