<template>
  <div class="dice">
    <input
      v-model.number="number"
      type="number"
      class="dice__nbr"
      min="1"
      max="100"
      title="Number of dice"
    >

    <select v-model.number="select" class="select" title="Type of dice">
      <option value="2">d2</option>
      <option value="4">d4</option>
      <option value="6">d6</option>
      <option value="8">d8</option>
      <option value="10">d10</option>
      <option value="12">d12</option>
      <option value="20">d20</option>
      <option value="100">d100</option>
    </select>

    <input v-model.number="modificator" type="number" title="Modificator (+/-)">

    <button @click="roll" :disabled="number < 1" class="btnRollin">Roll</button>

    <span v-if="result!==undefined" class="result">{{ result }}</span>

    <span v-if="explain" class="explain">{{ explain }}</span>

    <button
      @click="delline"
      v-if="result!==undefined"
      class="deleteRoll"
      title="Delete this line"
    >Delete</button>
  </div>
</template>

<script>
export default {
  name: 'Dice',
  data() {
    return {
      result: undefined,
      number: 1,
      select: 20,
      modificator: 0,
      explain: '',
    };
  },
  props: {
    diceid: Number,
  },
  methods: {
    roll() {
      const seed = Math.floor(Math.random() * parseInt(this.select, 10)) + 1;

      this.result = parseInt(this.number, 10) * seed + parseInt(this.modificator, 10);

      if (this.modificator !== 0) {
        this.explain = `(without mod = ${parseInt(this.number, 10) * seed})`;
      }
    },
    delline() {
      this.$emit('deletethis', this); // this = Dice compoenent
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
