<template>
  <div class="min-w-screen min-h-screen bg-yellow-500 flex items-center">
    <div v-if="loaded" class="mx-auto">
      <div class="text-3xl text-center mb-3 uppercase font-mono text-white">
        <h5 v-if="!expired">Timer on</h5>
        <h5 v-else>Timer off</h5>
      </div>
      <section class="text-6xl text-center flex w-full items-center justify-center">
        <div class="w-24 mx-1 p-2 bg-white text-yellow-500 rounded-lg">
          <div class="font-mono leading-none">{{displayDays}}</div>
          <div class="font-mono uppercase text-sm leading-none">days</div>
        </div>
        <div class="w-24 mx-1 p-2 bg-white text-yellow-500 rounded-lg">
          <div class="font-mono leading-none">{{displayHours}}</div>
          <div class="font-mono uppercase text-sm leading-none">hours</div>
        </div>
        <div class="w-24 mx-1 p-2 bg-white text-yellow-500 rounded-lg">
          <div class="font-mono leading-none">{{displayMinutes}}</div>
          <div class="font-mono uppercase text-sm leading-none">minutes</div>
        </div>
        <div class="w-24 mx-1 p-2 bg-white text-yellow-500 rounded-lg">
          <div class="font-mono leading-none">{{displaySeconds}}</div>
          <div class="font-mono uppercase text-sm leading-none">seconds</div>
        </div>
      </section>
    </div>
  </div>


</template>

<script>
export default {
  props: ['year', 'month', 'day', 'hour', 'minute', 'second', 'millisecond'],
  data: () => ({
    displayDays: 0,
    displayHours: 0,
    displayMinutes: 0,
    displaySeconds: 0,
    loaded: false,
    expired: false,
    today: Date.now(),
  }),
  computed: {
    _seconds: () => 1000,
    _minutes() {
      return this._seconds * 60
    },
    _hours() {
      return this._minutes * 60
    },
    _days() {
      return this._hours * 24
    },
    end() {
      return new Date(
        this.year,
        this.month,
        this.day,
        this.hour,
        this.minute,
        this.second,
        this.millisecond
      )
    }
  },
  mounted() {
    this.showRemaining()
  },
  methods: {
    formatNum: (num) => (num < 10 ? '0' + num : num),

    showRemaining() {
      const timer = setInterval(() => {
        const now = new Date();

        // timer static working
        const end = new Date(this.today + 24 * 3600 * 1000 * 1.7);
        const distance = end.getTime() - now.getTime();

        // timer working with props
        //const distance = this.end.getTime() - now.getTime();

        if (distance < 0) {
          clearInterval(timer);
          this.expired = true;
          this.loaded = true;
          return;
        }

        const days = Math.floor((distance / this._days));
        const hours = Math.floor((distance % this._days) / this._hours);
        const minutes = Math.floor((distance % this._hours) / this._minutes);
        const seconds = Math.floor((distance % this._minutes) / this._seconds);
        this.displayDays = this.formatNum(days);
        this.displayHours = this.formatNum(hours);
        this.displayMinutes = this.formatNum(minutes);
        this.displaySeconds = this.formatNum(seconds);
        this.loaded = true;
      }, 1000)
    }
  }
}
</script>

<style scoped>
.seconds {
  max-width: 60px;
}
</style>