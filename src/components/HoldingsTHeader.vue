<template>
  <thead>
    <tr>
      <th class="mainHeader" :colspan="selectedPlayerCount + 1">
        Players Holdings Table
      </th>
    </tr>
    <tr>
      <th>Corporation</th>
      <PlayerTHeader
        v-for="(player, index) in players"
        v-bind:key="index"
        v-bind:index="index"
        v-bind:name="player"
      />
    </tr>
  </thead>
</template>

<script>
import { mapState } from "vuex";

import PlayerTHeader from "@/components/PlayerTHeader";

export default {
  name: "HoldingsTHeader",
  components: {
    PlayerTHeader,
  },
  computed: {
    ...mapState(["selectedPlayerCount", "players"]),
  },
};
</script>
