<template>
  <div id="app">
    <header class="bm-header"></header>
    <bookmarks></bookmarks>
  </div>
</template>

<script type="text/ecmascript-6">
  import bookmarks from './components/bookmarks/bookmarks'

  export default {
    name: 'App',
    props: {
      bookmarks: {
        type: Object
      }
    },
    components: {
      bookmarks
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
</style>
