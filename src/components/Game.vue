<template>
  <div>
    <section id="monster" class="container">
      <h2>Monster Health</h2>
      <div class="healthbar">
        <div class="healthbar__value" v-bind:style="monsterBar"></div>
      </div>
    </section>
    <section id="player" class="container">
      <h2>Your Health</h2>
      <div class="healthbar">
        <div class="healthbar__value" v-bind:style="playerBar"></div>
      </div>
    </section>
    <section id="controls">
      <button v-on:click="attackMonster">ATTACK</button>
      <button v-bind:disabled="round % 3 !== 0" v-on:click="specialAttack">
        SPECIAL ATTACK
      </button>
      <button v-on:click="healPlayer">HEAL</button>
      <button>SURRENDER</button>
      <button v-on:click="restartGame">RESTART GAME</button>
    </section>
    <section id="log" class="container">
      <h2>Battle Log</h2>
      <ul></ul>
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
        alert("You won! You beat the monster! The game will be restarted");
        this.restartGame();
      } else {
        this.monsterHealth -= attackValue;
        this.attackPlayer();
      }
    },
    attackPlayer() {
      const attackValue = getRandomValue(8, 15);
      if (this.playerHealth - attackValue <= 0) {
        alert("The Monster beat you, you lost. The game will be restarted");
        this.restartGame();
      } else {
        this.playerHealth -= attackValue;
      }
    },
    specialAttack() {
      this.round++;
      const attackValue = getRandomValue(10, 22);
      this.monsterHealth -= attackValue;
      this.attackPlayer();
    },
    healPlayer() {
      this.round++;
      const healValue = getRandomValue(8, 12);
      if (this.playerHealth + healValue > 100) {
        this.playerHealth = 100;
      } else {
        this.playerHealth += healValue;
      }
      this.attackPlayer();
    },
    restartGame() {
      this.playerHealth = 100;
      this.monsterHealth = 100;
      this.round = 0;
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
}

.log--monster {
  color: #da8d00;
}

.log--damage {
  color: red;
}

.log--heal {
  color: green;
}
</style>
