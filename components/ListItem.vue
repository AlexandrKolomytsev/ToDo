<template>
  <div>
    <div
      class="list__item"
      :class="{'list__item-active': item.checked}"
      @mouseover="closeCircleActive = true"
      @mouseleave="closeCircleActive = false"
    >
      <div :class="{close__circle: closeCircleActive}" @click="deleteRequest(item.id)" />
      <input
        :id="item.id"
        type="checkbox"
        class="custom-checkbox"
        name="happy"
        :checked="item.checked"
        @change="toggleChange(item.id, item.checked)"
      >
      <label :for="item.id" />
      <div class="list__item-text">
        {{ item.text }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ListItem',
  inject: ['updateKey'],
  props: {
    item: {
      type: Object,
      default: null,
      require: true
    }
  },
  data () {
    return {
      closeCircleActive: false
    }
  },
  methods: {
    toggleChange (id, checked) {
      if (checked === false) {
        this.$axios.patch('http://localhost:3001/todos/' + `${id}`, {
          checked: true
        })
          .then(() => {
            this.updateKey()
          })
      }
      if (checked === true) {
        this.$axios.patch('http://localhost:3001/todos/' + `${id}`, {
          checked: false
        })
          .then(() => {
            this.updateKey()
          })
      }
    },
    deleteRequest (id) {
      this.$axios.delete('http://localhost:3001/todos/' + `${id}`)
        .then((response) => {
          this.inputText = ''
          this.updateKey()
        })
    }
  }
}
</script>

<style lang="scss">
.custom-checkbox{
  position: absolute;
  z-index: -1;
  opacity: 1;
}
.custom-checkbox+label {
  display: inline-flex;
  align-items: center;
  user-select: none;
}
.custom-checkbox+label::before {
  content: '';
  display: inline-block;
  width: 16px;
  height: 16px;
  flex-shrink: 0;
  flex-grow: 0;
  border: 1px solid transparent;
  background-repeat: no-repeat;
  background-position: center center;
  background-size: 50% 50%;
  background-color: #F2C3A7;
  border-radius: 4px;
  cursor: pointer;
}

.custom-checkbox:checked+label::before {
  border-color: transparent;
  background-color: #F2C3A7;
  background-size: 13px 11px;
  background-image: url(~assets/img/svg/checkbox.svg);
  cursor: pointer;
}
.todo__card{
  color: #734D3F;
  font-style: normal;
  font-weight: 500;
  font-size: 20px;
  line-height: 23px;
  margin: 30px;
  width: 320px;
  height: 558px;
  background: #F2C3A7;
  border-radius: 8px;
  & .todo__card__title{
    padding-top: 20px;
    display: flex;
    justify-content: center;
  }
  & .list__items{
    padding: 15px 0 0 20px;
    & .list__item-wrapper{
      & .list__item{
        margin: 10px 20px 0 0;
        background: #734D3F;
        border-radius: 8px;
        padding: 7px 10px 7px;
        display: flex;
        justify-content: left;
        gap: 7px;
        align-items: center;
        width: fit-content;
        position: relative;
        & .list__item-text{
          color: #F2C3A7;
          font-size: 14px;
        }
      }
      & .close__circle{
        z-index: 10;
        content: url(~assets/img/svg/clouse.svg);
        position: absolute;
        top: 0;
        left: 100%;
        transform: translate(-50%, -40%);
        cursor: pointer;
      }
      & .list__item-active{
        background-color: #BF8C6F;
        text-decoration: line-through;
        text-decoration-color: #F2C3A7;
      }
    }
  }
}
</style>
