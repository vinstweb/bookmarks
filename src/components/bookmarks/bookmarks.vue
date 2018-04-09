<template>
  <div class="bookmarks">
    <div class="bookmarks-wrapper">
      <ul class="left-wrapper">
        <li class="top-item" v-for="(bookmark, index) in bookmarks" :key="index">
          <a href="javascript:;" class="top-name"><i class="icon" :class="bookmark.icon"></i>{{bookmark.name}}</a>
          <ul class="sub-list" v-if="bookmark.sublist || bookmark.sublist.length>0">
            <li class="sub-item" v-for="(sub, index) in bookmark.sublist" :key="index">
              <a href="javascript:;" class="sub-name">{{sub.name}}</a>
            </li>
          </ul>
        </li>
      </ul>
      <div class="content-wrapper">
        <a class="content-item" v-for="(item, index) in curList" :key="index" :href="item.url" target="_blank">
          <div class="top-wrapper">
            <span class="pic"><img width="40" height="40" :src="item.pic"></span>
            <div class="title">
              <span class="name">{{item.name}}</span>
              <span class="url">{{item.url}}</span>
            </div>
          </div>
          <div class="description">{{item.description}}</div>
        </a>
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
        // curList: [],
        curTopIndex: -1,
        curSubIndex: -1
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
    computed: {
      curList() {
        var curArr = []
        if (this.curTopIndex === -1) {
          for (var topli of this.bookmarks) {
            for (var subli of topli.sublist) {
              for (var item of subli.items) {
                if (curArr.includes(item)) {
                  curArr.push(item)
                }
              }
            }
          }
        }
        return curArr
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
body
  background: #eee
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
        flex-wrap: wrap
        .content-item
          flex: 0 0 23%;
          margin: 6px 1%
          display: inline-block
          box-sizing: border-box
          padding: 13px 15px 15px 15px
          overflow: hidden
          background: #f6f6f6
          border-radius: 6px
          transition: all 0.2s
          box-shadow: 0px 4.33px 5px 0px rgba( 111, 111, 111, 0.1 )
          &:hover
            box-shadow: 0px 4.33px 5px 0px rgba( 1, 1, 1, 0.25 )
          .top-wrapper
            position: relative
            height: 40px
            .pic
              position: absolute
              display: block
              left: 0
              top: 0
              width: 40px
              height: 40px
              border-radius: 50%
            .title
              display: block
              padding-left: 50px
              font-size: 0
              .name
                display: block
                width:100%
                height: 14px
                line-height: 14px
                padding-top: 9px
                overflow: hidden
                text-overflow: ellipsis
                white-space: nowrap
                font-size: 14px
                font-weight: 700
                color: #46474f
              .url
                display: block
                width:100%
                height: 14px
                line-height: 14px
                overflow: hidden
                text-overflow: ellipsis
                white-space: nowrap
                font-size: 12px
                color: #a4a4a4
          .description
            height: 32px
            line-height: 16px
            padding: 10px 0 0 5px
            font-size: 12px
            color: #8c8d8f
</style>
