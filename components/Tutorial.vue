<template>
  <div>
    <div class="todo__card">
      <h3 class="todo__card__title">
        Активный список
      </h3>
      <div class="list__items">
        <div class="list__item-wrapper">
          <div
            v-for="item in list"
            :key="item.id"
            class="list__item"
            :class="{'list__item-active': item.checked}"
          >
            <input
              :id=item.id
              type="checkbox"
              class="custom-checkbox"
              name="happy"
              :checked="item.checked"
              @change="toggleChange(item.id)"
            >
            <label :for=item.id />
            <div class="list__item-text">
              {{ item.text }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'NuxtTutorial',
  data () {
    return {
      toggle: false,
      list: null
    }
  },
  mounted () {
    this.getIncidents()
  },
  methods: {
    async getIncidents () {
      await this.$axios.get('http://localhost:3001/posts')
        .then((response) => {
          this.list = Object.assign(response.data, { checked: false })
          console.log(this.list)
        })
    },
    toggleChange (id) {
      const index = this.list.findIndex(e => e.id === id)
      if (index < 0) { return }
      this.list[index].checked = !this.list[index].checked
      this.list = [...this.list]
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
}

.custom-checkbox:checked+label::before {
  border-color: transparent;
  background-color: #F2C3A7;
  background-size: 13px 11px;
  background-image: url(~assets/img/svg/checkbox.svg);
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
        & .list__item-text{
          color: #F2C3A7;
          font-size: 14px;
        }
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
