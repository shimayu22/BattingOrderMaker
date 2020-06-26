<template>
  <div class="text-center">
    <button 
      class="btn-danger btn-lg col-md-4" 
      @click="$emit('click')"
    >
      データをリセットする
    </button>
  </div>
</template>