<template>
  <div class="add__block">
    <input v-model="inputNameNote" type="text" class="input__add" :class="{hide: addHide}" @keyup.enter="addBlock">
    <div class="circle__block" @click="openInput">
      <div class="line__plus" />
      <div class="line__plus rotate" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'AddBlock',
  inject: ['updateKey'],
  data () {
    return {
      addHide: true,
      inputNameNote: ''
    }
  },
  methods: {
    addBlock () {
      this.$axios.post('http://localhost:3001/lists', {
        noteName: this.inputNameNote,
        mas: []
      })
        .then(() => {
          this.updateKey()
        })
    },
    openInput () {
      this.addHide = !this.addHide
    }
  }
}
</script>

<style lang="scss">
.hide{
  display: none;
}
.add__block{
  width: 320px;
  height: 640px;
  border: 5px dashed #F2C3A7;
  box-sizing: border-box;
  filter: drop-shadow(0px 4px 4px rgba(0, 0, 0, 0.25));
  border-radius: 8px;
  margin-top: 30px;
  position: relative;
  cursor: pointer;

  & .input__add{
    position: absolute;
    top: 15px;
    transform: translate(-50%, 0);
    left: 50%;
    width: 222px;
    height: 32px;
  }

& .circle__block {
    position: absolute;
    width: 200px;
    height: 200px;
    left: 60px;
    top: 220px;
    border: 5px dashed #F2C3A7;
    box-sizing: border-box;
    border-radius: 100px;

& .line__plus{
    position: absolute;
    width: 100px;
    height: 20px;
    left: 23%;
    top: 44%;
    background: #F2C3A7;
    transform: rotate(-90deg);
  }
& .rotate{
    transform: rotate(0);
  }
}
}
</style>
