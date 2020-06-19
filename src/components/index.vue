<template>
  <div class="container" >
    <div class='emoji'>
      <textarea class="text" rows="5" v-model="content" ></textarea>
      <div class='addEmoji'>添加表情</div>
      <div class='emojiDiv'>
        <emotion @emotion="handleEmotion" :height="200" ></emotion>
      </div>
      <div><button @click="saveHandler">发布评论</button></div>
    </div>

    <div v-for='item in comments' :key="item.id">
      <p v-html="item.content.replace(/\#[\u4E00-\u9FA5]{1,3}\;/gi, emotion)"></p>
    </div>


    <!-- <p>文本输入框</p>
    <textarea class="text" rows="5" v-model="content" ></textarea>
    <p>表情选择框</p>
    <emotion @emotion="handleEmotion" :height="200" ></emotion>
    <p>效果显示框</p>
    <div class="text-place">
      <p v-html="content.replace(/\#[\u4E00-\u9FA5]{1,3}\;/gi, emotion)"></p>
    </div> -->
  </div>
</template>

<script>
import Emotion from '@/components/Emotion/index'
import {post,get} from '@/http/axios'
export default {
  data () {
    return {
      comment_form:{
        orderId:1145,
        cusId:134
      },
      content:'',
      comments:[]
    }
  },
  created(){
    this.findComment()
  },
  methods: {
    // 查询评论
    findComment(){
      get('/comment/findCommentByCus?cusId=134').then((res)=>{
        this.comments = res.data.list
      })
    },
    // 保存评论
    saveHandler(){
      this.comment_form.content = this.content
      post('/comment/saveOrUpdate',this.comment_form).then((res)=>{
        // 更新数据
        this.findComment()
        this.content = ''
      })
    },
    handleEmotion (i) {
      this.content += i
    },
    // 将匹配结果替换表情图片
    emotion (res) {
      let word = res.replace(/\#|\;/gi,'')
      const list = ['微笑', '撇嘴', '色', '发呆', '得意', '流泪', '害羞', '闭嘴', '睡', '大哭', '尴尬', '发怒', '调皮', '呲牙', '惊讶', '难过', '酷', '冷汗', '抓狂', '吐', '偷笑', '可爱', '白眼', '傲慢', '饥饿', '困', '惊恐', '流汗', '憨笑', '大兵', '奋斗', '咒骂', '疑问', '嘘', '晕', '折磨', '衰', '骷髅', '敲打', '再见', '擦汗', '抠鼻', '鼓掌', '糗大了', '坏笑', '左哼哼', '右哼哼', '哈欠', '鄙视', '委屈', '快哭了', '阴险', '亲亲', '吓', '可怜', '菜刀', '西瓜', '啤酒', '篮球', '乒乓', '咖啡', '饭', '猪头', '玫瑰', '凋谢', '示爱', '爱心', '心碎', '蛋糕', '闪电', '炸弹', '刀', '足球', '瓢虫', '便便', '月亮', '太阳', '礼物', '拥抱', '强', '弱', '握手', '胜利', '抱拳', '勾引', '拳头', '差劲', '爱你', 'NO', 'OK', '爱情', '飞吻', '跳跳', '发抖', '怄火', '转圈', '磕头', '回头', '跳绳', '挥手', '激动', '街舞', '献吻', '左太极', '右太极']
      let index = list.indexOf(word)
      return `<img src="https://res.wx.qq.com/mpres/htmledition/images/icon/emotion/${index}.gif" align="middle">`   
    }
  },
  components: {
    Emotion
  }
}
</script>
<style scoped>
  .emoji div{
    display: inline-block
  }
  .emoji div:last-child{
    float: right;
  }
  .emoji {
    position: relative;
  }
  .container >>> .emojiDiv{
    position:absolute;
    left:0;
    display: none;
  }
  .addEmoji:hover ~ .emojiDiv{
    display:block;
  }
  .emojiDiv:hover{
    display:block;
  }


  .container {
    margin: 0 auto;
    margin-top: 20px;
    width: 400px;
  }
  .text {
    display: block;
    margin: 0 auto;
    margin-bottom: 10px;
    width: 400px;
    resize: none;
    box-sizing: border-box;
    padding: 5px 10px;
    border-radius: 8px;
  }
  .text-place {
    height: 80px;
    box-sizing: border-box;
    padding: 5px 10px;
    border-radius: 8px;
    background: #ddd5d5;
  }
  .text-place p {
    display: flex;
    align-items: center;
    margin: 0;
  }
</style>