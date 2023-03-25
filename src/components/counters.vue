<script>
export default {
  data() {
    return {
      data: [],
    }
  },
  methods: {
    createData() {
      const newId = this.data.length + 1;
      let intervalTime = false;
      const newData = {
        id: newId, time: 0, active: false,
        startTime() { this.active = !this.active; return intervalTime = setInterval(() => this.time++, 1000)},
        stopTime() { this.active = !this.active;  return clearInterval(intervalTime) },
      }
      this.data.push(newData);
      },
    getTime(times) { 
      const newTime = Math.trunc(times);
      if (newTime < 10) return "0" + newTime;
      return newTime;
    },
    resetDataTime(counter) {
     return this.data = this.data.filter(el=> el.id !== counter.id)
    }
  }
  
}
</script>

<template>
  <div class="wrapper">
    <counter 
      class="wrapper-counter"
      v-for="counter in data"
      :key="counter.id"
    >
        <div class="counter-time" :class="{'counter-active': counter.active}">
            {{ [Math.trunc(counter.time/60/60%60), Math.trunc(counter.time/60%60) > 0 && getTime(counter.time/60%60),  getTime(counter.time%60)].filter(el=> !!el).join(":")}}  
        </div> 
        <div class="counter-btn">
          <div :class="{'pause': counter.active, 'triangle': !counter.active}" v-on:click="!counter.active ? counter.startTime() : counter.stopTime() "></div>
          <div class="square" :class="{'active-square': counter.active}" v-on:click="resetDataTime(counter)"></div>
        </div>
    </counter>
    <div v-on:click="createData()" class="wrapper-counter counter-create">
      +
    </div>
  </div>
</template>

<style>
.wrapper{
    display: grid;
    gap: 3.75rem;
    grid-template-columns: repeat(auto-fill, 225px);
    margin-top: 4.5rem;
    justify-content: center;
}
.wrapper .wrapper-counter{
  background-color: #696969;
  display: block;
  height: 120px;
  color: #9E9E9E;
  width: 225px;
}
.wrapper .counter-create{
  display: grid;
  justify-content: center;
  align-content: center;
  font-size: 40px;
  font-weight: 900;
  cursor: pointer;
}
.wrapper-counter .counter-time, .counter-btn{
  height: 60px;
  display: grid;
  justify-content: center;
  text-align: center;
  align-items: center;
  cursor: pointer;
  font-family: 'Gotham Pro';
  font-style: normal;
  font-weight: 400;
  font-size: 22px;
  line-height: 21px;
}

.wrapper-counter .counter-time{
  border-bottom: 1px solid #9E9E9E;
}

.wrapper-counter .counter-btn{
  grid-template-columns: minmax(10px, 20px) minmax(10px, 20px);
  gap: 48px;
}

.wrapper-counter .counter-active{
  color: #FFFFFF;
  border-bottom: 1px solid #FFFFFF;
}

.triangle{
  border: 10px solid transparent;
  border-left: 17px solid #9E9E9E;
}

.square{
  border: 10px solid #9E9E9E;
}

.active-square{
  border-color: #FFFFFF;
}

.pause{
  width: 10px;
    height: 20px;
    border-style: double;
    border-width: 0px 0px 0px 10px;
    border-color: #ffffff;
}
</style>
