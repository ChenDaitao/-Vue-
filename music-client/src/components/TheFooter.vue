<template>
  <div class="the-footer">
    <p>{{list}}</p>
  </div>
</template>

<script>
export default {
  name: 'the-footer',
  data () {
    return {
      list: '音乐使一个民族的气质更高贵',
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../assets/css/the-footer.scss';
</style>
