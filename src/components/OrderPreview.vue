<template>
  <margin-div>
    <heading2>プレビューだよ</heading2>
    <div class="row justify-content-center">
      <div>
        <div id="preview" class="pb-4">
          <div id="preview-inner">
            <div id="team-name">{{ selectTeam }}</div>
            <div id="user-name">{{ !!userName ? userName : 'ななしさん' }}</div>
            <div id="today">{{ today }}</div>
            <div v-for="(player, index) in selectPlayers" :key="index">
              <div
                class="position"
                :style="(player.position === 'DH') ? { top: player.top, left: '86px' }:{ top: player.top }"
              >{{ player.position }}</div>
              <div class="player-name" :style="{ top: player.top }">{{ player.player.name }}</div>
              <div
                class="number"
                :style="{ top: player.top, left: player.player.left }"
              >{{ player.player.id }}</div>
              <div class="bt" :style="{ top: player.top }">{{ player.player.bt }}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </margin-div>
</template>

<script>
import Heading2 from "./Heading2";
import MarginDiv from "./MarginDiv";

export default {
    name: "OrderPreview",
    props: {
      selectTeam: {
          type: String
      },
      userName: {
          type: String
      },
      today: {
          type: String
      },
      selectPlayers: {
          type: Array
      }
    },
    components: {
      "heading2": Heading2,
      "margin-div": MarginDiv        
    }
};

</script>