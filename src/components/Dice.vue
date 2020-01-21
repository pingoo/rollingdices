<template>
  <div class="dice">
    <input
      v-model.number="dicedata.number"
      type="number"
      class="dice__nbr"
      min="1"
      max="100"
      title="Number of dice"
    >

    <select v-model.number="dicedata.select" class="select" title="Type of dice">
      <option value="2">d2</option>
      <option value="4">d4</option>
      <option value="6">d6</option>
      <option value="8">d8</option>
      <option value="10">d10</option>
      <option value="12">d12</option>
      <option value="20">d20</option>
      <option value="100">d100</option>
    </select>

    <input v-model.number="dicedata.modificator" type="number" title="Modificator (+/-)">

    <button @click="roll" :disabled="dicedata.number < 1" class="btnRollin">Roll</button>

    <span v-if="dicedata.result!==undefined" class="result">{{ dicedata.result }}</span>

    <span v-if="dicedata.explain" class="explain">{{ dicedata.explain }}</span>

    <button
      @click="deleteme"
      v-if="dicedata.result!==undefined"
      class="deleteRoll"
      title="Delete this line"
    >Delete</button>
  </div>
</template>

<script>
export default {
  name: 'Dice',

  props: {
    diceid: {
      type: Number,
      required: true,
      default: 0,
    },
    dicedata: {
      type: Object,
      required: true,
    },
  },

  methods: {
    roll() {
      const seed = Math.floor(Math.random() * parseInt(this.dicedata.select, 10)) + 1;
      const result = parseInt(this.dicedata.number, 10) * seed
      + parseInt(this.dicedata.modificator, 10);

      if (this.dicedata.modificator !== 0) {
        this.dicedata.explain = `
          (without mod = ${parseInt(this.dicedata.number, 10) * seed})
        `;
      }

      this.$emit('roll', this.diceid, result);
    },
    deleteme() {
      this.$emit('deleteme', this.diceid);
    },
  },

  created() {
    this.$parent.$on('allin', this.roll);
  },
};
</script>

<style>
.dice {
  font-size: 20px;
  margin: 0 0 5px;
  height: 30px;
  border-top: 1px solid transparent;
  border-bottom: 1px solid transparent;
}
.dice:last-child {
  margin: 0;
}
.dice:hover {
  border-top: 1px solid #808080;
  border-bottom: 1px solid #808080;
}
.btnRollin {
  margin: 0 0 0 5px;
}
input {
  width: 50px;
  text-align: center;
}
.result {
  font-weight: 700;
  padding: 0 10px;
  font-family: "Arial", Helvetica, sans-serif;
  border: 2px solid rgba(128, 128, 128, 0.7);
  height: 30px;
  box-sizing: border-box;
  display: inline-block;
  line-height: 28px;
  vertical-align: bottom;
  min-width: 50px;
  text-align: center;
  margin: 0 0 0 5px;
  color: #474747;
}
.explain {
  font-size: 12px;
  font-style: italic;
  font-family: "Arial", Helvetica, sans-serif;
  margin: 0 0 0 5px;
  color: #474747;
}
.deleteRoll {
  margin: 0 0 0 5px;
  display: none;
}
.dice:hover .deleteRoll {
  display: inline-block;
}
</style>
