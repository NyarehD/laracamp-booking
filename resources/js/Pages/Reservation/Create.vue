<template>
  <div>
    <h1 class="text-3xl font-light">Create New Reservation</h1>
    <div class="[&>div]:mt-2">
      <div class="mb-3">
        <h2 class="mb-2 text-2xl">Room</h2>
        <div class="gap-x-3 flex mb-2 [&>div]:w-60">
          <div class="flex flex-col">
            <label for="room">Room Number</label>
            <MultiSelect v-model="reservationForm.room_id" :options="availableRooms" filter placeholder="Select rooms"
              :maxSelectedLabels="5" class=" w-full" :class="{ 'p-invalid': errors.room_id }" />
            <InlineMessage v-if="errors.room_id" severity="error" class="mt-2">{{ errors.room_id }}</InlineMessage>
          </div>
          <div class=" flex flex-col">
            <label for="totalPerson">Total Number Of Person</label>
            <InputNumber id="totalPerson" v-model="reservationForm.total_person"
              :class="{ 'p-invalid': errors.total_person }" class="" />
            <InlineMessage v-if="errors.total_person" severity="error" class="mt-2">{{ errors.total_person }}
            </InlineMessage>
          </div>
          <div class="flex flex-col">
            <label for="totalPrice">Total Price</label>
            <InputNumber id="totalPrice" v-model="reservationForm.total_price" mode="currency" currency="MMK"
              :class="{ 'p-invalid': errors.total_price }" />
            <InlineMessage v-if="errors.total_price" severity="error" class="mt-2">{{ errors.total_price }}
            </InlineMessage>
          </div>
        </div>
        <h3 class=" text-xl">Room Reservation Date</h3>
        <div class="gap-x-3 flex mt-2 [&>div]:w-60">
          <div class="flex flex-col">
            <label for="reservedFrom">From</label>
            <Calendar v-model="reservationForm.from_date" :minDate="minDate" :manualInput="false" id="reservedFrom"
              :class="{ 'p-invalid': errors.from_date }" class="w-full" />
            <InlineMessage v-if="errors.from_date" severity="error" class="mt-2">{{ errors.from_date }}</InlineMessage>
          </div>
          <div class="flex flex-col">
            <label for="reservedTo">To</label>
            <Calendar v-model="reservationForm.to_date" :minDate="minDate" :manualInput="false" id="reservedTo"
              :class="{ 'p-invalid': errors.to_date }" />
            <InlineMessage v-if="errors.to_date" severity="error" class="mt-2">{{ errors.to_date }}</InlineMessage>
          </div>
        </div>
      </div>
      <div class="gap-x-3 flex mt-2 [&>div]:w-60">
        <div class="flex flex-col">
          <label for="checkin">Checkin</label>
          <Calendar v-model="reservationForm.checkin_time" :minDate="minDate" :manualInput="false" id="checkin"
            :showTime="true" hourFormat="24" :class="{ 'p-invalid': errors.checkin_time }" />
          <InlineMessage v-if="errors.checkin_time" severity="error" class="mt-2">{{ errors.checkin_time }}
          </InlineMessage>
        </div>
        <div class=" flex flex-col">
          <label for="checkout">Checkout</label>
          <Calendar v-model="reservationForm.checkout_time" :minDate="minDate" :manualInput="true" id="checkout"
            :showTime="true" hourFormat="24" :class="{ 'p-invalid': errors.checkout_time }" />
          <InlineMessage v-if="errors.checkout_time" severity="error" class="mt-2">{{ errors.checkout_time }}
          </InlineMessage>
        </div>
      </div>
      <div class="mb-3">
        <Button label="Add Reservation" outlined @click="submitForm" />
      </div>
    </div>
  </div>
</template>

<script setup>
  import { useForm } from '@inertiajs/vue3';
  import Button from 'primevue/button';
  import Calendar from 'primevue/calendar';
  import InputNumber from 'primevue/inputnumber';
  import MultiSelect from 'primevue/multiselect';
  import InlineMessage from 'primevue/inlinemessage';
  import { useToast } from "primevue/usetoast";

  import { ref } from 'vue';

  const props = defineProps({
    // availableRooms: Array,
    errors: Object
  })

  // Dummy data for props
  const availableRooms = ref([
    "234", "2323", "342", "924", "239", "294", "629"
  ])

  const reservationForm = useForm({
    room_id: [],
    total_person: 0,
    total_price: 0,
    from_date: "",
    to_date: "",
    checkin_time: "",
    checkout_time: "",
  })

  const toast = useToast();
  function submitForm() {
    reservationForm.post(route("reservation.store"), {
      onSuccess: () => toast.add({ severity: 'success', summary: 'Success', detail: 'Added Reservation Successfully', life: 3000 }),
    })
  }

  //For calculating min date
  const minDate = ref(new Date());

</script>
