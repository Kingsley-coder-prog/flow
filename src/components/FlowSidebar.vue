<template>
  <div>
    <h2 :class="textStyle">Welcome to Vahalah</h2>
  </div>
  <input v-model="message" placeholder="Enter message" />
  <p>Message is: {{ message }}</p>
  <select v-model="selected">
    <option disabled value="">Choose a fruit</option>
    <option>Orange</option>
    <option>Banana</option>
    <option>Orange</option>
  </select>
  <span>Fruit chosen: {{ selected }}</span>
  <a v-bind:ref="'https://' + domain + path">{{ linkText }}</a>

  <!-- Watchers -->
  <div>
    <p>My name is: {{ name }}</p>
    <button @click="nameChange()">Change Name</button>
  </div>
</template>

<script>
export default {
  props: {
    name: {
      type: String,
      required: true,
      default: {
        firstName: "John",
        lastName: "",
      },
    },
  },
  data() {
    return {
      isUnderline: true,
      isBold: true,
      name: "Flavio",
    };
  },
  computed: {
    textStyle: function () {
      return {
        underline: this.isUnderline,
        bold: this.isBold,
      };
    },
  },
  methods: {
    nameChange: function () {
      this.name = "Flavius";
    },
  },
  watch: {
    name: function () {
      console.log(this.name);
    },
  },
};
</script>

<style>
.underline {
  text-decoration: underline;
}
.bold {
  font-weight: bold;
}
</style>