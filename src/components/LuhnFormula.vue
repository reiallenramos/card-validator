<template lang='pug'>
  v-card
    v-card-title How it works:
    v-simple-table
      thead
        th.text-left Step
        th(colSpan="16")
        th.text-left Total
      tbody
        tr
          td Original number:
          template(v-for="digit in step0Digits")
            td {{ digit }}
          td(v-for="n in padTableCount(17, step0Digits.length)")
        tr
          td Drop the last digit:
          template(v-for="digit in step1Digits")
            td {{ digit }}
          td(v-for="n in padTableCount(17, step1Digits.length)")
        tr
          td Reverse the digits:
          template(v-for="digit in step2Digits")
            td {{ digit }}
          td(v-for="n in padTableCount(17, step2Digits.length)")
        tr
          td Multiple odd digits by 2:
          template(v-for="digit in step3Digits")
            td {{ digit }}
          td(v-for="n in padTableCount(17, step3Digits.length)")
        tr
          td Subtract 9 to numbers over 9:
          template(v-for="digit in step4Digits")
            td {{ digit }}
          td(v-for="n in padTableCount(17, step4Digits.length)")
        tr
          td Add all numbers:
          template(v-for="digit in step5Digits")
            td {{ digit }}
          td(v-for="n in padTableCount(17, step5Digits.length)")
        tr
          td Mod 10:
          td(colSpan="15") {{ step6.checksum }} modulo 10 = {{ step6.lastDigit }}
          td(colSpan="2") {{ step6.lastDigit }}
</template>

<script>
export default {
  props: {
    num: String
  },
  methods: {
    padArray(arr, len ,fill) {
      return arr.concat(Array(len).fill(fill)).slice(0, len)
    },
    padTableCount(total, currentDigits) {
      return total - currentDigits
    }
  },
  computed: {
    step0Digits() {
      if (this.num) {
        return this.num.split('').filter(digit => digit != ' ')
      } else {
        return []
      }
    },
    step1Digits() {
      let og = [...this.step0Digits]
      og.pop()
      return og
    },
    step2Digits() {
      let res = this.step1Digits
      return res.reverse()
    },
    step3Digits() {
      let res = this.step2Digits
      res.forEach((digit, index) => {
        if ((index + 1) % 2) { // because arrays start at 0
          res[index] = digit * 2;
        }
      })
      return res
    },
    step4Digits() {
      let res = this.step3Digits
      res.forEach((digit, index) => {
        if (digit > 9) { 
          res[index] = digit - 9;
        }
      })
      return res
    },
    step5Digits() {
      let og = this.step4Digits
      this.step4Digits.forEach((digit, index) => {
        og[index] = parseInt(digit)
      })
      let newNumber = og.reduce((a,b) => a + b, 0)
      return this.padArray(og, 16, ' ').concat(newNumber)
    },
    step6() {
      let step5Digits = [...this.step5Digits]
      let og = [...this.step0Digits]
      let lastDigit = og.pop()
      let checksum = step5Digits.pop()
      return { checksum, lastDigit }
    },
  }
}
</script>