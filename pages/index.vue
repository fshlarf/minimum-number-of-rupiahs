<template>
  <div class="index-container">
    <input type="text" style="font-size: 40px;" v-model="inputAmmount" @keydown.enter="checkAmmount">
    <div class="index-content">
      <button class="index-content__btn" @click="checkAmmount">Check</button>
    </div>
    <div class="index-content" v-for="item in result" :key="item.nominal">
      <h5>{{ item.jumlah }}x Rp {{ item.nominal }}</h5>
    </div>
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  },
  data() {
    return {
      inputAmmount: '',
      dataArray: [100000, 50000, 20000, 10000, 5000, 2000, 1000, 500, 100, 50 ],
      result: ''
    }
  },
  methods: {
     checkAmmount() {
      let tempAmmount = this.inputAmmount
      if (this.inputAmmount !== '') {
        this.arrayAmount = []
        this.inputAmmount = JSON.parse(this.inputAmmount)
        this.result = this.dataArray.map(e => {
          if (tempAmmount >= e) {
            let jumlah = tempAmmount / e
            tempAmmount = tempAmmount % e
            return {'nominal': e,'jumlah' : Math.floor(jumlah)}
          }
        }).filter(function( element ) {
          return element !== undefined
        })
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.index {
  &-container {
    margin: 0 auto;
    min-height: 100vh;
    margin-top: 30%;
    justify-content: center;
    align-items: center;
    text-align: center;
  }
  &-content {
    margin-top: 20px;
    &__btn {
      background-color: dodgerblue;
      color: white;
      border: 0px;
      padding: 7px 10px 7px 10px;
    }
  }
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
