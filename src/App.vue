<template>
  <div id="app">
      <h1>ぼくのかんがえたさいきょーのおーだー2020</h1>
      <h2>リーグを選んでね</h2>
      <label><input type="radio" id="central" value="central" v-model="selectLeague" checked>セ・リーグ</label>
      <label><input type="radio" id="pacific" value="pacific" v-model="selectLeague">パ・リーグ</label><br>
      <span>選択したのは{{ selectLeague }}です。</span>
      <h2>球団を選んでね</h2>
      <select v-model="selectTeam" v-if="selectLeague === 'central'">
          <option disabled value="">球団を選択してください</option>
          <option v-for="(team, index) in centralLeague" :key="index">{{ team }}</option>
      </select>
      <select v-model="selectTeam" v-if="selectLeague === 'pacific'">
          <option disabled value="">球団を選択してください</option>
          <option v-for="(team, index) in pacificLeague" :key="index">{{ team }}</option>
      </select><br>
      <span>選択したのは{{ selectTeam }}です。</span>
      <h2>打順を組んでね</h2>
      <ol>
        <li v-for="(n, index) in 9" :key="index">
          <select v-model="selectPosition">
            <option disabled value="">ポジション</option>
            <option v-for="(position, index) in positions" :key="index">{{ position }}</option>
          </select>
          <select v-model="selectPlayer">
            <option disabled value="">選手</option>
            <option v-for="(player, index) in players" :key="index">{{ player }}</option>
          </select>
        </li>
      </ol>
    </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      selectLeague: 'central',
      centralLeague: ['読売ジャイアンツ', '横浜DeNAベイスターズ', '阪神タイガース', '広島東洋カープ', '中日ドラゴンズ', '東京ヤクルトスワローズ'],
      pacificLeague: ['埼玉西武ライオンズ', '福岡ソフトバンクホークス', '東北楽天ゴールデンイーグルス', '千葉ロッテマリーンズ', '北海道日本ハムファイターズ', 'オリックス・バファローズ'],
      selectTeam: '',
      positions: ['投', '捕', '一', '二', '三', '遊', '左', '中', '右', 'DH'],
      players: ['清水優心', '中田翔', '渡邉諒', '平沼翔太', '中島卓也', '王柏融', '西川遥輝', '大田泰示', '近藤健介']
    }
  }
}
</script>

