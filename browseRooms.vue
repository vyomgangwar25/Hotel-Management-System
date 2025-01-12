<script setup>
import { Form, Field } from "vee-validate";
const items = ref([]);
const items2 = ref([]);
const modalVisible = ref(false);
const token = useCookie("Token");
const router = useRouter();
const roomtype = ref("");
const roomCount = ref(0);
const trackClickedButton = ref(1);
const handleRoomList = async () => {
  try {
    const data = await fetch(
      `http://localhost:8080/room/roomlist?page=${trackClickedButton.value-1}&pagesize=${3}`,
      {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${token.value}`,
        },
      }
    );
    const response = await data.json();
    items.value = response.list;
    roomCount.value = Math.ceil(response.count / 3);
  } catch (err) {
    console.error("Fetch error:", err.message);
  }
};
const handleBooking = (id) => {
  if (!token) {
    modalVisible.value = true;
    return;
  }
  router.push({
    path: "/bookRoom",
    query: { id },
  });
};

const handleRoomType = async () => {
  try {
    const data = await fetch(`http://localhost:8080/room/roomTypes`, {
      method: "GET",
      headers: {
        "Content-Type": "application/json",
        Authorization: `Bearer ${token.value}`,
      },
    });
    const response = await data.json();
    items2.value = response;
  } catch (err) {
    console.log(err);
  }
};

const handleSearch = async () => {
  try {
    const data = await fetch(
      `http://localhost:8080/room/search/${roomtype.value}`,
      {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${token.value}`,
        },
      }
    );
    const response = await data.json();
    items.value = response;
  } catch (err) {
    console.log(err);
  }
};
const clearFilter = async () => {
  handleRoomList();
};
const modalClose = () => {
  modalVisible.value = false;
};
const redirectLogin = () => {
  router.push("/login");
};

const buttonClicked=(number)=>{
 trackClickedButton.value=number
 handleRoomList();
}

onMounted(() => {
  handleRoomList();
  handleRoomType();
});
</script>

<template>
  <div class="w-full h-screen bg-gray-50">
    <DeleteModal
      modalType="viewRooms"
      :isVisible="modalVisible"
      @close="modalClose"
      @success="redirectLogin"
    >
      <template #delete-modal-content-heading>
        Please log in to view available rooms
      </template>
    </DeleteModal>

    <div class="max-w-7xl mx-auto">
      <div class="flex justify-between">
        <Form class="mt-2 w-full flex" @submit="clearFilter">
          <label
            for="RoomType"
            class="p-2 w-1/8 bg-gray-200 border-1 rounded-l border-2"
            >Filter by room type :</label
          >
          <Field
            name="RoomType"
            as="select"
            v-model="roomtype"
            class="w-2/5 border-2 px-2 py-2"
            @change="handleSearch"
            rules="required"
          >
            <option value="" disabled>Select Room type to filter....</option>

            <option v-for="item in items2" :value="item.room">
              {{ item.room }}
            </option>
          </Field>
          <div class="p-2 text-xl rounded-r bg-red-500">
            <button class="">clear filter</button>
          </div>
        </Form>

        <div
          class=" border-2 border-black mr-2 rounded text-blue-400 p-2 mt-5 cursor-pointer"
          v-for="(number, ind) in roomCount"
          :class="trackClickedButton == number?  'bg-blue-400 text-white' : ''"
          @click="buttonClicked(number)"
        >
          {{ number }}
        </div>
      </div>
      <div v-for="(item, ind) in items" class="mt-4 border-2 border-gray">
        <div class="flex justify-between w-full">
          <div class="flex flex-row w-1/2">
            <div class="">
              <img
                src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTj5k5vBGnZWxOsrAECLllBigzDVlaofyXtqg&s"
                class="m-5 rounded-lg ml-3 w-48 h-42"
                alt=""
              />
            </div>
            <div class="p-4 mt-5 text-xl">
              <h1 class="text-red-500 font-bold">{{ item.roomType }}</h1>
              <h1 class="text-blue-500">{{ item.price }}/night</h1>
              <h1>this is room description</h1>
            </div>
          </div>
          <div
            class="flex items-center my-14 p-2 text-white mr-14 bg-red-500 rounded"
          >
            <button @click="handleBooking(item.id)">View/Book Now</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
