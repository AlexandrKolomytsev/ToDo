<template>
  <div>
    <div>
      <div class="todo__card">
        <h3 class="todo__card__title">
          {{ item.noteName }}
        </h3>
        <div class="list__items">
          <div class="list__item-wrapper">
            <list-item v-for="item in list" :key="item.id" :item="item" />
          </div>
        </div>
        <div class="close__circle" @click="removeBlock(item.id)" />
        <div class="list__items">
          <div class="list__item-wrapper" />
        </div>
      </div>
      <div class="add__to-do">
        <p
          class="add__to-do-text"
          :class="{ hide: addHide===false }"
        >
          laboriosam mollitia et enim quasi adipisci quia provident illum
        </p>
        <input
          v-model="inputText"
          class="add__input"
          type="text"
          :class="{ hide: addHide===true }"
          @keyup.enter="postRequest"
        >
        <img
          class="add__image"
          src="~assets/img/svg/add.svg"
          alt=""
          @click="change"
        >
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'NewBlock',
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
      inputText: '',
      addHide: true,
      list: null
    }
  },
  mounted () {
    this.getIncidents()
  },
  methods: {
    async getIncidents () {
      await this.$axios.get('http://localhost:3001/todos')
        .then((response) => {
          this.list = Object.assign(response.data)
        })
    },
    postRequest () {
      this.$axios.post('http://localhost:3001/todos', {
        text: this.inputText,
        checked: false
      })
        .then((response) => {
          console.log(response)
          this.inputText = ''
          this.updateKey('updateKey')
        })
    },
    removeBlock (id) {
      this.$axios.delete('http://localhost:3001/lists/' + `${id}`)
        .then((response) => {
          this.updateKey()
        })
    },
    change () {
      this.addHide = !this.addHide
    }
  }
}
</script>

<style lang="scss" scoped>
.hide {
  display: none;
}
.add__to-do {
  width: 320px;
  height: 72px;
  background: #F2C3A7;
  border-radius: 8px;
  margin: 10px 0 0 30px;
  font-style: normal;
  font-weight: 500;
  font-size: 14px;
  line-height: 16px;
  display: flex;
  align-items: center;
  color: #734D3F;

  & .add__to-do-text {
    width: 222px;
    height: 32px;
    margin: 20px 38px 20px 20px;
  }

  & .add__input {
    width: 222px;
    height: 32px;
    margin: 20px 38px 20px 20px;
  }

  & .add__image {
    cursor: pointer;
  }

}

.custom-checkbox {
  position: absolute;
  z-index: -1;
  opacity: 1;
}

.custom-checkbox + label {
  display: inline-flex;
  align-items: center;
  user-select: none;
}

.custom-checkbox + label::before {
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
}

.custom-checkbox:checked + label::before {
  border-color: transparent;
  background-color: #F2C3A7;
  background-size: 13px 11px;
  background-image: url(~assets/img/svg/checkbox.svg);
}

.todo__card {
  position: relative;
  color: #734D3F;
  font-style: normal;
  font-weight: 500;
  font-size: 20px;
  line-height: 23px;
  margin: 30px 30px 0;
  width: 320px;
  height: 558px;
  background: #F2C3A7;
  border-radius: 8px;

  & .close__circle{
    z-index: 10;
    content: url(~assets/img/svg/clouse.svg);
    position: absolute;
    top: 0;
    left: 100%;
    transform: translate(-50%, -40%);
    cursor: pointer;
  }

  & .todo__card__title {
    font-size: 20px;
    padding-top: 20px;
    margin: 0;
    display: flex;
    justify-content: center;
  }

  & .list__items {
    padding: 15px 0 0 20px;

    & .list__item-wrapper {
      & .list__item {
        margin: 10px 20px 0 0;
        background: #734D3F;
        border-radius: 8px;
        padding: 7px 10px 7px;
        display: flex;
        justify-content: left;
        gap: 7px;
        align-items: center;
        width: fit-content;

        & .list__item-text {
          color: #F2C3A7;
          font-size: 14px;
        }
      }

      & .list__item-active {
        background-color: #BF8C6F;
        text-decoration: line-through;
        text-decoration-color: #F2C3A7;
      }
    }
  }
}
</style>
