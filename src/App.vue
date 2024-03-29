<template>
  <div id="app">
    <Panel>
      <div id="nav">
        <router-link to="/">Home</router-link>|
        <router-link to="/about">About</router-link>
      </div>
      <router-view />
    </Panel>
    <Menus refresh debug />
  </div>
</template>

<script>
// Utility components, see here:
// https://github.com/Inventsable/lokney
import { Menus, Panel } from "lokney";
/*
  Panel component above also includes:
    - Starlette UI theme and color library: 
      https://github.com/Inventsable/starlette
    - CEP-Spy identification and app utility:
      https://github.com/Inventsable/cep-spy
 These are still installed into this panel and can be used when needed like so:
 import spy from 'cep-spy'

 NOTES: 
  - Starlette is already active in your panel! There's no need to initialize it.
  - Need CSInterface or a script? You can use the script-path attribute of Panel to launch scripts or utilities:
    https://github.com/Inventsable/lokney/tree/master/components/Panel
*/

// If you need to use evalScript, we need a specific method due to recent CORS policy changes in Adobe apps (as of AUG2021):
import { evalScript } from "cluecumber";

export default {
  name: "App",
  components: {
    Menus,
    Panel,
  },
  async mounted() {
    // If you need CEP-Spy:
    // let spy = require('cep-spy').default;
    // console.log(spy)

    // If you need to run JSX script:
    evalScript(`alert("Hello World")`).then((result) => {
      console.log(result); // This is only relevant if you need to return a value or execute code after the script
    });

    // You can also use async/await, though you'll need the async keyword prefix for mounted() or any particular method using it:
    let someScripting = await evalScript(`alert("I run first")`);
    console.log("I run second, since we're using async/await");
  },
  methods: {
    getCSS(prop) {
      // Returns current value of CSS variable
      // prop == typeof String as name of variable, with or without leading dashes:
      // this.getCSS('color-bg') || this.getCSS('--scrollbar-width')
      return window
        .getComputedStyle(document.documentElement)
        .getPropertyValue(`${/^\-\-/.test(prop) ? prop : "--" + prop}`);
    },
    setCSS(prop, data) {
      // Sets value of CSS variable
      // prop == typeof String as name of variable, with or without leading dashes:
      // this.setCSS('color-bg', 'rgba(25,25,25,1)') || this.setCSS('--scrollbar-width', '20px')
      document.documentElement.style.setProperty(
        `${/^\-\-/.test(prop) ? prop : "--" + prop}`,
        data
      );
    },
  },
};
</script>

<style>
#nav {
  padding: 10px;
  display: flex;
  justify-content: center;
  color: var(--color-default);
}

#nav a {
  padding: 0px 5px;
  font-weight: bold;
  color: var(--color-default);
}

#nav a.router-link-exact-active {
  color: var(--color-selection);
}
</style>
