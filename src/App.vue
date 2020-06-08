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
      <option v-for="(team, index) in centralLeague" :key="index" :value="team.jsonName">{{ team.displayName }}</option>
    </select>
    <select v-model="selectTeam" v-if="selectLeague === 'pacific'">
      <option disabled value="">球団を選択してください</option>
      <option v-for="(team, index) in pacificLeague" :key="index" :value="team.jsonName">{{ team.displayName }}</option>
    </select><br>
    <span>選択したのは{{ selectTeam }}です。</span>

    <h2>打順を組んでね</h2>
    <ol>
      <li v-for="(selectPlayer, index) in selectPlayers" :key="index">
        <select v-model="selectPlayer.position">
          <option disabled value="">ポジション</option>
          <option v-for="(position, index) in positions" :key="index">{{ position }}</option>
        </select>
        <select v-model="selectPlayer.player" v-if="selectTeam === 'fighters'">
          <option disabled value="">選手</option>
          <option v-for="(player, index) in fighters_players" :key="index" :value="player">{{ player.name }}</option>
        </select>
        {{ selectPlayer }}
      </li>
    </ol>
  </div>
</template>

<script>
import giants from '../src/assets/giants.json'
import baystars from '../src/assets/baystars.json'
import tigers from '../src/assets/tigers.json'
import carp from '../src/assets/carp.json'
import dragons from '../src/assets/dragons.json'
import swallows from '../src/assets/swallows.json'
import lions from '../src/assets/lions.json'
import hawks from '../src/assets/hawks.json'
import eagles from '../src/assets/eagles.json'
import marines from '../src/assets/marines.json'
import fighters from '../src/assets/fighters.json'
import buffaloes from '../src/assets/buffaloes.json'

export default {
  name: 'app',
  data () {
    return {
      selectLeague: 'central',
      centralLeague: [
                        {'jsonName': 'giants', 'displayName': '読売ジャイアンツ', 'hashTag': '#giants'},
                        {'jsonName': 'baystars', 'displayName': '横浜DeNAベイスターズ', 'hashTag': '#baystars'},
                        {'jsonName': 'tigers', 'displayName': '阪神タイガース', 'hashTag': '#tigers'},
                        {'jsonName': 'carp', 'displayName': '広島東洋カープ', 'hashTag': '#carp'},
                        {'jsonName': 'dragons', 'displayName': '中日ドラゴンズ', 'hashTag': '#dragons'},
                        {'jsonName': 'swallows', 'displayName': '東京ヤクルトスワローズ', 'hashTag': '#swallows'}
                      ],
      pacificLeague: [
                        {'jsonName': 'lions', 'displayName': '埼玉西武ライオンズ', 'hashTag': '#seibulions'}, 
                        {'jsonName': 'hawks', 'displayName': '福岡ソフトバンクホークス', 'hashTag': '#sbhawks'},
                        {'jsonName': 'eagles', 'displayName': '東北楽天ゴールデンイーグルス', 'hashTag': '#RakutenEagles'},
                        {'jsonName': 'marines', 'displayName': '千葉ロッテマリーンズ', 'hashTag': '#chibalotte'},
                        {'jsonName': 'fighters', 'displayName': '北海道日本ハムファイターズ', 'hashTag': '#lovefighters'},
                        {'jsonName': 'buffaloes', 'displayName': 'オリックス・バファローズ', 'hashTag': '#Bs2020'}
                      ],
      selectTeam: '',
      fighters_players: fighters,
      positions: ['投', '捕', '一', '二', '三', '遊', '左', '中', '右', 'DH'],
      selectPlayers: [
                        {'order': 1, 'position': '', 'player': ''},
                        {'order': 2, 'position': '', 'player': ''},
                        {'order': 3, 'position': '', 'player': ''},
                        {'order': 4, 'position': '', 'player': ''},
                        {'order': 5, 'position': '', 'player': ''},
                        {'order': 6, 'position': '', 'player': ''},
                        {'order': 7, 'position': '', 'player': ''},
                        {'order': 8, 'position': '', 'player': ''},
                        {'order': 9, 'position': '', 'player': ''}
                      ]
    }
  }
}
</script>

