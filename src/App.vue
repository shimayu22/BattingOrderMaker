<template>
  <div id="app">
    <div class="text-center">
      <div class="row justify-content-center">
        <div>
          <h2 class="pb-2">監督名を入れてね（任意）</h2>
          <div class="input-group">
            <input class="form-control" v-model="userName">
            <div class="input-group-append"><span class="input-group-text">監督</span></div>
          </div>
        </div>
      </div>

      <h2 class="pt-4 pb-2">リーグを選んでね</h2>
      <div class="form-check form-check-inline">
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
      <select v-model="selectTeam" class="custom-select-lg col-4">
        <option disabled value="" class="text-center">球団を選択してください</option>
        <option v-for="(team, index) in npb[selectLeague]" :key="index" :value="team">{{ team.displayName }}</option>
      </select>

      <h2 class="pt-4 pb-2">打順を組んでね</h2>
      <p>※重複するとその箇所が赤くなりますが、エラーにはなりません。</p>
      <div>
        <div v-for="(selectPlayer, index) in selectPlayers" :key="index" class="mt-2">
          {{ index + 1 }}　：　
          <select class="custom-select col-1" v-model="selectPlayer.position" :class="{ duplicated: selectPlayer.duplicatedPosition }">
            <option disabled value="">守備</option>
            <option v-for="(position, index) in positions" :key="index">{{ position }}</option>
          </select>
          　
          <select class="custom-select col-4" v-model="selectPlayer.player" :class="{ duplicated: selectPlayer.duplicatedPlayer }">
            <option disabled value="">選手</option>
            <option v-for="(player, index) in npbPlayers[selectTeam.jsonName]" :key="index" :value="player">{{ player.name }}</option>
          </select>
        </div>
      </div>

      <h4  class="pt-4 pb-2">プレビューだよ</h4>
      <div class="row justify-content-center">
        <div>
          <div id="preview" class="pb-4">
            <div id="preview-inner">
              <div id="team-name"> {{ selectTeam.displayName }} </div>
              <div id="user-name"> {{ userName }}</div>
              <div v-for="(player, index) in selectPlayers" :key="index">
                <div class="position" :style="(player.position === 'DH') ? { top: player.top, left: '86px' }:{ top: player.top }">{{ player.position }}</div>
                <div class="player-name" :style="{ top: player.top }">{{ player.player.name }}</div>
                <div class="number" :style="{ top: player.top, left: player.player.left }">{{ player.player.id }}</div>
                <div class="bt" :style="{ top: player.top }">{{ player.player.bt }}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <button class="btn-primary btn-lg col-6" @click="generate">画像を生成</button>
      <hr>
      <p>↓ここに表示される画像を右クリック or 長押しで保存してください</p>
      <p id="result"></p>
      <!-- <button class="btn-primary btn-lg" @click="download">画像をダウンロード(PC用)</button> -->
      <hr>

      <h4 class="pt-2 pb-2">ここからツイートしてね</h4>
      <button class="btn-primary btn-lg col-6" @click="twitterShare">オーダーをツイートする</button>

    </div>
  </div>
</template>

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
                          {'jsonName': 'giants', 'displayName': '読売ジャイアンツ', 'hashTag': 'giants'},
                          {'jsonName': 'baystars', 'displayName': '横浜DeNAベイスターズ', 'hashTag': 'baystars'},
                          {'jsonName': 'tigers', 'displayName': '阪神タイガース', 'hashTag': 'tigers'},
                          {'jsonName': 'carp', 'displayName': '広島東洋カープ', 'hashTag': 'carp'},
                          {'jsonName': 'dragons', 'displayName': '中日ドラゴンズ', 'hashTag': 'dragons'},
                          {'jsonName': 'swallows', 'displayName': '東京ヤクルトスワローズ', 'hashTag': 'swallows'}
                       ],
            'pacific': [
                          {'jsonName': 'lions', 'displayName': '埼玉西武ライオンズ', 'hashTag': 'seibulions'}, 
                          {'jsonName': 'hawks', 'displayName': '福岡ソフトバンクホークス', 'hashTag': 'sbhawks'},
                          {'jsonName': 'eagles', 'displayName': '東北楽天ゴールデンイーグルス', 'hashTag': 'RakutenEagles'},
                          {'jsonName': 'marines', 'displayName': '千葉ロッテマリーンズ', 'hashTag': 'chibalotte'},
                          {'jsonName': 'fighters', 'displayName': '北海道日本ハムファイターズ', 'hashTag': 'lovefighters'},
                          {'jsonName': 'buffaloes', 'displayName': 'オリックス・バファローズ', 'hashTag': 'Bs2020'}
                       ]
           },
      selectTeam: '',
      npbPlayers: {'giants':giants, 'baystars': baystars, 'tigers': tigers, 'carp': carp, 'dragons': dragons, 'swallows': swallows,
                    'lions': lions, 'hawks': hawks, 'eagles':eagles, 'marines': marines, 'fighters': fighters, 'buffaloes': buffaloes},
      positions: ['投', '捕', '一', '二', '三', '遊', '左', '中', '右', 'DH'],
      selectPlayers: [
                        {'order': 1, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '135px'},
                        {'order': 2, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '175px'},
                        {'order': 3, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '214px'},
                        {'order': 4, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '254px'},
                        {'order': 5, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '294px'},
                        {'order': 6, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '334px'},
                        {'order': 7, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '373px'},
                        {'order': 8, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '413px'},
                        {'order': 9, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '453px'}
                     ]
    }
  },
  methods: {
    generate() {
      if (!!document.getElementById("resultImg")) {
            let resultImg = document.getElementById("resultImg");
            resultImg.remove();
      }
      html2canvas(document.getElementById('preview-inner')).then(
        function(canvas){
          let img = document.createElement("img");
          img.src = canvas.toDataURL('image/png');
          img.width = "500";
          img.height = "500";
          img.id = "resultImg";
          
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
    },
    twitterShare() {
      let shareURL = 'https://twitter.com/intent/tweet?text=' + "ぼくのかんがえた" + this.selectTeam.displayName + "のオーダー" + "%20%23野球 %20%23ぼくのオーダー %20%23" + this.selectTeam.hashTag + '&url=' + "https://battingordermaker.web.app/";
      location.href = shareURL
    }
  },
  watch: {
    selectPlayers: {
      handler: function() {
        let positions = [];
        let players = [];
        this.selectPlayers.filter(e => {
          if (e.position.length > 0) {
            if (positions.indexOf(e.position) === -1) {
              positions.push(e.position);
              e.duplicatedPosition = false;
            }else{
              e.duplicatedPosition = true;
            }
          }
          if (e.player !== '') {
            if (players.indexOf(e.player.name) === -1) {
              players.push(e.player.name);
              e.duplicatedPlayer = false;
            }else{
              e.duplicatedPlayer = true;
            }
          }
          return e;
        });
      },
      deep: true
    },
    selectLeague: {
      handler: function() {
        this.selectTeam = '';
      }
    },
    selectTeam: {
      handler: function() {
        this.selectPlayers = [
                        {'order': 1, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '135px'},
                        {'order': 2, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '175px'},
                        {'order': 3, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '214px'},
                        {'order': 4, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '254px'},
                        {'order': 5, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '294px'},
                        {'order': 6, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '334px'},
                        {'order': 7, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '373px'},
                        {'order': 8, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '413px'},
                        {'order': 9, 'position': '', 'player': '', 'duplicatedPosition': false, 'duplicatedPlayer': false, 'top': '453px'}
                     ];
      }
    }
  }
}

</script>

