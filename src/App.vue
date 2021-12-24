<template>
  <div id="app">
    <header class="header">
      <h1># T R E E #</h1>
      <h2>Tree in browser:</h2>
      <p class="pathInfo">
        <span class="pathInfo-prefix">Path to: </span>{{ pathToChild }}
      </p>
    </header>
    <div class="tree-list">
      <TreeList 
        :item="data"
        @showChild="showInfo"
      />
    </div>

  </div>
</template>

<script>
import TreeList from './components/TreeList.vue';
import data from './assets/node_modules.json';

export default {
  name: 'App',
  components: {
    TreeList
  },
  data(){
    return{
      data,
      pathToChild: '...',
    }
  },
  methods: {
    showInfo(obj){
      this.removeSelected();
      const {target, rm} = obj;
      if (rm === false){
        this.pathToChild = '...';
        this.pathToChild = this.getPath(target,'tree-list');
      }else{
        target.classList.add('selected');
        this.pathToChild = this.getPath(target,'tree-list');
      }
    },
    removeSelected(){
      const collection = document.querySelectorAll('.not-directory');
      collection.forEach( (node) => {
        node.classList.remove('selected');
      })
    },
    getPath(current, elClass){
      let path = '';
      const arr = [current.textContent];
      let parent = current.parentElement;
      while(parent.className !== elClass){
        if (parent.querySelector('.directory')?.textContent){
          arr.push(parent.querySelector('.directory')?.textContent);
        }
        parent = parent.parentElement;
      }
      arr.reverse();
      if (current.className === 'directory'){
        arr.splice(-2);
      }
      
      path = arr.join(' | ');
      return path;
    },
  }
}
</script>

<style>
html, body{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
h1,h2,p{
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.tree-list{
    text-align: left;
  }
.pathInfo{
  border: 2px darkolivegreen dashed;
  padding: 5px;
  text-align: left;
  margin-bottom: 5px;
}
.pathInfo-prefix{
  font-weight: bold;
}
.selected{
  border: 2px olive solid!important;
}
.header{
  padding: 7px 5px;
  position: sticky;
  top: 0;
  background-color: bisque;
}
*:focus-visible{
  outline: 1px solid rgba(255, 58, 58, 0.6);
}
</style>
  