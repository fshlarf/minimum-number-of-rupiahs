<template>
  <div class="index">
    <div class="index-container">
      <h4>Masukkan nominal yang ingin dicek</h4>
      <input type="text" v-model="inputAmmount" @keypress="whenKeyPress" @keydown.enter="checkAmmount" placeholder="Masukkan Nominal" :style="errorInput">
      <p class="index-container__errormsg" v-if="errorMsg">{{ errorMsg }}</p>
      <p class="index-container__info">Tekan Enter atau klik Button Check untuk mengecek nominal</p>
      <div class="index-content">
        <button class="index-content__btn" @click="checkAmmount">Check</button>
      </div>
      <div class="index-content" v-for="item in result" :key="item.nominal">
        <h5>{{ item.jumlah }}x Rp {{ item.nominal }}</h5>
      </div>
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
  computed: {
    errorInput() {
      return this.errorMsg ? {'borderColor': 'red'} : {}
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
      const numberPattern = /\d+/g
      if (this.inputAmmount) {
        this.theValue = this.inputAmmount.match(numberPattern).join('')
      }
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
    },
    whenKeyPress() {
      this.errorMsg = ''
    }
  }
}
</script>

<style lang="scss" scoped>
.index {
  margin: 0 auto;
  &-container {
    min-height: 100vh;
    margin-top: 5%;
    justify-content: center;
    align-items: center;
    text-align: center;
    input {
      font-size: 14px;
      width: 95%;
      margin-top: 10px;
      height: 50px;
      position: relative;
      border-radius: 5px;
      outline: none;
      padding: 5px;
      border: 1px solid #DDDDDD;
    }
    @media only screen and (min-width: 1140px) {
      input {
        width: 25%;
      }
    }
    &__errormsg {
      color: red;
      font-size: 18px;
      margin-top: 10px;
    }
    &__info {
      font-size: 12px;
      margin-top: 10px;
      font-style: italic;
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
.error-border {
  border-color: red;
}
</style>
