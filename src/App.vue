<template>
  <div id="app">
    <h1>ぼくのかんがえたさいきょーのおーだー2020</h1>
    <h2>監督名を入れてね（任意）</h2>
    <input v-model="userName">監督

    <h2>リーグを選んでね</h2>
    <label><input type="radio" id="central" value="central" v-model="selectLeague" checked>セ・リーグ</label>
    <label><input type="radio" id="pacific" value="pacific" v-model="selectLeague">パ・リーグ</label>

    <h2>球団を選んでね</h2>
    <select v-model="selectTeam">
      <option disabled value="">球団を選択してください</option>
      <option v-for="(team, index) in npb[selectLeague]" :key="index" :value="team">{{ team.displayName }}</option>
    </select>

    <h2>打順を組んでね</h2>
    <ol>
      <li v-for="(selectPlayer, index) in selectPlayers" :key="index">
        <select v-model="selectPlayer.position" :class="{ duplicated: selectPlayer.duplicatedPosition }">
          <option disabled value="">ポジション</option>
          <option v-for="(position, index) in positions" :key="index">{{ position }}</option>
        </select>
        <select v-model="selectPlayer.player" :class="{ duplicated: selectPlayer.duplicatedPlayer }">
          <option disabled value="">選手</option>
          <option v-for="(player, index) in npbPlayers[selectTeam.jsonName]" :key="index" :value="player">{{ player.name }}</option>
        </select>
      </li>
    </ol>
    <h3>プレビュー</h3>
    <div class="preview">
      <div class="preview-inner">
        <div class="team-name"> {{ selectTeam['displayName'] }} </div>
        <div class="user-name"> {{ userName }}</div>

        <div class="position-1">{{ selectPlayers[0]['position'] }}</div>
        <div class="player-name-1">{{ selectPlayers[0]['player']['name'] }}</div>
        <div class="id-1">{{ selectPlayers[0]['player']['id'] }}</div>
        <div class="bt-1">{{ selectPlayers[0]['player']['bt'] }}</div>


        
      </div>
    </div>
  </div>
</template>

<style type="text/css">
.duplicated {
  border: 2px dashed red;
}

.preview {
  overflow: auto;
  width: 100%;
}

.preview-inner {
  background: url(/member.png) no-repeat left bottom;
  background-size: 400px 400px;
  position: relative;
  padding-top: 60px;
  padding-bottom: 340px;
  width: 400px;
}

.team-name {
  position: absolute;
  top: 35px;
  left: 8px;
}

.user-name {
  position: absolute;
  top: 59px;
  left: 60px;
  font-size: 12px;
}

.position-1 {
  position: absolute;
  top: 107px;
  left: 68px;
}

.player-name-1 {
  position: absolute;
  top: 107px;
  left: 170px;
}

.id-1 {
  position: absolute;
  top: 107px;
  left: 320px;
}

.bt-1 {
  position: absolute;
  top: 107px;
  left: 355px;
}

</style>

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
      userName: '名無しさん',
      selectLeague: 'central',
      npb: {'central': [
                          {'jsonName': 'giants', 'displayName': '読売ジャイアンツ', 'hashTag': '#giants'},
                          {'jsonName': 'baystars', 'displayName': '横浜DeNAベイスターズ', 'hashTag': '#baystars'},
                          {'jsonName': 'tigers', 'displayName': '阪神タイガース', 'hashTag': '#tigers'},
                          {'jsonName': 'carp', 'displayName': '広島東洋カープ', 'hashTag': '#carp'},
                          {'jsonName': 'dragons', 'displayName': '中日ドラゴンズ', 'hashTag': '#dragons'},
                          {'jsonName': 'swallows', 'displayName': '東京ヤクルトスワローズ', 'hashTag': '#swallows'}
                       ],
            'pacific': [
                          {'jsonName': 'lions', 'displayName': '埼玉西武ライオンズ', 'hashTag': '#seibulions'}, 
                          {'jsonName': 'hawks', 'displayName': '福岡ソフトバンクホークス', 'hashTag': '#sbhawks'},
                          {'jsonName': 'eagles', 'displayName': '東北楽天ゴールデンイーグルス', 'hashTag': '#RakutenEagles'},
                          {'jsonName': 'marines', 'displayName': '千葉ロッテマリーンズ', 'hashTag': '#chibalotte'},
                          {'jsonName': 'fighters', 'displayName': '北海道日本ハムファイターズ', 'hashTag': '#lovefighters'},
                          {'jsonName': 'buffaloes', 'displayName': 'オリックス・バファローズ', 'hashTag': '#Bs2020'}
                       ]
           },
      selectTeam: {'jsonName': 'giants', 'displayName': '読売ジャイアンツ', 'hashTag': '#giants'},
      npbPlayers: {'giants':giants, 'baystars': baystars, 'tigers': tigers, 'carp': carp, 'dragons': dragons, 'swallows': swallows,
                    'lions': lions, 'hawks': hawks, 'eagles':eagles, 'marines': marines, 'fighters': fighters, 'buffaloes': buffaloes},
      positions: ['投', '捕', '一', '二', '三', '遊', '左', '中', '右', 'Ｄ'],
      selectPlayers: [
                        {'order': 1, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false},
                        {'order': 2, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false},
                        {'order': 3, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false},
                        {'order': 4, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false},
                        {'order': 5, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false},
                        {'order': 6, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false},
                        {'order': 7, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false},
                        {'order': 8, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false},
                        {'order': 9, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false}
                     ]
    }
  },
  watch: {
    selectPlayers: {
      handler: function() {
        let positions = [];
        let players = [];
        this.selectPlayers.filter(e => {
          if (e["position"].length > 0) {
            if (positions.indexOf(e["position"]) === -1) {
              positions.push(e["position"]);
              e["duplicatedPosition"] = false;
            }else{
              e["duplicatedPosition"] = true;
            }
          }
          if (e['player'] !== '') {
            if (players.indexOf(e["player"]["name"]) === -1) {
              players.push(e["player"]["name"]);
              e["duplicatedPlayer"] = false;
            }else{
              e["duplicatedPlayer"] = true;
            }
          }
          return e;
        });
      },
      deep: true
    }
  }
}
</script>

