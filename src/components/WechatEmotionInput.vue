<template>
  <div class="comment-input-wrapper">
    <div class="comment-input">
      <div class="div-input-area-wrapper">
        <div ref="comment" class="div-input-area" contenteditable="true" spellcheck="false" tabindex="1"
             data-placeholder="写评论..."
             v-html="content"></div>
      </div>
      <div class="emotions-switch">
        <i class="fa fa-emotions" @click="open" v-show="!show"></i>
        <i class="fa fa-jianpan" @click="input" v-show="show"></i>
      </div>
      <div class="btn-submit bg-primary" @click="submit">提交</div>
    </div>
    <div class="emotions" v-show="show" ref="target">
      <div class="emotions-wrapper">
        <i class="emoji" :class="`emoji-${emoji}`" v-for="emoji in emojis"
           @click="choose(emoji)"></i>
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
        map1: {'OK': 'OK', 'buxie': '不屑', 'qinqin': '亲亲', 'bianbian': '便便', 'touxiao': '偷笑', 'aoman': '傲慢', 'shadai': '傻呆', 'zaijian': '再见', 'diaoxie': '凋谢', 'shanchu': '删除', 'jiayou': '加油', 'gouyin': '勾引', 'fadai': '发呆', 'fanu': '发怒', 'fadou': '发抖', 'kouzhao': '口罩', 'kelian': '可怜', 'youhengheng': '右哼哼', 'heshi': '合十', 'tu': '吐', 'tushetou': '吐舌头', 'ziya': '呲牙', 'zhouma': '咒骂', 'kafei': '咖啡', 'haqian': '哈欠', 'pijiu': '啤酒', 'xu': '嘘', 'zuichun': '嘴唇', 'heiha': '嘿哈', 'jiong': '囧', 'kun': '困', 'huaixiao': '坏笑', 'daku': '大哭', 'taiyang': '太阳', 'fendou': '奋斗', 'jianxiao': '奸笑', 'weiqu': '委屈', 'haixiu': '害羞', 'ganga': '尴尬', 'zuohengheng': '左哼哼', 'qingzhu': '庆祝', 'ruo': '弱', 'qiang': '强', 'deyi': '得意', 'weixiao': '微笑', 'xinsui': '心碎', 'kuaikule': '快哭了', 'ouhuo': '怄火', 'kongju': '恐惧', 'youxian': '悠闲', 'beishang': '悲伤', 'jingkong': '惊恐', 'jingya': '惊讶', 'yukuai': '愉快', 'hanxiao': '憨笑', 'zhuakuang': '抓狂', 'koubi': '抠鼻', 'baoquan': '抱拳', 'yongbao': '拥抱', 'quantou': '拳头', 'wulian': '捂脸', 'woshou': '握手', 'piezui': '撇嘴', 'cahan': '擦汗', 'qiaoda': '敲打', 'yun': '晕', 'yueliang': '月亮', 'jizhi': '机智', 'liuhan': '流汗', 'liulei': '流泪', 'zhadan': '炸弹', 'aixin': '爱心', 'zhutou': '猪头', 'meigui': '玫瑰', 'yiwen': '疑问', 'baiyan': '白眼', 'zhoumei': '皱眉', 'shui': '睡', 'liwu': '礼物', 'xiaoku': '笑哭', 'hongbao': '红包', 'ye': '耶', 'shengli': '胜利', 'se': '色', 'caidao': '菜刀', 'dangao': '蛋糕', 'shuai': '衰', 'xigua': '西瓜', 'diaopi': '调皮', 'tiaotiao': '跳跳', 'zhuanquan': '转圈', 'bishi': '鄙视', 'bizui': '闭嘴', 'yinxian': '阴险', 'nanguo': '难过', 'kulou': '骷髅', 'gaoxing': '高兴', 'guilian': '鬼脸', 'ji': '鸡', 'guzhang': '鼓掌'},
        map2: {'OK': 'OK', '不屑': 'buxie', '亲亲': 'qinqin', '便便': 'bianbian', '偷笑': 'touxiao', '傲慢': 'aoman', '傻呆': 'shadai', '再见': 'zaijian', '凋谢': 'diaoxie', '删除': 'shanchu', '加油': 'jiayou', '勾引': 'gouyin', '发呆': 'fadai', '发怒': 'fanu', '发抖': 'fadou', '口罩': 'kouzhao', '可怜': 'kelian', '右哼哼': 'youhengheng', '合十': 'heshi', '吐': 'tu', '吐舌头': 'tushetou', '呲牙': 'ziya', '咒骂': 'zhouma', '咖啡': 'kafei', '哈欠': 'haqian', '啤酒': 'pijiu', '嘘': 'xu', '嘴唇': 'zuichun', '嘿哈': 'heiha', '囧': 'jiong', '困': 'kun', '坏笑': 'huaixiao', '大哭': 'daku', '太阳': 'taiyang', '奋斗': 'fendou', '奸笑': 'jianxiao', '委屈': 'weiqu', '害羞': 'haixiu', '尴尬': 'ganga', '左哼哼': 'zuohengheng', '庆祝': 'qingzhu', '弱': 'ruo', '强': 'qiang', '得意': 'deyi', '微笑': 'weixiao', '心碎': 'xinsui', '快哭了': 'kuaikule', '怄火': 'ouhuo', '恐惧': 'kongju', '悠闲': 'youxian', '悲伤': 'beishang', '惊恐': 'jingkong', '惊讶': 'jingya', '愉快': 'yukuai', '憨笑': 'hanxiao', '抓狂': 'zhuakuang', '抠鼻': 'koubi', '抱拳': 'baoquan', '拥抱': 'yongbao', '拳头': 'quantou', '捂脸': 'wulian', '握手': 'woshou', '撇嘴': 'piezui', '擦汗': 'cahan', '敲打': 'qiaoda', '晕': 'yun', '月亮': 'yueliang', '机智': 'jizhi', '流汗': 'liuhan', '流泪': 'liulei', '炸弹': 'zhadan', '爱心': 'aixin', '猪头': 'zhutou', '玫瑰': 'meigui', '疑问': 'yiwen', '白眼': 'baiyan', '皱眉': 'zhoumei', '睡': 'shui', '礼物': 'liwu', '笑哭': 'xiaoku', '红包': 'hongbao', '耶': 'ye', '胜利': 'shengli', '色': 'se', '菜刀': 'caidao', '蛋糕': 'dangao', '衰': 'shuai', '西瓜': 'xigua', '调皮': 'diaopi', '跳跳': 'tiaotiao', '转圈': 'zhuanquan', '鄙视': 'bishi', '闭嘴': 'bizui', '阴险': 'yinxian', '难过': 'nanguo', '骷髅': 'kulou', '高兴': 'gaoxing', '鬼脸': 'guilian', '鸡': 'ji', '鼓掌': 'guzhang'},
      }
    },
    created() {
    },
    methods: {
      choose(emoji) {
        if(this.isIOS()){
          this.content = this.$refs.comment.innerHTML + `[${this.map1[emoji]}]`
        }
        else{
          this.content = this.$refs.comment.innerHTML + `<i class="emoji emoji-${emoji}" contenteditable="false"></i>`;
        }
      },
      input() {
        this.close();
        setTimeout(()=>{
          this.keepLastIndex(this.$refs.comment);
        }, 50);
      },
      submit() {
        let content = this.$refs.comment.innerHTML.replace(/contenteditable="false"/g, "").trim();
        if(content.length > 0){
          content = content.replace(/\[([^\[\]]*)\]/g, (match)=>{
            let emoji = this.map2[match.slice(1, -1)];
            if(emoji === undefined){
              return match
            }
            return `<i class="emoji emoji-${emoji}"></i>`
          })
        }
        this.$emit("submit", content);
        this.$refs.comment.innerHTML = "";
        this.close();
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
      },
      open() {
        this.show = true;
        this.$nextTick(() => {
          window.scrollTo(window.scrollX, window.scrollY + 1);
        });
      },
      close() {
        this.show = false;
        this.$nextTick(() => {
          window.scrollTo(window.scrollX, window.scrollY + 1);
        });
      },
      isIOS() {
        return !!window.navigator.userAgent.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
      }
    }
  }
</script>

<style scoped lang="less">
  .comment-input-wrapper {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    width: 100%;
    background-color: #F7F8FC;
    z-index: 11;

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
          user-select: auto;
          -webkit-user-select: auto;
          outline: none;

          *{
            user-select: auto;
            -webkit-user-select:auto;
          }
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
    user-select: none;
    -webkit-user-select: none;
    -webkit-overflow-scrolling:touch;

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

  .emotion-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    z-index: 10;
  }
</style>
