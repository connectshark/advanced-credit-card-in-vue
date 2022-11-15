<template>
  <main>
    <form class="container">
      <div class="card end">
        <div class="roll"></div>
        <fieldset class="card-cvc">
          <legend>CVC</legend>
          <label for="cvc">CVC</label>
          <div>
            <input
              v-model.trim.number="creditCard.cvc"
              type="tel"
              ref="cardCvcRef"
              id="cvc"
              required
              maxlength="3"
              @keypress="handlerCardNumber"
              pattern="[0-9]{3}"
            >
          </div>
        </fieldset>
      </div>
      <div class="card front">
        <div class="row">
          <h1>NS Bank</h1>
          <span class="card-logo">
            <i class='bx bxl-visa' v-if="false"></i>
            <i class='bx bxl-mastercard' ></i>
          </span>
        </div>
        <fieldset class="card-number">
          <legend>CARD NUMBER</legend>
          <label for="cc-1">CARD NUMBER</label>
          <div class="input-group">
            <input
              v-for="(number, index) in creditCard.number"
              v-model.trim="number.value"
              ref="numberRef"
              type="tel"
              required
              maxlength="4"
              aria-label="Credit Card First 4 Digits"
              :id="`cc-${index + 1}`"
              pattern="[0-9]{4}"
              @keypress="handlerCardNumber"
              @input="handlerCardNumberChange(number.value)"
            >
          </div>
        </fieldset>
        <div class="card-detail">
          <div class="card-user">
            <label for="username">NAME</label>
            <input
              ref="usernameRef"
              v-model="creditCard.username"
              @keyup.tab="focusNext"
              type="text"
              id="username"
              aria-label="Credit Card Username"
              required
            >
          </div>
          <fieldset class="card-date">
            <legend>EXPIRATION</legend>
            <label for="expiration">EXPIRATION</label>
            <div>
              <select required v-model="creditCard.date.month" id="expiration" class="month">
                <option v-for="item in month" :value="item">{{item}}</option>
              </select>
              <select
                required
                v-model.number="creditCard.date.year"
                class="year"
                @change="cardCvcRef.focus()"
              >
                <option v-for="item in years" :value="item">{{item}}</option>
              </select>
            </div>
          </fieldset>
        </div>
        <span class=" ring"></span>
        <span class=" ring"></span>
      </div>
    </form>
    <div>
      <p>Credit Number: {{ creditCard.number[0].value }} {{ creditCard.number[1].value }} {{ creditCard.number[2].value }} {{ creditCard.number[3].value }}</p>
      <p>Name: {{ creditCard.username }}</p>
      <p>Date: {{ creditCard.date.month }} / {{ creditCard.date.year }}</p>
      <p>CVC: {{ creditCard.cvc }}</p>
    </div>
  </main>
</template>

<script setup>
import { reactive, ref } from 'vue'

const years = [2022, 2023, 2024]
const month = ['01', '02', '03', '04', '05', '06', '07', '08', '09', '10', '11', '12']

const numberRef = ref([])
const usernameRef = ref(null)
const handlerCardNumberChange = value => {
  if (value.length >= 4) {
    const notField = numberRef.value.find(item => item.value.length < 4)
    if (notField === undefined) {
      usernameRef.value.focus()
    } else {
      notField.focus()
    }
  }
}
const handlerCardNumber = el => {
  const key = parseInt(el.key)

  if (isNaN(key) || key > 9 || key < 0) {
    return el.preventDefault()
  }
}

const cardCvcRef = ref(null)

const creditCard = reactive({
  number: [{ value: '' }, { value: '' }, { value: '' }, { value: '' }],
  username: '',
  date: {
    month: '01',
    year: 2022
  },
  cvc: ''
})
</script>

<style lang="scss" scoped>
main {
  height: 100vh;
  display: grid;
  place-items: center;
  color: #fff;
  background-image: linear-gradient(to top, #ff9a9e 0%, #fecfef 99%, #fecfef 100%);
  .container {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    grid-template-rows: repeat(7, 1fr);
    .card {
      width: 400px;
      height: 200px;
      background-color: #0C6B94;
      border-radius: 1rem;
      border: 1px solid #222;
      box-sizing: border-box;
    }
    .front {
      grid-column: 1 / 7;
      grid-row: 1 / 7;
      display: flex;
      flex-flow: column nowrap;
      justify-content: space-between;
      padding: 1rem;
      position: relative;
      overflow: hidden;
      .row {
        display: flex;
        flex-flow: row nowrap;
        align-items: flex-start;
        justify-content: space-between;
        position: relative;
        h1{
          font-size: 1.5rem;
          font-weight: bold;
        }
        .card-logo {
          right: 0;
          top: 0;
          position: absolute;
          font-size: 4rem;
        }
      }
      .card-number {
        legend {
          font-size: 0;
        }
        label {
          line-height: 2;
        }
        .input-group {
          margin-bottom: .1rem;
          input {
            width: 2rem;
            border: none;
            outline: 1px solid #222;
            border-radius: .3rem;
            padding: .3rem;
            text-align: center;
            &:not(:last-child) {
              margin-right: 1rem;
            }
          }
        }
      }
      .card-detail {
        display: flex;
        flex-flow: row nowrap;
        justify-content: space-between;
        .card-user {
          width: 60%;
          label {
            display: block;
            margin-bottom: .3rem;
          }
          #username {
            width: 100%;
            border: none;
            outline: 1px solid #222;
            padding: .3rem;
            border-radius: .3rem;
          }
        }
        .card-date {
          legend {
            font-size: 0;
          }
          label {
            display: block;
            margin-bottom: .3rem;
          }
          .month,.year {
            appearance: none;
            padding: .3rem;
            border-radius: .3rem;
            border: none;
            outline: 1px solid #222;
            text-align: center;
          }
          .month {
            margin-right: 15px;
          }
        }
      }
      .ring {
        content: '';
        position: absolute;
        right: -30%;
        bottom: 30%;
        width: 20rem;
        height: 20rem;
        border-radius: 50%;
        background-color: transparent;
        border: 3rem solid #ffffff11;
        &:last-child {
          bottom: 0%;
          right: -70%;
          width: 40rem;
          height: 40rem;
          border: 2rem solid #ffffff11;
        }
      }
    }
    .end {
      grid-column: 2 / 8;
      grid-row: 2 / 8;
      .roll {
        background-color: #222;
        height: 3rem;
        margin: 2rem 0 1.5rem;
      }
      .card-cvc {
        width: 3.5rem;
        margin-left: auto;
        legend {
          font-size: 0;
        }
        label {
          display: block;
          margin-bottom: .3rem;
        }
        div {
          input {
            min-width: 0;
            width: 2.5rem;
            border: none;
            outline: 1px solid #222;
            padding: .3rem;
            border-radius: .3rem;
            text-align: center;
          }
        }
        
      }
    }
  }
}
</style>