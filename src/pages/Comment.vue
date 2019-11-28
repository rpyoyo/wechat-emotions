<template>
  <div class="comments-content" @resize="resize">
    <div class="comment" v-for="({name, content, datetime, avatar, replies, showAll}, cidx) in comments" :key="cidx">
      <img :src="avatar" class="avatar" alt="">
      <div class="comment-info">
        <div class="flex-align-center flex-space-between">
          <div>
            <p class="fs-14 tc-3">{{name}}</p>
            <p class="fs-13 tc-9">{{datetime}}</p>
          </div>
          <i class="fa fa-message l tc-9"></i>
        </div>
        <div class="comment-content" v-html="content"></div>

        <div class="replies" :class="showAll ? 'all' : ''" v-if="replies.length > 0">
          <div class="reply-content" v-for="({name, content}, ridx) in replies" :key="ridx">
            <span>{{name}}: </span>
            <div v-html="content" class="d-inline"></div>
          </div>
          <div class="reply-toggle" v-show="!showAll && replies.length > 2">
            <em class="more" @click="toggle(cidx)">共{{replies.length}}条回复 <i
              class="fa fa-arrow-bottom"></i></em>
          </div>
          <div class="reply-toggle" v-show="showAll && replies.length > 2">
            共{{replies.length}}条回复 <em class="less" @click="toggle(cidx)">收起 <i
            class="fa fa-arrow-top"></i></em>
          </div>
        </div>
      </div>
    </div>
    <wechat-emotion-input @submit="submit"/>
  </div>
</template>

<script>
  import wechatEmotionInput from "../components/WechatEmotionInput"

  export default {
    components: {
      wechatEmotionInput
    },
    data() {
      return {
        template: {
          avatar: require("../assets/images/6.jpg"),
          name: "rpyoyo",
          replies: [],
          showAll: false,
        },
        comments: [
          {
            avatar: require("../assets/images/1.jpg"),
            name: "Eva Lee",
            datetime: "2019/11/10 12:20",
            content: "这车型都很有个性，这样年轻化的内饰果然还是比较适合年轻人~",
            replies: [
              {name: "Awaatif Al Sadek", content: "不知道开起来会是怎样"},
              {name: "Kong Yijun", content: "看上去外形美观大气，内部空间宽敞，感觉还挺不错的"},
              {name: "Lilah Ioselev", content: "我很喜欢白色，挺前卫的"},
            ],
            showAll: false
          },
          {
            avatar: require("../assets/images/2.jpg"),
            name: "Ekene Obasey",
            datetime: "2019/11/10 12:13",
            content: "挺不错的车子，关键是自己开着顺手才最重要",
            replies: [
              {name: "Olivia Evans", content: "方向盘有多重功能"},
              {name: "Lu Zhou", content: "一款特别大气的车，它的配置也是相当的不错哟～"},
            ],
            showAll: false
          },
          {
            avatar: require("../assets/images/3.jpg"),
            name: "Oslo",
            datetime: "2019/11/09 18:52",
            content: "配置还是可以的，天窗，雾灯，自动巡航。。。",
            replies: [
              {name: "Xian Zhou", content: "cool，就是喜欢"},
            ],
            showAll: false
          },
          {
            avatar: require("../assets/images/4.jpg"),
            name: "Ainara Vergara",
            datetime: "2019/11/08 21:22",
            content: "内部空间宽敞，感觉还挺不错的",
            replies: [
            ],
            showAll: false
          },
          {
            avatar: require("../assets/images/5.jpg"),
            name: "Dashonte Clarke",
            datetime: "2019/11/05 21:51",
            content: "开着这车出去兜风应该不错",
            replies: [
            ],
            showAll: false
          },
        ]
      }
    },
    created() {
    },
    methods: {
      submit(content) {
        if(content.length > 0){
          this.comments = [{...this.template, datetime: new Date().format("yyyy/MM/dd hh:mm"), content}, ...this.comments]
        }
      },
      toggle(index){
        this.comments[index].showAll = !this.comments[index].showAll;
      },
      resize(){
        this.nextTick(()=>{
          window.scrollTo(window.scrollX, window.scrollY + 1);
        })
      }
    }
  }
</script>

<style lang="less" scoped>
  .comments-content {
    padding: 10px 15px 100px 15px;

    .comment {
      padding: 15px 0;
      display: flex;
      border-top: thin solid #cacaca;

      &:first-of-type {
        border-top: none;
      }

      .avatar {
        flex: 40px 0 0;
        height: 40px;
        margin-right: 12px;
        border-radius: 50%;
      }

      .comment-info {
        width: 100%;

        .comment-content {
          font-size: 14px;
          line-height: 20px;
          margin-top: 10px;
          word-break: break-all;
          color: #333333;
        }

        .replies {
          margin-top: 12px;
          background: #F3F4F9;
          border-radius: 2px;
          padding: 10px;

          .reply-content {
            margin-top: 5px;
            font-size: 13px;
            line-height: 18px;
            color: #333333;
            display: none;
            word-break: break-all;

            &:nth-of-type(1), &:nth-of-type(2){
              display: block;
            }

            span {
              color: #2a2a2a;
              font-weight: 600;
            }

            &:first-of-type {
              margin-top: 0;
            }
          }

          &.all {
            .reply-content {
              display: block;
            }
          }

          .reply-toggle {
            font-size: 13px;
            line-height: 18px;
            margin-top: 5px;
          }

          .less, .more {
            color: #6f9be1;
            i {
              font-size: 12px;
            }
          }
        }
      }
    }
  }
</style>
