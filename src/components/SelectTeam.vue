<template>
  <margin-div>
    <heading2>　チームを選択してね　</heading2>
    <div>
      <select class="form-control form-control-lg" :name="name" @change="updateValue">
        <option disabled selected value="">球団を選択してください</option>
        <template v-for="(option, index) in options">
          <option :key="index" :value="option.teamLabel">
            {{ option.teamLabel }}
          </option>
        </template>
      </select>
    </div>
  </margin-div>
</template>

<script>
import Heading2 from "./Heading2";
import MarginDiv from "./MarginDiv";

export default {
  name: "SelectTeam",
  props: {
    value: {
      type: String
    },
    options: {
      type: Array
    },
    name: {
      type: String
    },
  },
  methods: {
    updateValue: function(e) {
      this.$emit('input', e.target.value)
    }
  },
  components: {
    "heading2": Heading2,
    "margin-div": MarginDiv
  }
};
</script>