<script setup>
import { ref } from "vue";
import { _ } from "lodash";
import { nanoid } from "nanoid";

const cols = ref([]);
const selectedValues = ref([]);
const turns = ref(0);
const matches = ref(0);

const generate = () => {
  debugger;

  const alpha = Array.from(Array(9)).map((e, i) => i + 65);
  const alphabet = alpha.map((x) => String.fromCharCode(x));

  alphabet.forEach((item) => {
    cols.value.push({ id: nanoid(), value: item, show: false });
    cols.value.push({ id: nanoid(), value: item, show: false });
  });

  cols.value = _.orderBy(cols.value, ["id"]);


};
generate();
const reset = () => {
  generate();
};
const selected = (id) => {
  const index = cols.value.findIndex((c) => c.id === id);
  if (selectedValues.value.length < 2) {
    const item = cols.value[index];
    item.show = true;
    selectedValues.value.push(item);
  }
  if (selectedValues.value.length === 2) {
    setTimeout(() => {
      if (selectedValues.value[0].value == selectedValues.value[1].value) {
        matches.value = matches.value + 1;
      } else {
        cols.value.filter((col) => {
          if (selectedValues.value.map((c) => c.id).includes(col.id))
            col.show = false;
        });
      }
      turns.value = turns.value + 1;
      selectedValues.value = [];
    }, 500);
  }
};
</script>

<template>
  <ul class="wrapper">
    <li v-for="(col,index) in cols">
      <button v-if="!col.show" @click="selected(col.id)" class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded">{{ index+1 }}</button>
      <button v-else class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">{{ col.value }}</button>
    </li>
  </ul>

  matches:<span>{{ matches }}</span>
  <br />
  turns:<span>{{ turns }}</span>
  <br />
  <button @click="reset">reset</button>
</template>


<style scoped>
.wrapper{
 display: flex;
 flex-wrap: wrap;
}
.wrapper li {
flex-basis: 33%;
width: 33%;
display: flex;
flex-flow: column;
}
</style>
