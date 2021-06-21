<template>
  <div>
    <section v-show="gameStopped">
      <h2>Game result</h2>
      <p class="winLoseMsg">{{ winLoseMessage }}</p>
    </section>
    <section id="monster" class="container">
      <h2>Monster Health</h2>
      <h3>{{ monsterHealth }}</h3>
      <div class="healthbar">
        <div class="healthbar__value" v-bind:style="monsterBar"></div>
      </div>
    </section>
    <section id="player" class="container">
      <h2>Your Health</h2>
      <h3>{{ playerHealth }}</h3>
      <div class="healthbar">
        <div class="healthbar__value" v-bind:style="playerBar"></div>
      </div>
    </section>
    <section id="controls">
      <button v-on:click="attackMonster" v-bind:disabled="gameStopped">
        ATTACK
      </button>
      <button
        v-bind:disabled="round % 3 !== 0 || gameStopped"
        v-on:click="specialAttack"
      >
        SPECIAL ATTACK
      </button>
      <button v-on:click="healPlayer" v-bind:disabled="gameStopped">
        HEAL
      </button>
      <button v-on:click="restartGame">RESTART GAME</button>
    </section>
    <section id="log" class="container">
      <h2>Battle Log</h2>
      <ul>
        <li v-for="logMessage in logMessagesList" v-bind:key="logMessage">
          <span
            v-bind:class="{
              'log--player': logMessage.whoActed === 'player',
              'log--monster': logMessage.whoActed === 'monster',
            }"
            >{{ logMessage.whoActed }}</span
          >
          – {{ logMessage.whichAction }}:
          <span
            v-bind:class="{
              'log--damage': logMessage.whichAction === 'attack',
              'log--heal': logMessage.whichAction === 'heal',
            }"
            >{{ logMessage.valueOfAction }}</span
          >
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
function getRandomValue(min, max) {
  return Math.floor(Math.random() * (max - min) + min);
}

export default {
  name: "Game",
  data() {
    return {
      playerHealth: 100,
      monsterHealth: 100,
      round: 0,
      winLoseMessage: "",
      gameStopped: false,
      logMessagesList: [],
    };
  },
  computed: {
    monsterBar() {
      return { width: this.monsterHealth + "%" };
    },
    playerBar() {
      return { width: this.playerHealth + "%" };
    },
  },
  methods: {
    attackMonster() {
      this.round++;
      const attackValue = getRandomValue(5, 12);
      if (this.monsterHealth - attackValue <= 0) {
        this.monsterHealth = 0;
        this.addActionMessage("player", "attack", attackValue);
        this.winLoseMessage = "You won! You beat the monster!";
        this.gameStopped = true;
      } else {
        this.monsterHealth -= attackValue;
        this.addActionMessage("player", "attack", attackValue);
        this.attackPlayer();
      }
    },
    attackPlayer() {
      const attackValue = getRandomValue(8, 15);
      if (this.playerHealth - attackValue <= 0) {
        this.playerHealth = 0;
        this.addActionMessage("monster", "attack", attackValue);
        this.winLoseMessage = "The Monster beat you, you lost.";
        this.gameStopped = true;
      } else {
        this.playerHealth -= attackValue;
        this.addActionMessage("monster", "attack", attackValue);
      }
    },
    specialAttack() {
      this.round++;
      const attackValue = getRandomValue(10, 22);
      if (this.monsterHealth - attackValue <= 0) {
        this.monsterHealth = 0;
        this.addActionMessage("player", "attack", attackValue);
        this.winLoseMessage = "You won! You beat the monster!";
        this.gameStopped = true;
      } else {
        this.monsterHealth -= attackValue;
        this.addActionMessage("player", "attack", attackValue);
        this.attackPlayer();
      }
    },
    healPlayer() {
      this.round++;
      const healValue = getRandomValue(8, 12);
      if (this.playerHealth + healValue > 100) {
        this.playerHealth = 100;
      } else {
        this.playerHealth += healValue;
      }
      this.addActionMessage("player", "heal", healValue);
      this.attackPlayer();
    },
    restartGame() {
      this.playerHealth = 100;
      this.monsterHealth = 100;
      this.round = 0;
      this.gameStopped = false;
      this.winLoseMessage = "";
      this.logMessagesList = [];
    },
    addActionMessage(who, what, value) {
      this.logMessagesList.unshift({
        whoActed: who,
        whichAction: what,
        valueOfAction: value,
      });
    },
  },
};
</script>

<style scoped>
* {
  box-sizing: border-box;
}

html {
  font-family: "Jost", sans-serif;
}

body {
  margin: 0;
}

header {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 0.5rem;
  background-color: #880017;
  color: white;
  text-align: center;
  margin-bottom: 2rem;
}

section {
  width: 90%;
  max-width: 40rem;
  margin: auto;
}

.winLoseMsg {
  border: 3px solid rgb(7, 138, 245);
  border-radius: 30%;
  width: max-content;
  padding: 1rem;
  box-sizing: border-box;
  margin: 0 auto;
}

.healthbar {
  width: 100%;
  height: 40px;
  border: 1px solid #575757;
  margin: 1rem 0;
  background: #fde5e5;
}

.healthbar__value {
  background-color: #00a876;
  width: 100%;
  height: 100%;
}

.container {
  text-align: center;
  padding: 0.5rem;
  margin: 1rem auto;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  border-radius: 12px;
}

#monster h2,
#player h2 {
  margin: 0.25rem;
}

#controls {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
}

button {
  font: inherit;
  border: 1px solid #88005b;
  background-color: #88005b;
  color: white;
  padding: 1rem 2rem;
  border-radius: 12px;
  margin: 1rem;
  width: 12rem;
  cursor: pointer;
  box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.26);
}

button:focus {
  outline: none;
}

button:hover,
button:active {
  background-color: #af0a78;
  border-color: #af0a78;
  box-shadow: 1px 1px 8px rgba(0, 0, 0, 0.26);
}

button:disabled {
  background-color: #ccc;
  border-color: #ccc;
  box-shadow: none;
  color: #3f3f3f;
  cursor: not-allowed;
}

#log ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

#log li {
  margin: 0.5rem 0;
}

.log--player {
  color: #7700ff;
  text-transform: capitalize;
}

.log--monster {
  color: #da8d00;
  text-transform: capitalize;
}

.log--damage {
  color: red;
}

.log--heal {
  color: green;
}
</style>
