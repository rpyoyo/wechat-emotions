<template>
  <div class="comment-input-wrapper">
    <div class="comment-input">
      <div class="div-input-area-wrapper">
        <div ref="comment" class="div-input-area" contenteditable="true" spellcheck="false" tabindex="1" data-placeholder="写评论..."
             v-html="content"></div>
      </div>
      <div class="emotions-switch">
        <i class="fa fa-emotions" @click="show = !show" v-show="!show"></i>
        <i class="fa fa-jianpan" @click="show = !show" v-show="show"></i>
      </div>
      <div class="btn-submit bg-primary" @click="submit">提交</div>
    </div>
    <div class="emotions" v-show="show">
      <div class="emotions-wrapper">
        <div class="emoji" :class="`emoji-${emoji}`" contenteditable="false" v-for="emoji in emojis"
             @click="choose(emoji)"></div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "WechatEmotionInput",
    data() {
      return {
        show: false,
        content: '',
        emojis: ["weixiao", "piezui", "se", "fadai", "deyi", "liulei", "haixiu", "bizui", "shui", "daku", "ganga", "fanu", "diaopi", "ziya", "jingya", "nanguo", "jiong", "zhuakuang", "tu", "touxiao", "yukuai", "baiyan", "aoman", "kun", "jingkong", "liuhan", "hanxiao", "youxian", "fendou", "zhouma", "yiwen", "xu", "yun", "shuai", "kulou", "qiaoda", "zaijian", "cahan", "koubi", "guzhang", "huaixiao", "zuohengheng", "youhengheng", "haqian", "bishi", "weiqu", "kuaikule", "yinxian", "qinqin", "kelian", "caidao", "xigua", "pijiu", "kafei", "zhutou", "meigui", "diaoxie", "zuichun", "aixin", "xinsui", "dangao", "zhadan", "bianbian", "yueliang", "taiyang", "yongbao", "qiang", "ruo", "woshou", "shengli", "baoquan", "gouyin", "quantou", "OK", "tiaotiao", "fadou", "ouhuo", "zhuanquan", "gaoxing", "kouzhao", "xiaoku", "tushetou", "shadai", "kongju", "beishang", "buxie", "heiha", "wulian", "jianxiao", "jizhi", "zhoumei", "ye", "guilian", "heshi", "jiayou", "qingzhu", "liwu", "hongbao", "ji"],
      }
    },
    created() {
    },
    methods: {
      choose(emoji) {
        this.content = this.$refs.comment.innerHTML + `<div class="emoji emoji-${emoji}" contenteditable="false"></div>`;
        this.$nextTick(()=>{
          this.keepLastIndex(this.$refs.comment);
        });
      },
      submit() {
        this.$emit("submit", this.$refs.comment.innerHTML);
        this.$refs.comment.innerHTML = "";
      },
      keepLastIndex(obj) {
        if (window.getSelection) {
          obj.focus();
          let range = window.getSelection();
          range.selectAllChildren(obj);
          range.collapseToEnd();
        } else if (document.selection) {
          let range = document.selection.createRange();
          range.moveToElementText(obj);
          range.collapse(false);
          range.select();
        }
      }
    }
  }
</script>

<style scoped lang="less">
  .comment-input-wrapper {
    position: fixed;
    bottom: 0;
    width: 100%;
    background-color: #F7F8FC;

    .comment-input {
      border-top: thin solid #cacaca;
      padding: 8px 15px;
      align-items: flex-end;
      display: flex;

      .div-input-area-wrapper {
        border-radius: 4px;
        padding: 10px;
        margin: 0;
        background-color: white;
        border: thin solid #e1e1e1;
        width: 100%;

        .div-input-area {
          height: auto;
          min-height: 20px;
          font-size: 14px;
          line-height: 20px;
          color: #333333;
          word-break: break-all;
          max-height: 80px;
          overflow: auto;
          -webkit-user-select: text;
          outline: none;

          &:empty:before {
            content: attr(data-placeholder);
            color: #cacaca;
          }
        }
      }

      .emotions-switch {
        width: 28px;
        height: 28px;
        margin: 0 8px 6px 8px;
        flex-shrink: 0;

        > i {
          font-size: 28px;
          display: block;
        }
      }

      .btn-submit {
        height: 32px;
        width: 54px;
        flex-shrink: 0;
        font-size: 14px;
        color: white;
        line-height: 32px;
        border-radius: 4px;
        text-align: center;
        margin-bottom: 4px;
      }
    }
  }

  .emotions {
    height: 220px;
    overflow-y: auto;
    overflow-x: hidden;

    .emotions-wrapper {
      margin: 0 auto;
      width: 352px;
      font-size: 0;
    }

    .emoji {
      width: 32px;
      height: 32px;
      margin: 6px;
    }
  }

</style>
