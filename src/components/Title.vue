<template>
  <div class="text-center">
    <img src="../assets/title_logo.png" alt="ぼく将オーダー" class="img-fluid">
    <horizontal></horizontal>
  </div>
</template>

<script>
import Horizontal from './Horizontal';

export default {
  name: 'Title',
  components: {
    'horizontal': Horizontal
  }
}
</script>