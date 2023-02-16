<template>
  <div class="w-full">
    <div
      class="fixed text-5xl text-right right-4 top-2 md:text-7xl lg:text-8xl xl:text-9xl"
    >
      <div class="flex justify-end space-x-2">
        <span class="font-thin text-opacity-100 text-amber-300">{{
          days
        }}</span>
        <span class="font-extrabold uppercase text-amber-300"
          >Days</span
        >
      </div>
      <div class="flex justify-end space-x-2">
        <span class="font-thin text-opacity-100 text-amber-300">{{
          hours
        }}</span>
        <span class="font-extrabold uppercase text-amber-300"
          >Hours</span
        >
      </div>
      <div class="flex justify-end space-x-2">
        <span class="font-thin text-opacity-100 text-amber-300">{{
          minutes
        }}</span>
        <span class="font-extrabold uppercase text-amber-300"
          >Minutes</span
        >
      </div>
      <div class="flex justify-end space-x-2">
        <span class="font-thin text-opacity-100 text-amber-300">{{
          seconds
        }}</span>
        <span class="font-extrabold uppercase text-amber-300"
          >Seconds</span
        >
      </div>
      <div class="flex justify-end space-x-2 text-base md:text-2xl">

        <span class="font-medium text-amber-300"
          >Donaldsonville Reservation Countdown</span
        >
      </div>
    </div>
    <div class="flex flex-wrap items-start justify-start w-full">
      <span
        v-for="index in totalMinutes"
        :key="index"
        class="block w-6 h-6 mt-px ml-px xl:w-7 xl:h-7"
        :class="getBoxClass(index)"
      >
      </span>
      <span
        class="relative block w-6 h-6 mt-px ml-px bg-opacity-100 bg-amber-300 xl:w-7 xl:h-7"
      >
        <span
          class="absolute flex items-baseline text-base text-purple-400 uppercase left-0.5 md:text-2xl top-8 md:top-8 md:-left-1 xl:-left-px xl:top-9"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="w-5 h-5 md:w-8 md:h-8"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M8 7l4-4m0 0l4 4m-4-4v18"
            />
          </svg>
          <span>Reservation</span>
        </span>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CountdownTimer',
  data() {
    return {
      interval: null,
      countdownDate: new Date('Febuary 17, 2023 5:00 pm').getTime(),
      startDate: new Date('Febuary 16, 2023, 2023 12:00 pm').getTime(),
      distance: 0,
      expired: false,
    }
  },

  computed: {
    days() {
      return Math.floor(this.distance / (1000 * 60 * 60 * 24))
    },
    hours() {
      return Math.floor(
        (this.distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)
      )
    },
    minutes() {
      return Math.floor((this.distance % (1000 * 60 * 60)) / (1000 * 60))
    },
    seconds() {
      return Math.floor((this.distance % (1000 * 60)) / 1000)
    },
    totalDistance() {
      return this.countdownDate - this.startDate
    },

    totalMinutes() {
      return Math.floor(this.totalDistance / 1000 / 60)
    },

    currentMinutes() {
      return this.totalMinutes - Math.floor(this.distance / 1000 / 60)
    },
  },

  beforeMount() {
    this.interval = setInterval(this.countdownTick, 1000)
  },

  methods: {
    countdownTick() {
      const now = new Date().getTime()

      // Find the distance between now and the count down date
      this.distance = this.countdownDate - now

      // If the count down is finished, write some text
      if (this.distance < 0) {
        clearInterval(this.interval)
        this.expired = true
      }
    },
    isFuture(i) {
      return this.currentMinutes < i
    },
    getBoxClass (index) {
      if (this.isFuture(index)) {
        return 'bg-opacity-40 bg-purple-600';
      }
      if(index === this.currentMinutes) {
        return 'bg-opacity-100 bg-amber-300';
      }

      return 'bg-opacity-100 bg-purple-600';
    },
  },
}
</script>
