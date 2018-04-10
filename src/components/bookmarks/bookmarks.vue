<template>
  <div class="bookmarks">
    <h1 class="tip">持续更新...</h1>
    <div class="bookmarks-wrapper">
      <ul class="left-wrapper">
        <li class="top-item">
          <a href="javascript:;" @click="updateList(-1, -1)" class="top-name"><i class="icon icon-zh_strong"></i>综合推荐</a>
        </li>
        <li class="top-item" v-for="(bookmark, index) in bookmarks" :key="index">
          <a href="javascript:;" @click="updateList(bookmark.type, -1)" class="top-name"><i class="icon" :class="bookmark.icon"></i>{{bookmark.name}}</a>
          <ul class="sub-list" v-if="bookmark.sublist || bookmark.sublist.length>0">
            <li class="sub-item" v-for="(sub, index) in bookmark.sublist" :key="index">
              <a href="javascript:;" @click="updateList(bookmark.type, sub.type)" class="sub-name">{{sub.name}}</a>
            </li>
          </ul>
        </li>
      </ul>
      <div class="content-wrapper">
        <a class="content-item" v-for="(item, index) in curList" :key="index" :href="item.url" target="_blank">
          <div class="top-wrapper">
            <span class="pic"><img width="40" height="40" :src="item.pic && item.pic !== ''?item.pic:defaultPic" onerror="this.onerror='';src='../../static/imgs/default.png'"></span>
            <div class="title">
              <span class="name" :title="item.name">{{item.name}}</span>
              <span class="url" :title="item.url">{{item.url}}</span>
            </div>
          </div>
          <div class="description" :title="item.description">{{item.description}}</div>
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
        curTopIndex: -1,
        curSubIndex: -1,
        defaultPic: '../../static/imgs/default.png'
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
        let curArr = []
        if (this.curTopIndex === -1) {
          this.bookmarks.forEach((topli) => {
            topli.sublist.forEach((subli) => {
              subli.items.forEach((item) => {
                if (!curArr.includes(item)) {
                  curArr.push(item)
                }
              })
            })
          })
        } else if (this.curSubIndex === -1) {
          this.bookmarks[this.curTopIndex].sublist.forEach((subli) => {
            subli.items.forEach((item) => {
              if (!curArr.includes(item)) {
                curArr.push(item)
              }
            })
          })
        } else {
          this.bookmarks[this.curTopIndex].sublist[this.curSubIndex].items.forEach((item) => {
            if (!curArr.includes(item)) {
                curArr.push(item)
              }
          })
        }
        return curArr
      }
    },
    methods: {
      updateList(toptype, subtype) {
        this.curTopIndex = toptype
        this.curSubIndex = subtype
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
body
  background: #eee
  .bookmarks
    position: relative
    margin-top: 48px
    padding-top: 50px
    width: 100%
    border-top: 1px solid rgba(7, 17, 27, 0.1)
    .tip
      position: absolute
      top:0
      left:0
      width:100%
      line-height: 50px
      text-align: center
      font-size: 20px
      font-weight: 700
      color: #ccc
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
      @media screen and (max-width: 530px)
        .left-wrapper
          display: none
      .content-wrapper
        flex: 1
        .content-item
          width: 23%
          min-width:110px;
          margin: 6px 1%
          height: 110px
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
            display: -webkit-box;
            overflow: hidden
            height: 32px
            line-height: 16px
            -webkit-box-orient: vertical;
            -webkit-line-clamp: 2;
            padding: 10px 0 0 5px
            font-size: 12px
            color: #8c8d8f
        @media screen and (max-width: 1040px)
          .content-item
            width: 30%
            margin: 6px 1.5%
        @media screen and (max-width: 880px)
          .content-item
            width: 48%
            margin: 6px 1%
        @media screen and (max-width: 400px)
          .content-item
            width: 100%
            margin: 6px 0
</style>
