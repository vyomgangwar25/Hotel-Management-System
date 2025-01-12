<script setup>
import { Form, Field, ErrorMessage } from "vee-validate";
const roomPrice = ref("");
const selectedRoom = ref("");
const items = ref([]);
const token = useCookie("Token");

const handleSubmit = async () => {
  try {
    const response = await fetch("http://localhost:8080/room/create", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        Authorization: `Bearer ${token.value}`,
      },
      body: JSON.stringify({
        roomtype: selectedRoom.value,
        roomprice: roomPrice.value,
      }),
    });
    const data = await response.text();
    console.log(data);
  } catch (err) {}
};
const handleList = async () => {
  try {
    const response = await fetch("http://localhost:8080/room/roomTypes", {
      method: "GET",
      headers: {
        "Content-Type": "application/json",
        Authorization: `Bearer ${token.value}`,
      },
    });
    const data = await response.json();
    items.value = data;
  
  } catch (err) {}
};
onMounted(() => {
  handleList();
});
</script>
<template>
  <div class="w-full h-screen bg-gray-200 flex justify-center items-center">
    <div class="w-2/5 p-8 bg-white shadow-lg rounded-lg">
      <h1 class="text-2xl font-bold flex items-center justify-center">
        Add Room
      </h1>

      <Form @submit="handleSubmit">
        <div>
          <label for="">Room Type</label>
          <Field
            name="RoomType"
            as="select"
            class="w-full rounded border-2 px-2 py-2"
            v-model="selectedRoom"
            rules="required"
          >
            <option value="" disabled>Select Room</option>
            <option v-for="item in items" :value="item.room">
              {{ item.room }}
            </option>
             
          </Field>
          <ErrorMessage name="RoomType" class="text-red-600" />
        </div>
        <div class="mt-3">
          <label for="" class="text">Room Price</label>
          <Field
            name="RoomPrice"
            type="text"
            v-model="roomPrice"
            rules="required"
            class="px-4 py-2 text-base border-2 w-full"
          ></Field>
          <ErrorMessage name="RoomPrice" class="text-red-600" />
        </div>
        <div class="flex justify-center items-center">
          <Button label="Submit" />
        </div>
      </Form>
    </div>
  </div>
</template>
