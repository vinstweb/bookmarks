<template>
  <div class="bookmarks">
    <ul class="left-wrapper">
      <li class="top-item" v-for="(bookmark, index) in bookmarks" :key="index">
        <a href="javascript:;" class="top-name">{{bookmark.name}}</a>
        <ul v-if="bookmark.items || bookmark.items.length>0">
          <li class="sub-item" v-for="(item, index) in bookmark.items" :key="index">
            <a href="javascript:;" class="sub-name">{{item.name}}</a>
          </li>
        </ul>
      </li>
    </ul>
  </div>
</template>

<script type="text/ecmascript-6">
  const ERR_OK = 0
  export default {
    props: {},
    data() {
      return {
        bookmarks: []
      }
    },
    created() {
      this.$http.get('/api/bookmarks').then((response) => {
        response = response.body
        if (response.errno === ERR_OK) {
          this.bookmarks = response.data
        }
      })
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">

</style>
