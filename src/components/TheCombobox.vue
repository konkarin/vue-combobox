<script setup lang="ts">
import { computed, ref } from 'vue'
const inputValue = ref('')
const options = ref(['hoge', 'fuga', 'piyo'])
const filteredOptions = computed(() => {
  if (!inputValue.value) return options.value
  return options.value.filter((option) => {
    return option.includes(inputValue.value)
  })
})
const expandedListbox = ref(false)
const toggleListbox = (value: boolean) => {
  expandedListbox.value = value
}
</script>

<template>
  <div class="combobox">
    <label for="combobox">Combobox sample</label>
    <input
      id="combobox"
      type="text"
      v-model="inputValue"
      class="combobox-input"
      @focus="toggleListbox(true)"
      @blur="toggleListbox(false)"
    />
    <!-- popup -->
    <div v-if="expandedListbox" class="combobox-listbox">
      <button v-for="option in filteredOptions" :key="option" class="combobox-option">
        {{ option }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.combobox {
  position: relative;
  display: grid;
  width: 300px;
}
.combobox-input {
  padding: 8px 12px;
}
.combobox-listbox {
  display: grid;
  position: absolute;
  top: 100%;
  width: 100%;
  border: 1px solid #eee;
  padding: 0;
}
.combobox-option {
  text-align: left;
  padding: 8px 12px;
}
button {
  background: none;
  border: none;
  margin: 0;
  padding: 0;
}
</style>
