<template>
  <div id="app" class="container">
    <h1 class="text-center">ぼく将おーだー2020（beta）</h1>
    <hr class="pt-1 pb-2">
    <h2 class="pb-2">監督名を入れてね（任意）</h2>
    <div class="input-group">
      <input class="form-control col" v-model="userName">
      <div class="input-group-append"><span class="input-group-text">監督</span></div>
    </div>

    <h2 class="pt-4 pb-2">リーグを選んでね</h2>
    <div class="form-check form-check-inline ">
      <label class="form-check-label">
        <input class="form-check-input" type="radio" id="central" value="central" v-model="selectLeague" checked>
        <span class="leagu-name">セ・リーグ</span>
      </label>
    </div>
    <div class="form-check form-check-inline">
      <label class="form-check-label">
        <input class="form-check-input" type="radio" id="pacific" value="pacific" v-model="selectLeague">
        <span class="leagu-name">パ・リーグ</span>
        </label>
    </div>

    <h2 class="pt-4 pb-2">球団を選んでね</h2>
    <select v-model="selectTeam" class="custom-select-lg col">
      <option disabled value="">球団を選択してください</option>
      <option v-for="(team, index) in npb[selectLeague]" :key="index" :value="team">{{ team.displayName }}</option>
    </select>

    <h2 class="pt-4 pb-2">打順を組んでね</h2>
    <p>※重複するとその箇所が赤くなりますが、エラーにはなりません。</p>
    <ol>
      <li v-for="(selectPlayer, index) in selectPlayers" :key="index">
        <select class="custom-select col" v-model="selectPlayer.position" :class="{ duplicated: selectPlayer.duplicatedPosition }">
          <option disabled value="">守備</option>
          <option v-for="(position, index) in positions" :key="index">{{ position }}</option>
        </select>
        <select class="custom-select col" v-model="selectPlayer.player" :class="{ duplicated: selectPlayer.duplicatedPlayer }">
          <option disabled value="">選手</option>
          <option v-for="(player, index) in npbPlayers[selectTeam.jsonName]" :key="index" :value="player">{{ player.name }}</option>
        </select>
      </li>
    </ol>
    <h4  class="pt-4 pb-2">プレビューだよ</h4>
    <div id="preview" class="pb-4">
      <div id="preview-inner">
        <div id="team-name"> {{ selectTeam.displayName }} </div>
        <div id="user-name"> {{ userName }}</div>
        <div v-for="(player, index) in selectPlayers" :key="index">
          <div class="position" :style="{ top: player.top }">{{ player['position'] }}</div>
          <div class="player-name" :style="{ top: player.top }">{{ player.player.name }}</div>
          <div class="number" :style="{ top: player.top }">{{ player.player.id }}</div>
          <div class="bt" :style="{ top: player.top }">{{ player.player.bt }}</div>
        </div>
      </div>
    </div>
    <button class="btn-primary btn-lg" @click="generate">画像を生成</button>
    <hr>
    <p id="result"></p>
    <button class="btn-primary btn-lg" @click="download">画像をダウンロード(PC用)</button>
    <h4 class="pt-2 pb-2">ここからツイートしてね</h4>
  </div>
</template>

<style type="text/css">
.duplicated {
  border: 2px dashed red;
}

.leagu-name {
  font-size: 14pt;
}

#preview {
  overflow: auto;
  width: 500px;
}

#preview-inner {
  background: url(/member.png) no-repeat left bottom;
  background-size: 500px 500px;
  position: relative;
  width: 500px;
  height: 500px;
}

#team-name {
  position: absolute;
  top: 35px;
  left: 8px;
}

#user-name {
  position: absolute;
  top: 59px;
  left: 60px;
  font-size: 12px;
}

.position {
  position: absolute;
  left: 68px;
}

.player-name {
  position: absolute;
  left: 170px;
}

.number {
  position: absolute;
  left: 320px;
}

.bt {
  position: absolute;
  left: 355px;
}

</style>

<script>
import giants from '../src/assets/giants.json';
import baystars from '../src/assets/baystars.json';
import tigers from '../src/assets/tigers.json';
import carp from '../src/assets/carp.json';
import dragons from '../src/assets/dragons.json';
import swallows from '../src/assets/swallows.json';
import lions from '../src/assets/lions.json';
import hawks from '../src/assets/hawks.json';
import eagles from '../src/assets/eagles.json';
import marines from '../src/assets/marines.json';
import fighters from '../src/assets/fighters.json';
import buffaloes from '../src/assets/buffaloes.json';
import html2canvas from 'html2canvas';

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
      selectTeam: '',
      npbPlayers: {'giants':giants, 'baystars': baystars, 'tigers': tigers, 'carp': carp, 'dragons': dragons, 'swallows': swallows,
                    'lions': lions, 'hawks': hawks, 'eagles':eagles, 'marines': marines, 'fighters': fighters, 'buffaloes': buffaloes},
      positions: ['投', '捕', '一', '二', '三', '遊', '左', '中', '右', 'Ｄ'],
      selectPlayers: [
                        {'order': 1, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '100px'},
                        {'order': 2, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '130px'},
                        {'order': 3, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '160px'},
                        {'order': 4, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '190px'},
                        {'order': 5, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '220px'},
                        {'order': 6, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '250px'},
                        {'order': 7, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '280px'},
                        {'order': 8, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '310px'},
                        {'order': 9, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '340px'}
                     ]
    }
  },
  methods: {
    generate() {
      html2canvas(document.getElementById('preview-inner')).then(
        function(canvas){
          let img = document.createElement("img");
          img.src = canvas.toDataURL('image/png');
          img.width = "500";
          img.height = "500";
          
          let parentElement = document.getElementById("result").parentNode;
          let p = document.getElementById("result");
          parentElement.insertBefore(img, p);
        }
      )
    },
    download() {
      html2canvas(document.getElementById('preview-inner')).then(
        function(canvas){
          let link = document.createElement("a");
          link.href = canvas.toDataURL('image/png');
          link.download = "myorder.png";
          link.click();
        }
      )
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

