<template>
  <div class="row justify-content-center">
    <div>
      <heading2>監督名を入れてね（任意）</heading2>
      <div class="input-group">
        <input class="form-control" placeholder="ななしさん" :value="value" @input="$emit('input', $event.target.value)">
        <div class="input-group-append"><span class="input-group-text">監督</span></div>
      </div>
    </div>
  </div>
</template>

<script>
import Heading2 from './Heading2';

export default {
  name: 'InputName',
  props: {
    value: {
      type: String
    }
  },
  components: {
    'heading2': Heading2
  }
}
</script>