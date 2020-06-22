<template>
  <button 
    class="btn-danger btn-lg col-md-4" 
    @click="$emit('click')"
  >
    データをリセットする
  </button>
</template>