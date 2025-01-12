<script setup>
import { Form, Field, ErrorMessage } from "vee-validate";
import DeleteModal from "~/components/DeleteModal.vue";
const token = useCookie("Token");
const items = ref([]);
const items2 = ref([]);
const roomtype = ref();
const roomCount = ref(0);
const id = ref();
const size = 3;
const modalVisible = ref(false);
const trackClickedButton = ref(1);

const buttonClicked = (ind) => {
  trackClickedButton.value = ind;
  handleRoomList();
};
const closeToast = () => {
  modalVisible.value = false;
};

// this function will give the list of room along with all the info of room like price and id
const handleRoomList = async () => {
  try {
    const data = await fetch(
      `http://localhost:8080/room/roomlist?page=${
        trackClickedButton.value - 1
      }&pagesize=${size}`,
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
    roomCount.value = Math.ceil(response.count / size);
  } catch (err) {}
};

// this function is used to update the room info
const update = async (id) => {
  console.log(id);
};

// this function is used to open delete modal
const DeleteModalOpen = async (deleteId) => {
  id.value = deleteId;
  modalVisible.value = true;
};

// this function is used to delete the room by id
const Delete = async () => {
  try {
    const data = await fetch(`http://localhost:8080/room/delete/${id.value}`, {
      method: "Delete",
      headers: {
        "Content-Type": "application/json",
        Authorization: `Bearer ${token.value}`,
      },
    });
    const response = await data.text();
    modalVisible.value = false;
    handleRoomList();
  } catch (err) {
    console.log(err);
  }
};

// this function will give the list of all types of rooms present in database
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
// this function is used to search the room on the basis of room select by user
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
// this function is used to clear the filter
const clearFilter = async () => {
  handleRoomList();
};

onMounted(() => {
  handleRoomList();
  handleRoomType();
});
</script>

<template>
  <DeleteModal
    modalType="delete"
    :isVisible="modalVisible"
    @close="closeToast"
    @success="Delete"
  >
    <template #delete-modal-content-heading>
      Are you sure you want to delete this room
    </template>
  </DeleteModal>

  <div class="w-full h-screen">
    <div class="w- full h-1/5 px-9 mt-5">
      <img
        src="public/admin.jpg"
        class="w-full h-full shadow-lg rounded-lg"
        alt=""
      />
    </div>
    <div class="w-full px-9 mt-5">
      <div class="flex justify-between">
        <h1 class="text-3xl font-medium">Existing Rooms</h1>
        <h1>
          <NuxtLink
            to="/addRooms"
            class="pointer bg-blue-300 rounded p-2 hover:bg-blue-500"
            >Add Room</NuxtLink
          >
        </h1>
      </div>

      <Form class="w-full flex mt-5" @submit="clearFilter">
        <label for="RoomType" class="p-2 bg-gray-200 rounded-l border-2"
          >Filter by room type :</label
        >
        <Field
          name="RoomType"
          as="select"
          v-model="roomtype"
          class="w-1/3 rounded border-2 px-2 py-2"
          rules="required"
          @change="handleSearch"
        >
          <option value="" disabled>Select Room type to filter....</option>

          <option v-for="item in items2" :value="item.room">
            {{ item.room }}
          </option>
        </Field>
        <div class="p-2 rounded-r px-8 py-4 bg-red-500">
          <button>Clear Filter</button>
        </div>
      </Form>
    </div>
    <div class="w-full px-9 mt-5">
      <table class="w-full">
        <thead>
          <tr class="border border-gray-300 px-4 py-2 text-left">
            <th class="border border-gray-300 px-4 py-2 text-left">Id</th>
            <th class="border border-gray-300 px-4 py-2 text-left">
              Room Type
            </th>
            <th class="border border-gray-300 px-4 py-2 text-left">
              Room Price
            </th>
            <th colspan="2" class="border broder-gray-300 px-4 text-center">
              Actions
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            class="border border-gray-300 px-4 py-2 text-left hover:bg-gray-100"
            v-for="(item, ind) in items"
          >
            <td class="border border-gray-300 px-4 py-2 text-left">
              {{ item.id }}
            </td>
            <td class="border border-gray-300 px-4 py-2 text-left">
              {{ item.roomType }}
            </td>
            <td class="border border-gray-300 px-4 py-2 text-left">
              {{ item.price }}
            </td>
            <td class="px-4 py-2 space-x-4 border">
              <button
                class="bg-blue-500 px-2 rounded hover:bg-blue-700"
                @click="update(item.id)"
              >
                View/Edit
              </button>
            </td>
            <td class="px-4 py-2 space-x-4">
              <button
                class="bg-red-500 rounded hover:bg-red-700"
                @click="DeleteModalOpen(item.id)"
              >
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="w-full flex justify-center">
  
      <Pagination :roomCount="roomCount" @success="buttonClicked"/>
    </div>
  </div>
</template>
