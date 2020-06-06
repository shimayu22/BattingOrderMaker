<template>
  <div>
      <h1>ぼくのかんがえたさいきょーのおーだー2020</h1>
      <h2>リーグを選んでね</h2>
      <input type="radio" name="select_league" id="central" value="セ・リーグ" v-model="league" checked>
      <label for="central">セ・リーグ</label>
      <input type="radio" name="select_league" id="pacific" value="パ・リーグ" v-model="league">
      <label for="pacific">パ・リーグ</label><br>
      <span>選択したのは{{ league }}です。</span>
      <h2>球団を選んでね</h2>
      <h2>打順を組んでね</h2>
    </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      league: ''
    }
  }
}
</script>

