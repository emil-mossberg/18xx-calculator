<template>
  <div class="gameSettings">
    <h2 class="gameSectionHeader">Settings</h2>
    <div class="gameSettings__body">
      <label class="gameSettings__label" for="select-game">Game</label>
      <select
        class="gameSettings__select"
        name="select-game"
        v-model="selectedGame"
      >
        <option v-for="(game, index) in games" v-bind:key="index">
          {{ game }}
        </option>
      </select>
      <label class="gameSettings__label">Player Number</label>
      <select class="gameSettings__select" v-model="selectedPlayerCount">
        <option v-for="(playerCount, index) in playerCounts" v-bind:key="index">
          {{ playerCount }}
        </option>
      </select>
      <label class="gameSettings__label">Simulation Type</label>
      <div class="gameSettings__simulationOptions">
        <input
          class="gameSettings__simulationOption"
          type="radio"
          :value="false"
          v-model="setAdvancedSimulation"
          id="eq-revenue"
        />
        <label class="gameSettings__simulationLabel" for="eq-revenue"
          >Eq revenue</label
        >
        <input
          class="gameSettings__simulationOption"
          type="radio"
          :value="true"
          v-model="setAdvancedSimulation"
          id="different-revenue"
        /><label class="gameSettings__simulationLabel" for="different-revenue"
          >Diff revenue</label
        >
      </div>
      <label for="sim-input" class="gameSettings__label"
        >Simulated Rounds</label
      >
      <div class="gameSettings__simulateRounds">
        <button
          class="gameSettings__simButton gameSettings__simButton--left"
          :disabled="simulatedRounds < 1"
          v-on:click="changeSimulatedRounds(simulatedRounds - 1)"
        >
          -1
        </button>
        <input
          class="gameSettings__simInput"
          type="number"
          id="sim-input"
          :value="simulatedRounds"
          @change="changeSimulatedRounds($event.target.value)"
        />
        <button
          class="gameSettings__simButton gameSettings__simButton--right"
          v-on:click="changeSimulatedRounds(simulatedRounds + 1)"
        >
          +1
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapState } from "vuex";
import googleAnalyticsMixin from "../mixins/googleAnalyticsMixin";

export default {
  name: "GameSettings",

  mixins: [googleAnalyticsMixin],

  methods: {
    changeSimulatedRounds(value) {
      this.$store.dispatch("setSimulatedRounds", parseInt(value));
      this.gaEventSimulation(value);
    },
  },
  computed: {
    ...mapGetters(["playerCounts", "selectedPlayerCount"]),
    ...mapState(["games", "simulatedRounds"]),
    selectedGame: {
      get() {
        return this.$store.state.selectedGame;
      },
      set(value) {
        this.$store.dispatch("setupNewGame", value);
        this.gaEventChangeGame(value);
      },
    },

    selectedPlayerCount: {
      get() {
        return this.$store.state.selectedPlayerCount;
      },
      set(value) {
        this.$store.dispatch("changePlayerCount", value);
        this.gaEventChangePlayerCount(value);
      },
    },

    setAdvancedSimulation: {
      get() {
        return this.$store.state.advancedSimulation;
      },
      set(value) {
        this.$store.dispatch("toggleAdvancedSimulation", value);
      },
    },
  },
};
</script>

<style lang="less" src="../assets/less/_GameSettings.less" scoped></style>
