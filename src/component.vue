<template>
  <input type="tel"
        :id="id"
        :value="formattedValue"
        @change="change"
        @blur="blur"
        v-money="{precision, decimal, thousands, prefix, suffix, disableNegative, min, max}"
        class="v-money" />
</template>

<script>
import money from './directive'
import defaults from './options'
import {format, unformat} from './utils'

export default {
  name: 'Money',
  props: {
    id: {
      required: false,
      type: [Number, String],
      default: 0
    },
    value: {
      required: true,
      type: [Number, String],
      default: 0
    },
    masked: {
      type: Boolean,
      default: false
    },
    precision: {
      type: Number,
      default: () => defaults.precision
    },
    decimal: {
      type: String,
      default: () => defaults.decimal
    },
    thousands: {
      type: String,
      default: () => defaults.thousands
    },
    prefix: {
      type: String,
      default: () => defaults.prefix
    },
    suffix: {
      type: String,
      default: () => defaults.suffix
    },
    disableNegative: {
      type: Boolean,
      default: false
    },
    max: {
      type: Number,
      default: () => defaults.max
    },
    min: {
      type: Number,
      default: () => defaults.min
    },
  },

  directives: {money},

  data () {
    return {
      formattedValue: ''
    }
  },

  watch: {
    value: {
      immediate: true,
      handler (newValue, oldValue) {
        var formatted = format(newValue, this.$props)
        if (formatted !== this.formattedValue) {
          this.formattedValue = formatted
        }
      }
    }
  },

  methods: {
    change (evt) {
      this.$emit('input', this.masked ? evt.target.value : unformat(evt.target.value, this))
    },

    blur(evt) {
      this.$emit('blur', evt)
    }

  }
}
</script>
