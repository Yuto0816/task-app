<!--
https://eugenkiss.github.io/7guis/tasks/#crud
-->

<script setup lang="ts">
import { ref, reactive, computed, watch } from "vue";

const names = reactive<string[]>([
  "Emil, Hans",
  "Mustermann, Max",
  "Tisch, Roman",
]);
const selected = ref<string>("");
const prefix = ref<string>("");
const first = ref<string>("");
const last = ref<string>("");

const filteredNames = computed(() =>
  names.filter((name) =>
    name.toLowerCase().startsWith(prefix.value.toLowerCase())
  )
);

watch(selected, (name) => {
  const [lastName, firstName] = name.split(", ");
  last.value = lastName;
  first.value = firstName;
});

function create() {
  if (hasValidInput()) {
    const fullName = `${last.value}, ${first.value}`;
    if (!names.includes(fullName)) {
      names.push(fullName);
      first.value = last.value = "";
    }
  }
}

function update() {
  if (hasValidInput() && selected.value) {
    const i = names.indexOf(selected.value);
    names[i] = selected.value = `${last.value}, ${first.value}`;
  }
}

function del() {
  if (selected.value) {
    const i = names.indexOf(selected.value);
    names.splice(i, 1);
    selected.value = first.value = last.value = "";
  }
}

function hasValidInput(): boolean {
  return (first.value.trim() !== "") && (last.value.trim() !== "");
}
</script>

<template>
  <v-text-field
    label="Filter Prefix"
    v-model="prefix"
  ></v-text-field>

  <select size="5" v-model="selected">
    <option v-for="name in filteredNames" :key="name">{{ name }}</option>
  </select>

  <v-form>
    <v-text-field label="名前" v-model="first"></v-text-field>
    <v-text-field label="姓" v-model="last"></v-text-field>
  </v-form>

  <div class="buttons">
    <v-btn @click="create">Create</v-btn>
    <v-btn @click="update">Update</v-btn>
    <v-btn @click="del">Delete</v-btn>
  </div>
</template>
