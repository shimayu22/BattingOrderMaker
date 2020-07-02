<template>
  <margin-div>
    <heading3>ここからツイートしてね</heading3>
    <div>
      <button 
        class="btn btn-block btn-social btn-twitter btn-lg"
        @click="$emit('click')"
      >
        <i class="fa fa-twitter"></i>打順をツイートする
      </button>
    </div>
  </margin-div>
</template>

<script>
import Heading3 from "./Heading3";
import MarginDiv from "./MarginDiv";

export default {
  name: "TwitterButton",
  components: {
    "heading3": Heading3,
    "margin-div": MarginDiv
  }
};
</script>