<template>
  <div id="app">
    <div id="Tab" class="Tab">
      <div v-bind:class="[RABClass]" @click="RAButton"><h4>Resolution Calculator</h4></div>
      <div v-bind:class="[CPBClass]" @click="CPButton"><h4>Color Picker</h4></div>
    </div>
    <ResolutionApp v-bind:class="[RAClass]" />
    <ColorPicker v-bind:class="[CPClass]"/>
  </div>
</template>

<script>
import ResolutionApp from './components/ResolutionApp.vue'
import ColorPicker from './components/ColorPicker.vue'
export default {
  name: 'app',
  components: {
    ResolutionApp,
    ColorPicker
  },
  data: function() {
    return {
      RAClass: 'SelectedApp',
      CPClass: 'HiddebApp',
      RABClass: 'SelectedTab',
      CPBClass: 'TabHover'
    }
  },
  methods: {
    RAButton: function(){
      this.RAClass = 'SelectedApp';
      this.CPClass = 'HiddebApp';
      this.RABClass = 'SelectedTab';
      this.CPBClass = 'TabHover';
    },
    CPButton: function(){
      this.CPClass = 'SelectedApp';
      this.RAClass = 'HiddebApp';
      this.CPBClass = 'SelectedTab';
      this.RABClass = 'TabHover';
    },
  }
}

</script>

<style>

html {
  min-height: 100%;
}

body  {
  background-color: #23272f;
  color: #cccccc;
  margin: 0px;
  min-height: 100%;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #cccccc;
  min-height: calc(100% - 50px);
}

.SelectedApp {
  background-color: #23272f;
  display: block;
}

.HiddebApp {
  background-color: #181c24;
  display: none;
}

#Tab {
  background-color: #181c24;
}

#Tab .SelectedTab {
  display: inline-block;
  width: 200px;
  height: 50px;
  text-align: center;
  background-color: #23272f;
  user-select: none;
}

#Tab .TabHover {
  display: inline-block;
  width: 200px;
  height: 50px;
  text-align: center;
  background-color: #181c24;
  user-select: none;
  cursor: pointer;
}

#Tab .TabHover:hover{
  background-color: #333333;
}

.EmptySpace {
  width: 100%;
  height: 1px;
  padding: 0px;
  margin: 0px;
}

</style>
