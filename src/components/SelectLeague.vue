<template>
  <margin-div>
    <heading2>　リーグを選択してね　</heading2>
    <div class="text-center">
      <template v-for="(option, index) in options">
        <div class="form-check form-check-inline" :key="index">
          <label class="form-check-label form-control-lg">
            <input
              class="form-check-input"
              type="radio"
              :name="name"
              :value="option.value"
              :checked="option.checked"
              @change="updateValue"
            />
            <span>{{ option.label }}</span>
          </label>
        </div>
      </template>
    </div>
  </margin-div>
</template>

<script>
import Heading2 from "./Heading2";
import MarginDiv from "./MarginDiv";

export default {
  name: "SelectLeague",
  props: {
    value: {
      type: String
    },
    options: {
      type: Array
    },
    name: {
      type: String
    }
  },
  methods: {
    updateValue: function(e) {
      this.$emit("input", e.target.value);
    }
  },
  components: {
    "heading2": Heading2,
    "margin-div": MarginDiv
  }
};
</script>