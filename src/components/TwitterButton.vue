<template>
  <div class="mt-1 mb-1">
    <div class="row justify-content-center">
      <div>
        <heading3>ここからツイートしてね</heading3>
        <div>
          <button 
            class="btn btn-block btn-social btn-twitter btn-lg"
            @click="$emit('click')"
          >
            <i class="fa fa-twitter"></i>
            <div class="text-center">
              打順をツイートする
            </div>
          </button>
        </div>
      </div>
    </div>
  </div>
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