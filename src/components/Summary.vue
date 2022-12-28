<script lang="ts">
import { defineComponent, reactive } from 'vue'

export default defineComponent({
  props: {
    promo_code: {
      type: Number,
      default: -1,
    },
  },
  setup () {
    const data = reactive({
      roomName: localStorage.getItem('roomName') ? localStorage.getItem('roomName'): '-',
      checkinDate: localStorage.getItem('checkinDate') ? localStorage.getItem('checkinDate') : '-',
      checkoutDate: localStorage.getItem('checkoutDate') ? localStorage.getItem('checkoutDate') : '-',
      adults: localStorage.getItem('adults') ? localStorage.getItem('adults') : '-',
      children: localStorage.getItem('children') ? localStorage.getItem('children') : '-',
      total: localStorage.getItem('total') ? localStorage.getItem('total') : '-',
    });
    const updateHotelData = (hotelData: any) => {
      data.roomName = hotelData.name;
      data.total = hotelData.amount;
    };
    const updateReservationData = (reservationData: any) => {
      data.checkinDate = reservationData.startDate;
      data.checkoutDate = reservationData.endDate;
      data.adults = reservationData.adults;
      data.children = reservationData.children;
    }
    const saveHandler = () => {
      data.roomName && localStorage.setItem('roomName', data.roomName);
      data.checkinDate && localStorage.setItem('checkinDate', data.checkinDate);
      data.checkoutDate && localStorage.setItem('checkoutDate', data.checkoutDate);
      data.adults && localStorage.setItem('adults', data.adults);
      data.children && localStorage.setItem('children', data.children);
      data.total && localStorage.setItem('total', data.total);
    };

    return {
      data,
      updateHotelData,
      updateReservationData,
      saveHandler,
    };
  }
})
</script>

<template>
  <div class="p-4 border border-gray-light">
    <h2 class="mb-8 text-xl"><strong>Reservation summary</strong></h2>
    <h3 class="mb-4"><strong>{{ data.roomName }}</strong></h3>
    <div class="mb-4 space-y-8 text-sm">
      <div class="flex space-x-12">
        <div>
          <p><strong>Check in</strong></p>
          <p>From 15.00h</p>
        </div>
        <div>
          <p><strong>Check out</strong></p>
          <p>Before 12.00h</p>
        </div>
      </div>
      <div>
        <p><strong>Reservation date</strong></p>
        <p>From {{ data.checkinDate }} to {{ data.checkoutDate }}</p>
      </div>
      <div>
        <p><strong>People</strong></p>
        <p v-if="data.adults !=='-'">{{ data.adults }} Adults</p>
        <p v-if="data.children !=='-'">{{ data.children }} Children</p>
      </div>
    </div>
    <hr class="mb-4 border-0 border-t border-gray-light">
    <div v-if="promo_code">
      <div class="flex justify-between mb-8">
        <p>Subotal</p>
        <p>{{ data.total }}</p>
      </div>
      <div class="flex justify-between mb-8 color-red">
        <p>Promo Code</p>
        <p>-{{ promo_code }}%</p>
      </div>
    </div>
    <div class="flex justify-between mb-8">
      <p>Total</p>
      <p v-if="data.total && promo_code">{{ parseInt(data.total) - (parseInt(data.total) / 100 * promo_code)}}€</p>
      <p v-else>{{ data.total }}</p>
    </div>
    <button @click="saveHandler" class="button">Save</button>
  </div>
</template>

<style scoped>
.button {
  display: block;
  width: 100%;
  background-color: #0162B3;
  color: white;
  padding: 10px;
  font-size: 1rem;
  line-height: 1.5rem;
  min-width: 7rem;
  transition: all 0.3s ease;
}
.color-red {
  color: red;
}
</style>