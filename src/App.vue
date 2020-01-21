<template>
  <div id="app">
    <div class="btnBar">
      <button @click="adding" title="Add a new line">Add</button>
      <button @click="remove" :disabled="disabling" title="Remove last line">Remove</button>
      <button @click="reset" :disabled="disabling" title="Remove all lines">Remove all</button>
    </div>

    <Dice
      v-for="( dice, id ) in nbDice"
      :key="id"
      :diceid="id"
      :dicedata="dice"
      @roll="rolling"
      @deleteme="deletedice"
    />

    <button @click="allin" v-if="nbDice.length > 2" title="Rollin all dices">All in</button>
  </div>
</template>

<script>
import Dice from './components/Dice.vue';

const dataDefault = {
  number: 1,
  select: 20,
  modificator: 0,
  result: undefined,
  explain: '',
};

export default {
  name: 'App',

  components: {
    Dice,
  },

  data() {
    return {
      nbDice: [],
    };
  },

  created() {
    this.adding();
  },

  methods: {
    adding() {
      const dices = this.nbDice.length;
      const dataNew = dices > 0 ? this.nbDice[dices - 1] : { ...dataDefault };

      this.nbDice.push(dataNew);
    },
    remove() {
      if (this.nbDice.length) {
        this.nbDice.pop();
      }
    },
    reset() {
      this.nbDice = [];
    },
    allin() {
      this.$emit('allin');
    },
    rolling(id, value) {
      // console.log('rolling - ID:', id, 'RESULT:', value);
      this.nbDice[id].result = value;
    },
    deletedice(index) {
      this.nbDice.splice(index, 1);
    },
  },

  computed: {
    disabling() {
      return this.nbDice.length <= 0;
    },
  },
};
</script>

<style>
.btnBar {
  margin: 0 0 5px;
}
button {
  height: 30px;
  cursor: pointer;
  border: 0;
  background: #eee;
  text-transform: uppercase;
  font-weight: bold;
  color: #808080;
}
button:focus,
button:hover {
  background: #808080;
  color: #fff;
}
input,
select {
  border: 1px solid #eee;
  background-color: transparent;
  color: #474747;
  padding: 0;
  margin: 0;
  height: 30px;
  box-sizing: border-box;
}
input:focus,
input:hover,
select:focus,
select:hover {
  border-color: #808080;
}
button:disabled {
  cursor: not-allowed;
  opacity: 0.4;
}
</style>
