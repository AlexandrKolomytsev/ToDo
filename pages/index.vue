<template>
  <div>
    <active-list :key="listKey" @updateKey="updateKey" />
  </div>
</template>

<script>
import ActiveList from '../components/ActiveList'
export default {
  name: 'IndexPage',
  components: { ActiveList },
  provide () {
    return {
      updateKey: this.updateKey
    }
  },
  data () {
    return {
      listKey: 0
    }
  },
  methods: {
    updateKey () {
      this.listKey += 1
    }
  }
}
</script>
