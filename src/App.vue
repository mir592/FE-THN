<script lang="ts">
import { defineComponent, ref } from 'vue';
import Datepicker from 'vue3-datepicker';
import CustomSelect from './components/CustomSelect.vue';
import Rooms from './components/Rooms.vue';
import Summary from './components/Summary.vue';

export default defineComponent({
  components: {
    CustomSelect,
    Datepicker,
    Rooms,
    Summary,
  },
  data () {
    const queryString = window.location.search;
    const urlParams = new URLSearchParams(queryString);
    const promo_code = urlParams.get('promo_code');
    return {
      promo_code: promo_code ? parseInt(promo_code) : null
    }
  },
  setup() {
    const currentDay = new Date();
    const followingDay = new Date(currentDay.getTime() + 86400000);
    const sd = ref(currentDay);
    const ed = ref(followingDay);
    const adults = ref('');
    const children = ref('');
    const summary = ref(null);

    const selectRoom = (room: any) => {
      if (summary.value) {
        summary.value.updateHotelData(room);
      }
    }
    const modifyReservationHandler = () => {
      if (summary.value) {
        summary.value.updateReservationData({
          startDate: new Intl.DateTimeFormat('en-US').format(sd.value),
          endDate: new Intl.DateTimeFormat('en-US').format(ed.value),
          adults: adults.value,
          children: children.value
        })
      }
    };
    return {
      sd,
      ed,
      adults,
      children,
      selectRoom,
      summary,
      modifyReservationHandler,
    }
  }
})
</script>

<template>
  <header class="flex justify-between max-w-6xl px-8 py-4 mx-auto">
    <div class="flex justify-between flex-col md:flex-row w-1/3">
      <a href="#" class="menu-btn">Home</a>
      <a href="#" class="menu-btn">Rooms</a>
      <a href="#" class="menu-btn">Restaurant</a>
    </div>
    <div class="flex justify-center w-auto md:w-1/3">
      <img src="./assets/img/logo-cocos.png" alt="Hotel Los Cocos" class="logo">
    </div>
    <div class="flex justify-between flex-col md:flex-row w-1/3">
      <a href="#" class="menu-btn">Weedings</a>
      <a href="#" class="menu-btn">Membership</a>
      <a href="#" class="menu-btn">Contact</a>
    </div>
  </header>
  <div class="mb-4 hero">
    <div class="flex justify-center px-8 py-4 bg-primary bg-opacity-40">
      <div class="flex items-center md:items-start flex-col md:flex-row md:space-x-4">
        <div class="calendar-wrapper mb-1 md:mb-0">
          <Datepicker v-model="sd" class="calendar-input" />
        </div>
        <div class="calendar-wrapper mb-1 md:mb-0">
          <Datepicker v-model="ed" class="calendar-input" />
        </div>
        <CustomSelect class="mb-1 md:mb-0">
          <select v-model="adults">
            <option value="">Select Adults</option>
            <option value="1">Adults: 1</option>
            <option value="2">Adults: 2</option>
            <option value="3">Adults: 3</option>
            <option value="4">Adults: 4</option>
            <option value="5">Adults: 5</option>
          </select>
        </CustomSelect>
        <CustomSelect class="mb-1 md:mb-0">
          <select v-model="children">
            <option value="">Select Children</option>
            <option value="0">Children: 0</option>
            <option value="1">Children: 1</option>
            <option value="2">Children: 2</option>
            <option value="3">Children: 3</option>
            <option value="4">Children: 4</option>
            <option value="5">Children: 5</option>
            <option value="6">Children: 6</option>
          </select>
        </CustomSelect>
        <button @click="modifyReservationHandler" class="primary-btn ">Modify</button>
      </div>
    </div>
  </div>
  <div class="max-w-6xl p-8 mx-auto mb-4">
    <h1 class="mb-2 text-2xl font-display">Rooms & Rates</h1>
    <p class="mb-4">Plan your perfect stay at your hotel</p>
    <img src="./assets/img/path.png" alt="Path" width="400">
  </div>
  <div class="flex flex-col md:flex-row md:inline-block px-8 mb-16">
    <div class="float-left w-auto md:w-2/3">
      <Rooms
        :select-room="selectRoom"
      />
    </div>
    <div class="md:float-right w-auto md:w-1/3 md:pl-4 mt-2 md:mt-0">
      <Summary
        ref="summary"
        :promo_code="promo_code"
        />
    </div>
  </div>
  <div class="p-4 border-t border-gray-light">
    <div class="flex flex-col md:flex-row md:justify-between mx-auto text-sm footer">
      <div class="flex items-center space-x-4">
        <div>
          <img src="./assets/img/los-cocos-iso-footer.png" width="45">
        </div>
        <a href="#">Terms and Conditions</a>
        <a href="#">Privacy Policy</a>
        <a href="#">About us</a>
        <a href="#">Partners</a>
      </div>
      <div class="flex flex-col md:flex-row items-center space-x-4 mt-2 md:mt-0">
        <a href="#">reservations@loscocosbungalows.com</a>
        <a href="#">Tlf: +34 555 555 555</a>
        <div>
          <img src="./assets/img/footer_icons.png" width="125">
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
  .logo {
    width: 125px;
    height: auto;
  }
  .menu-btn {
    @apply transition-colors;
    @apply duration-300;
  }
  .menu-btn:hover {
    @apply text-primary;
  }
  .hero {
    @apply bg-cover;
    @apply bg-center;
    @apply bg-no-repeat;
    background-image: url(./assets/img/los-cocos-room-header-2-x.png);
  }
  .calendar-wrapper {
    position: relative;
  }
  .calendar-wrapper:before {
    content: '';
    position: absolute;
    @apply right-2.5;
    top: 50%;
    transform: translateY(-50%);
    display: block;
    width: 25px;
    height: 25px;
    background-repeat: no-repeat;
    background-position: center;
    background-image: url(./assets/img/calendar-empty.svg);
    z-index: 100;
  }
  .footer {
    max-width: 80rem;
  }
</style>
<style>
  :root {
    --vdp-hover-bg-color: theme('colors.primary');
    --vdp-selected-bg-color: theme('colors.primary');
  }
  .calendar-input {
    @apply p-2 pr-8;
  }
</style>