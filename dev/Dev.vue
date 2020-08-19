<template>
  <div id="app">
    <v-select v-model="selected" v-bind="config" />
asdad
    <v-select-renderless 
      v-model="selected"
      :options="config.options"
      #default="{ state, api, options, refs }"
    >
      <div class="select-wrapper-that-wont-exist-in-vue3">
        <div v-bind="refs.combobox.attributes">
          <span 
            v-for="(option, i) in state.selectedValue" 
            :key="i"
          >
            {{ option }}
          </span>
          <div style="position: relative">
            <!-- use getOptionLabel or provide an already pretiffied selected value for single select-->
            <input 
              :style="{
                'position': 'absolute', 
                'z-index': -1, 
                'pointer-events': 'none',
                'opacity': state.searching ? 0.4 : 1
              }"
              :value="state.selectedValue && state.selectedValue[0] && state.selectedValue[0].label" 
              readonly 
            />
            <input style="background: transparent" v-bind="refs.search.attributes" v-on="refs.search.events" />
          </div>
          <button @click="api.toggleDropdown">
            open
          </button>
        </div>
        <ul 
          v-show="state.open"
          v-bind="refs.listbox.attributes" 
          v-on="refs.listbox.events"
        >
          <li
            v-for="(option, i) in options"
            :key="refs.optionGetter(option, i).attributes.key"
            v-bind="refs.optionGetter(option, i).attributes"
            v-on="refs.optionGetter(option, i).events"
            :class="{
              'is-highlighted': state.highlightedIndex === i,
              'is-selected': api.isOptionSelected(option)
            }"
          >
            {{ option.label }}
          </li>
          <li v-if="!options.length">No results found.</li>
        </ul>
      </div>
    </v-select-renderless>
  </div>
</template>

<script>
import vSelect from "../src/components/Select";
import VSelectRenderless from "../src/components/SelectRenderless.vue";
import countries from "../docs/.vuepress/data/countryCodes";
import books from "../docs/.vuepress/data/books";

export default {
  components: { vSelect, VSelectRenderless },
  data: () => ({
    selected: null,
    open: false,
    config: {
      options: countries
    }
  })
};
</script>

<style>
html,
body {
  margin: 0;
  height: 100%;
  font-family: -apple-system, sans-serif;
}

#app {
  height: 100%;
  max-width: 20rem;
  margin: 10rem auto 0;
}

hr {
  border: none;
  border-bottom: 1px solid #cacaca;
  margin-bottom: 1em;
  padding-top: 1em;
  width: 90%;
}

.is-highlighted {
  background-color: lime;
}

.is-selected {
  color: lime;
}


.is-highlighted.is-selected {
  color: white;
}

</style>
