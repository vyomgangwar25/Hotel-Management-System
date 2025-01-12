<template>
  <div class="w-full h-screen">
    <div class="w- full h-1/5 px-9 mt-5">
      <img
        src="public/admin.jpg"
        class="w-full h-full shadow-lg rounded-lg"
        alt=""
      />
    </div>
    <div class="w-full px-9 mt-5">
      <h1 class="text-2xl   font-medium">Welcome to admin pannel</h1>
      <div class="flex flex-col">
        <ul class="list-disc px-8  text-blue-400 underline">
          <li><NuxtLink to="/existingRooms">manage rooms</NuxtLink></li>
          <li><NuxtLink to="/">manage bookings</NuxtLink></li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script setup></script>
