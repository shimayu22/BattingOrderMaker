<template>
  <margin-div>
    <div class="row justify-content-center">
      <div>
        <heading2>リーグを選んでね</heading2>
          <template v-for="(option, index) in options">
            <div class="form-check form-check-inline" :key="index">
              <label class="form-check-label">
                <input
                  class="form-check-input"
                  type="radio"
                  :name="name"
                  :value="option.value"
                  :checked="option.checked"
                  @change="updateValue"
                />
                <span class="leagu-name">{{ option.label }}</span>
              </label>
            </div>
          </template>
      </div>
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