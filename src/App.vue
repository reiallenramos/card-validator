<template lang="pug">
  #app
    input(v-model="cardNumber" type="text" v-mask="'#### #### #### ####'" @keyup="identifyCard")
    div(v-if="cardNumber == ''") Please input your card number
    div(v-else)
      div(v-if="!result.isValid") Invalid card
      template(v-if="result.isPotentiallyValid")
        table(v-if="result.isValid")
          tbody
            tr
              th niceType
              th type
              th code
            tr
              td {{ result.card.niceType }}
              td {{ result.card.type }}
              td {{ result.card.code.name }}
    div.grid
      .card#visa Visa
      .card#mastercard Mastercard
      .card#american-express American Express
      .card#discover Discover
      .card#jcb JCB
      .card#dinersclub Diners Club
      .card#maestro Maestro
</template>

<script>
import CardValidator from 'card-validator'
import Vue from 'vue'
import VueMask from 'v-mask'
import Isotope from 'isotope-layout'

Vue.use(VueMask)

export default {
  data () {
    return {
      cardNumber: '',
      iso: null,
    }
  },
  components: {
    CardValidator,
    VueMask,
  },
  computed: {
    result: function () {
      return CardValidator.number(this.cardNumber);
    }
  },
  mounted() {
    this.iso = new Isotope( '.grid', {
      itemSelector: '.card',
      masonry: {
        columnWidth: 100,
      }
    });
  },
  methods: {
    identifyCard: function() {
      let niceType = this.result?.card?.niceType
      let filter = '*'

      if (niceType) {
        let type = niceType.toLowerCase().replace(/\s/g, '')
        filter = `#${type}`
      }
      this.iso.arrange({ filter })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
