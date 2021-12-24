<template>
  <div>
    <p class="tree-list-item"
      :style="[{'margin-left': `${depth * 20}px`}]"
      @click.stop="toogleOpenClose"
      tabindex="0"
      @keyup.enter="toogleOpenClose"
    >
      <span 
        class="type"
        v-if="hasChildren"
      >
      <template v-if="opened">
        <img src="https://img.icons8.com/material-sharp/24/000000/filled-minus-2-math.png"/>
      </template>
      <template v-else>
        <img src="https://img.icons8.com/material-sharp/24/000000/filled-plus-2-math.png"/>
      </template>
      </span>
      <span class="marker" v-else ><img src="https://img.icons8.com/material-sharp/24/000000/filled-circle.png"/></span>
      <span
        :class="setClass(item)"
      >{{item.name }}</span>
    </p>

    <template
      v-if="opened"
    >
      <TreeList 
        v-for="child in item.contents"
        :key="child.name"
        :item="child"
        :depth="depth + 1"
        @showChild="(item) => $emit('showChild',item)"
      />
    </template>
  </div>
</template>

<script>
export default {
  name: 'TreeList',
  props: {
    item: Object,
    depth: {
      type: Number,
      default: 0,
    }
  },
  data() {
    return {
      opened: false,
    }
  },
  computed: {
    hasChildren(){
      return this.item.contents;
    }
  },
  methods: {
    toogleOpenClose(event){
      this.opened = !this.opened;
      if (!this.hasChildren){
        this.$emit('showChild',{target: event.target, rm: true});
      }else{
        this.$emit('showChild',{target: event.target, rm: false});
      }
    },
    setClass(item){
      if (item.type === 'directory'){
        return 'directory';
      }else if (item.type === 'file'){
        return 'file not-directory';
      }else if (item.type === 'link'){
        return 'link';
      }
    },
  }
}
</script>

<style scoped>
  .tree-list-item{
    margin: 1px 0px;
    display: inline-flex;
    align-items: center;
  }
  .tree-list-item:hover{
    cursor: pointer;
  }
  .type{
    display: inline-block;
    width: 24px;
    height: 24px;
    margin-right: 4px;
  }
  .marker{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 20px;
  }
  .marker img{
    width: 30%;
    height: 30%;
  }
  .directory{
    color: brown;
    font-weight: bold;
  }
  .file{
    color: orange;
  }
  .link{
    color: blue;
    text-decoration: underline;
  }
  .not-directory{
    font-size: .9em;
    padding: 2px;
    border: 2px transparent solid;
  }
</style>