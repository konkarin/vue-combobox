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

const currentSelectedIndex = ref(-1)
const onKeydownUp = () => {
  if (currentSelectedIndex.value === -1) {
    toggleListbox(true)
  }

  if (currentSelectedIndex.value <= 0) {
    currentSelectedIndex.value = filteredOptions.value.length - 1
  } else {
    currentSelectedIndex.value += -1
  }
}
const onKeydownDown = () => {
  if (currentSelectedIndex.value === -1) {
    toggleListbox(true)
  }

  if (currentSelectedIndex.value >= filteredOptions.value.length - 1) {
    currentSelectedIndex.value = 0
  } else {
    currentSelectedIndex.value += 1
  }
}

const onKeydownEnter = () => {
  if (filteredOptions.value[currentSelectedIndex.value] === undefined) {
    return
  }

  inputValue.value = filteredOptions.value[currentSelectedIndex.value]
  toggleListbox(false)
  resetCurrentIndex()
}

const onKeydownEsc = () => {
  toggleListbox(false)
  resetCurrentIndex()
}

const isActive = (index: number) => {
  return currentSelectedIndex.value === index
}

const resetCurrentIndex = () => {
  currentSelectedIndex.value = -1
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
      @input="resetCurrentIndex()"
      @keydown.prevent.up="onKeydownUp()"
      @keydown.prevent.down="onKeydownDown()"
      @keydown.prevent.enter="onKeydownEnter()"
      @keydown.prevent.esc="onKeydownEsc()"
    />
    <!-- popup -->
    <div v-if="expandedListbox" class="combobox-listbox">
      <button
        v-for="(option, index) in filteredOptions"
        :key="option"
        class="combobox-option"
        :class="{ 'combobox-option-active': isActive(index) }"
        @mouseover="currentSelectedIndex = index"
        @mouseleave="resetCurrentIndex()"
      >
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

.combobox-option-active {
  background-color: #009ae0;
}

button {
  background: none;
  border: none;
  margin: 0;
  padding: 0;
}
</style>
