<template>
  <div id="app">
    <VueTerminal />
  </div>
</template>

<script>
import VueTerminal from "./components/terminal";

export default {
  name: "App",
  components: { VueTerminal }
};
</script>

<style>
header {
  display: flex;
  justify-content: space-between;
}

.title {
  font-size: 40px;
  padding-left: 15px;
}

.buttons {
  align-self: center;
  margin: 0 20px;
}

button {
  margin: 0 5px;
  border: 1px solid rgb(0, 238, 0);
  background: #444;
  color: #fff;
}

body {
  margin: 0;
  padding: 0;
  width: 100vw;
  font: 16px Courier;
}

#cmd {
  width: calc(100% - 20px);
  height: calc(100% - 16%);
  margin: 0;
  padding: 10px;
  overflow-y: scroll;
}

#cmdbar {
  position: fixed;
  left: 0;
  padding: 10px;
  margin: 0;
  width: calc(100% - 20px);
  height: 40px;
  background: #323232;
  border-top: 1px solid #444;
}

#command {
  width: calc(100% - 10px);
  height: 36px;
  border: 1px solid #444;
  background: #222;
  color: #0e0;
  outline: none;
  padding-left: 8px;
  font-family: courier;
}
</style>
