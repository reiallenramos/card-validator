<template lang='pug'>
  v-dialog(:value="dialog"
            @input="$emit('handleCloseDialog')"
            max-width="600px"
            )
    v-card
      v-card-title.headline(style="word-break: normal") Reluctant to put your card number?
      v-card-text Relax. It's useless without the expiration date and the funny digits in the back.
      v-card-text Fun fact: credit card number validation goes all the way back to the Triassic period which uses an ancient technique called Math.
      v-card-text Nah, it's called Luhn algorithm.
      v-card-actions
        v-spacer
        v-btn(color="green darken-1" text href="https://www.freeformatter.com/credit-card-number-generator-validator.html" target="_blank") Learn more
</template>

<script>
export default {
  props: {
    dialog: Boolean,
    handleCloseDialog: String
  },
}
</script>