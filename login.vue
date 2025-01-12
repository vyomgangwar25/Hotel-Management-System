<script setup>
import { Form, Field, ErrorMessage } from "vee-validate";
import Button from "~/components/Button.vue";
const useremail = ref("");
const userpassword = ref("");
const Token = useCookie("Token");
const router = useRouter();
const handleSubmit = async () => {
  try {
    const response = await fetch("http://localhost:8080/user/login", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        email: useremail.value,
        password: userpassword.value,
      }),
    });
    const data = await response.text();
    if (!response.ok) {
      console.log(data);
      return;
    }

    Token.value = data;
    router.push("/dashboard");
  } catch (err) {
    console.error("Error during form submission:", err);
  }
};
</script>

<template>
  <div class="min-h-screen flex justify-center items-center bg-blue-200">
    <div class="max-w-sm w-full p-8 bg-white shadow-lg rounded-lg">
      <div class="flex flex-col">
        <h1 class="font-bold text-2xl p-3 flex justify-center">Login Form</h1>
        <Form @submit="handleSubmit">
          <div>
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
          <div>
            <label for="password">Password</label>
            <Field
              name="password"
              type="password"
              v-model="userpassword"
              rules="required|min:6"
              class="px-4 py-2 text-base border-b-2 border-slate-500 focus:outline-none bg-white text-slate-800 w-full"
            />
            <ErrorMessage name="password" class="text-red-600" />
          </div>
          <div class="flex justify-center items-center">
            <Button label="Sign in" />
          </div>
        </Form>

        <div class="flex items-center justify-center mt-2">
          <NuxtLink to="/forgetPassword"
            ><p class="cursor-pointer">
              forget <span class="text-blue-500">password?</span>
            </p></NuxtLink
          >
        </div>
        <div class="flex items-center justify-center mt-1">
          <NuxtLink to="/registration">
            <p>
              don't have account? <span class="text-blue-500">Sign up</span>
            </p></NuxtLink
          >
        </div>
      </div>
    </div>
  </div>
</template>
