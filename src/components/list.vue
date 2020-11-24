<template>

  <ul on:drop="()=> {this.$emit('ended', this.value.id)}">
    <li>
      {{ value.text }}
    </li>
    <div class="toggleBox">
      <input class='toggle' type='button' value='OK' v-on:click='end' v-if='value.isHidden'/>
      <input class='toggle' type='button' value='X' v-on:click='dele' />
    </div>
  </ul>

</template>

<script>
export default {
  name: 'list',
  props: ['value'],
  methods: {
    end: function () {
      this.$emit('ended', this.value.id);
    },
    dele: function () {
      this.$emit('delete', this.value.id);
    },
  },
};
</script>

<style>
ul{
  width: 50%;
  height: 5em;
  background-color: rgba(255, 255, 255, 0.4);
  margin: 5% auto;
  border-radius: 1em;
  padding-left: 0;
  transition: 0.8s;
}
ul:hover{background-color: rgba(117, 92, 143, 0.24); transition: 1s;}
.fade-enter-active, .fade-leave-active {
  transition: opacity .7s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
li {
  width: 70%;
  line-height: 4em;
  padding-top: 1%;
  list-style: none;
  text-align: center;
  border-radius: 0.7em;
  text-align: center;
  float: left;
}
.toggleBox{
  width: 30%;
  float: left;
  margin: 6% 0;
}
.toggle{
  width: 30%;
  height: 2em;
  border: none;
  background-color: rgba(67, 54, 189, 0.1);
  border-radius: 0.7em;
}  
ul:hover .toggle{background-color: rgba(247, 244, 255, 0.671); transition: 1s;}

</style>
