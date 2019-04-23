<template>
  <div class="index-container">
    <h4>Masukkan nominal yang ingin dicek</h4>
    <input type="text" v-model="inputAmmount" @keydown.enter="checkAmmount">
    <p class="index-container__errormsg" v-if="errorMsg">{{ errorMsg }}</p>
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
      result: '',
      errorMsg: '',
      theValue: ''
    }
  },
  methods: {
    checkAmmount() {
      const newArray = this.inputAmmount.split(' ')
      if (this.inputAmmount.indexOf(',') > -1) {
        this.errorMsg = '(Input tidak valid) karakter yang diisi tidak boleh ada koma'
        this.result = ''
        return
      } else if (this.inputAmmount[0] === '0') {
        this.errorMsg = '(Input tidak valid) Karakter yang diisi tidak boleh dimulai dari angka 0'
        this.result = ''
        return
      } else {
        if (newArray.length > 1) {
          const fisrtArray = parseInt(newArray[0])
          console.log(fisrtArray)
          if (fisrtArray > 0) {
            this.errorMsg = 'Input tidak valid'
            this.result = ''
            return
          } else {
            this.executeTheAmmount()
          }
        } else {
          this.executeTheAmmount()
        }
      }
    },
    executeTheAmmount() {
      const numberPattern = /\d+/g;
      this.inputAmmount ? this.theValue = this.inputAmmount.match(numberPattern).join('') : this.errorMsg = 'Silahkan input nominal yang akan dicek'
      let tempAmmount = this.theValue.replace(/^0+/, '')
      if (this.theValue !== '') {
        this.errorMsg = ''
        this.theValue = JSON.parse(this.theValue)
        this.result = this.dataArray.map(e => {
          if (tempAmmount >= e) {
            let jumlah = tempAmmount / e
            tempAmmount = tempAmmount % e
            return {'nominal': e,'jumlah' : Math.floor(jumlah)}
          }
        }).filter(function( element ) {
          return element !== undefined
        })
      } else if (!this.theValue) {
        this.errorMsg = 'Silahkan input nominal yang akan dicek'
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
    margin-top: 5%;
    justify-content: center;
    align-items: center;
    text-align: center;
    input {
      font-size: 40px;
      margin-top: 10px;
    }
    &__errormsg {
      color: red;
      font-size: 18px;
      margin-top: 10px;
    }
  }
  &-content {
    margin-top: 20px;
    &__btn {
      background-color: dodgerblue;
      color: white;
      border: 0px;
      padding: 7px 10px 7px 10px;
      border-radius: 4px;
    }
  }
}
</style>
