<template>
  <div class="bookmarks">
    <div class="bookmarks-wrapper">
      <ul class="left-wrapper">
        <li class="top-item" v-for="(bookmark, index) in bookmarks" :key="index">
          <a @click="tabNav" href="javascript:;" class="top-name"><i class="icon" :class="bookmark.icon"></i>{{bookmark.name}}</a>
          <ul class="sub-list" v-if="bookmark.sublist || bookmark.sublist.length>0">
            <li class="sub-item" v-for="(sub, index) in bookmark.sublist" :key="index">
              <a href="javascript:;" class="sub-name">{{sub.name}}</a>
            </li>
          </ul>
        </li>
      </ul>
      <div class="content-wrapper">
        <div class="content-item" v-for="(item, index) in curList" :key="index">
          <div class="top-wrapper">
            <span class="pic"><img :src="item.pic"></span>
            <div class="title">
              <span class="name">{{item.name}}</span>
              <span class="url">{{item.url}}</span>
            </div>
          </div>
          <div class="description">{{item.description}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  const ERR_OK = 0
  export default {
    data() {
      return {
        bookmarks: [],
        curList: [],
        curTopIndex: 0,
        curSubIndex: 0
      }
    },
    created() {
      this.$http.get('/api/bookmarks').then((response) => {
        response = response.body
        if (response.errno === ERR_OK) {
          this.bookmarks = response.data
        }
      })
    },
    methods: {
      tabNav() {
        this.curList = this.bookmarks[this.curTopIndex].sublist[this.curSubIndex].items
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .bookmarks
    margin-top: 48px
    padding-top: 50px
    width: 100%
    border-top: 1px solid rgba(7, 17, 27, 0.1)
    .bookmarks-wrapper
      display: flex
      width: 80%
      max-width: 1040px
      margin: 0px auto
      .left-wrapper
        flex: 0 0 120px
        width: 120px
        margin-right: 40px
        border-right: 1px solid rgba(7, 17, 27, 0.1)
        .top-item
          margin-bottom: 10px
          .top-name
            display: block
            height: 16px
            line-height: 16px
            padding: 4px 0
            margin-bottom: 4px
            font-size: 16px
            .icon
              display: inline-block
              vertical-align: top
              margin-right: 5px
              height: 16px
              line-height: 16px
          .sub-list
            padding-left: 30px
            .sub-item
              font-size: 0
              .sub-name
                display: block
                height: 14px
                line-height: 14px
                padding: 6px 0
                font-size: 14px
      .content-wrapper
        display: flex
        flex: 1
</style>
