<template>
  <div class="container">
    <div>
      <div id="app">
        <title-logo></title-logo>
        <hr>
        <input-name v-model="userName"></input-name>
        <select-league v-model="selectLeague" :name="league-radio" :options="leagueName" ref="selectLeague"></select-league>
        <select-team v-model="selectTeam" :name="team-select" :options="npbTeam[selectLeague]" ref="selectTeam"></select-team>
        <margin-div>
          <heading2>打順を組んでね</heading2>
          <p>　※守備、選手が重複するとその箇所が赤くなります。　</p>
          <div>
            <div v-for="(selectPlayer, index) in selectPlayers" :key="index" class="mt-2 row align-items-center">
              <div class="col-1">
                {{ index + 1 }}：
              </div>
              <select
                class="form-control col-3"
                v-model="selectPlayer.position"
                :class="{ duplicated: selectPlayer.duplicatedPosition }"
              >
                <option disabled value>守備</option>
                <option v-for="(position, index) in positions" :key="index">{{ position }}</option>
              </select>

              <select
                class="form-control col"
                v-model="selectPlayer.player"
                :class="{ duplicated: selectPlayer.duplicatedPlayer }"
              >
                <option disabled value>選手</option>
                <option
                  v-for="(player, index) in players[selectTeam]"
                  :key="index"
                  :value="player"
                >{{ player.name }}</option>
              </select>
            </div>
          </div>
        </margin-div>
        <order-preview :selectTeam="selectTeam" :userName="userName" :today="today" :selectPlayers="selectPlayers"></order-preview>
        <generate-button @click="generate"></generate-button>
        <p class="mtb-2"></p>
        <clear-button @click="resetData"></clear-button>
        <hr>
        <div class="text-center">
          <p>↓ここに表示される画像を右クリック or 長押しで保存してください</p>
          <p id="result"></p>
        </div>
        <!-- <button class="btn-primary btn-lg" @click="download">画像をダウンロード(PC用)</button> -->
        <hr>
        <twitter-button @click="twitterShare"></twitter-button>

      </div>
    </div>
  </div>
</template>

<script>
import giants from "./assets/giants.json";
import baystars from "./assets/baystars.json";
import tigers from "./assets/tigers.json";
import carp from "./assets/carp.json";
import dragons from "./assets/dragons.json";
import swallows from "./assets/swallows.json";
import lions from "./assets/lions.json";
import hawks from "./assets/hawks.json";
import eagles from "./assets/eagles.json";
import marines from "./assets/marines.json";
import fighters from "./assets/fighters.json";
import buffaloes from "./assets/buffaloes.json";
import central from "./assets/central.json";
import pacific from "./assets/pacific.json";
import hash from "./assets/hash.json";
import html2canvas from "html2canvas";
import Title from "./components/Title";
import InputName from "./components/InputName";
import SelectLeague from "./components/SelectLeague";
import SelectTeam from "./components/SelectTeam";
import OrderPreview from "./components/OrderPreview";
import GenerateButton from "./components/GenerateButton";
import ClearButton from "./components/ClearButton";
import TwitterButton from "./components/TwitterButton";
import Heading2 from "./components/Heading2";
import MarginDiv from "./components/MarginDiv";

export default {
  name: "app",
  data() {
    return {
      userName: "",
      leagueName: [
        { label: "セ・リーグ", value: "central", checked: true },
        { label: "パ・リーグ", value: "pacific", checked: false }
      ],
      selectLeague: "central",
      npbTeam: { central: central, pacific: pacific},
      selectTeam: "",
      players: {
        "読売ジャイアンツ": giants,
        "横浜DeNAベイスターズ": baystars,
        "阪神タイガース": tigers,
        "広島東洋カープ": carp,
        "中日ドラゴンズ": dragons,
        "東京ヤクルトスワローズ": swallows,
        "埼玉西武ライオンズ": lions,
        "福岡ソフトバンクホークス": hawks,
        "東北楽天ゴールデンイーグルス": eagles,
        "千葉ロッテマリーンズ": marines,
        "北海道日本ハムファイターズ": fighters,
        "オリックス・バファローズ": buffaloes
      },
      hashTags: hash,
      positions: ["投", "捕", "一", "二", "三", "遊", "左", "中", "右", "DH"],
      selectPlayers: [
        { order: 1, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "135px" },
        { order: 2, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "175px" },
        { order: 3, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "214px" },
        { order: 4, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "254px" },
        { order: 5, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "294px" },
        { order: 6, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "334px" },
        { order: 7, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "373px" },
        { order: 8, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "413px" },
        { order: 9, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "453px" }
      ],
      today: ""
    };
  },
  methods: {
    generate() {
      if (!!document.getElementById("resultImg")) {
        let resultImg = document.getElementById("resultImg");
        resultImg.remove();
      }
      html2canvas(document.getElementById("preview-inner")).then(function(
        canvas
      ) {
        let img = document.createElement("img");
        img.src = canvas.toDataURL("image/png");
        img.width = "500";
        img.height = "500";
        img.id = "resultImg";
        img.classList.add("img-fluid");

        let parentElement = document.getElementById("result").parentNode;
        let p = document.getElementById("result");
        parentElement.insertBefore(img, p);
      });
    },
    download() {
      html2canvas(document.getElementById("preview-inner")).then(function(
        canvas
      ) {
        let link = document.createElement("a");
        link.href = canvas.toDataURL("image/png");
        link.download = "myorder.png";
        link.click();
      });
    },
    twitterShare() {
      let shareURL =
        "https://twitter.com/intent/tweet?text=" +
        "ぼくのかんがえた" +
        this.selectTeam +
        "のオーダー" +
        "%20%23野球 %20%23ぼく将オーダー %20%23" +
        this.hashTags[this.selectTeam] +
        "&url=" +
        "https://battingordermaker.web.app/";
      let link = document.createElement("a");
      link.href = shareURL;
      link.target = "_brank";
      link.click();
    },
    resetData() {
      Object.assign(this.$data, this.$options.data());
      this.todaysDate();
      this.$refs.selectLeague.checkCentral();
      this.$refs.selectTeam.selectInit();
    },
    todaysDate() {
      let now = new Date();
      let year = now.getFullYear();
      let month = now.getMonth() + 1;
      let day = now.getDate();

      this.today = year + "/" + month + "/" + day;
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
            } else {
              e.duplicatedPosition = true;
            }
          }
          if (e.player !== "") {
            if (players.indexOf(e.player.name) === -1) {
              players.push(e.player.name);
              e.duplicatedPlayer = false;
            } else {
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
        this.selectTeam = "";
      }
    },
    selectTeam: {
      handler: function() {
        this.selectPlayers = [
          { order: 1, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "135px" },
          { order: 2, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "175px" },
          { order: 3, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "214px" },
          { order: 4, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "254px" },
          { order: 5, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "294px" },
          { order: 6, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "334px" },
          { order: 7, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "373px" },
          { order: 8, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "413px" },
          { order: 9, position: "", player: "", duplicatedPosition: false, duplicatedPlayer: false, top: "453px" }
        ];
      }
    }
  },
  created: function() {
    this.todaysDate();
  },
  components: {
    "title-logo": Title,
    "input-name": InputName,
    "select-league": SelectLeague,
    "select-team": SelectTeam,
    "order-preview": OrderPreview,
    "generate-button": GenerateButton,
    "clear-button": ClearButton,
    "twitter-button": TwitterButton,
    "heading2": Heading2,
    "margin-div": MarginDiv
  }
};
</script>

