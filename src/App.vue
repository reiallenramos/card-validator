<template lang="pug">
  v-app#app
    v-content
      v-row(justify="center" align="center")
        v-container.px-10.py-10
          v-row
            v-col.col-12
              v-row
                v-col(cols="12").py-7.col-xs-12.col-sm-5
                  v-container
                    v-form
                      v-text-field(v-model="cardNumber"
                                    label="Card Number"
                                    v-mask="'#### #### #### ####'"
                                    :rules="rules"
                                    outlined
                                    @keyup="identifyCard")
                        v-icon(slot="append" :color="iconColor") {{ appendIcon }}
                v-col.col-xs-12.col-sm-7
                  v-container
                    v-row(justify="center")
                      v-col.col-12
                        .grid
                          img(src="./assets/visa.png").card#visa
                          img(src="./assets/mastercard.png").card#mastercard
                          img(src="./assets/american_express.png").card#american-express
                          img(src="./assets/discover.png").card#discover
                          img(src="./assets/jcb.png").card#jcb
                          img(src="./assets/diners_club.png").card#dinersclub
                          img(src="./assets/maestro.png").card#maestro
            v-col.col-12
              luhn-formula(:num="cardNumber")
    v-btn(fixed dark bottom right fab color="indigo")
      v-icon mdi-help
</template>

<script>
import CardValidator from 'card-validator'
import Vue from 'vue'
import VueMask from 'v-mask'
import Isotope from 'isotope-layout'
import LuhnFormula from './components/LuhnFormula'

Vue.use(VueMask)

export default {
  data () {
    return {
      cardNumber: '',
      iso: null,
      rules: [
        value => !!value || 'Required',
        () => this.result.isValid || 'Invalid',
      ]
    }
  },
  components: {
    CardValidator,
    VueMask,
    LuhnFormula,
  },
  computed: {
    result: function () {
      return CardValidator.number(this.cardNumber);
    },
    appendIcon () {
      return this.result.isValid ? 'mdi-check-underline' : 'mdi-credit-card-outline'
    },
    iconColor() {
      if (this.cardNumber == '') return ''
      return this.result.isValid ? 'green' : 'red'
    }
  },
  created () {
    this.$vuetify.theme.dark = true
  },
  mounted() {
    this.iso = new Isotope( '.grid', {
      itemSelector: '.card',
      masonry: {
        columnWidth: 15,
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
  font-family: Roboto, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
