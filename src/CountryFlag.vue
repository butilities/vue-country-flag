<template>
  <span :class="flagIconClass" v-if="country"></span>
</template>

<script>
import "./css/flags/flags.min.css";
const iso = require("iso-3166-1");

export default {
  name: "CountryFlag",
  props: {
    country: {
      type: String,
      required: true,
      validator: function(value) {
        return value.length >= 2;
      }
    },
    size: {
      type: String,
      validator: function(value) {
        return value === "small" || value === "normal" || value === "big";
      },
      default: "normal"
    }
  },
  computed: {
    flagIconClass() {
      return {
        [`flag ${this.flagIconCountry}`]: true,
        [this.flagMargin]: true
      };
    },
    flagIconCountry() {
      if (this.country.length > 3) {
        this.country = (
          iso.whereCountry(this.country.toUpperCase()) || {}
        ).alpha2;
      }
      return `flag-${this.country.toLowerCase()}`;
    },
    flagMargin() {
      switch (this.size) {
        case "small":
          return "small-flag";
        case "normal":
          return "normal-flag";
        case "big":
          return "big-flag";
        default:
          return "normal-flag";
      }
    }
  }
};
</script>
<style scoped>
.small-flag {
  margin-left: -24px;
  margin-right: -24px;
  transform: scale(0.25);
  -ms-transform: scale(0.25);
  -webkit-transform: scale(0.25);
  -moz-transform: scale(0.25);
}
.normal-flag {
  margin-left: -16px;
  margin-right: -16px;
  transform: scale(0.5);
  -ms-transform: scale(0.5);
  -webkit-transform: scale(0.5);
  -moz-transform: scale(0.5);
}
.big-flag {
  margin-right: 0;
  margin-left: 0;
  transform: scale(1);
  -ms-transform: scale(1);
  -webkit-transform: scale(1);
  -moz-transform: scale(1);
}
</style>
