<template>
  <div class="cover-sign">
    <div class="box-container">
      <div class="main">
        <hr color="lightgrey"/>
        <header class="sign-title"><b>新用户注册</b></header>
        <div class="sign">
            <input type="text" placeholder="请输入手机号"  class="register_content_input" v-model= "LUserPhone" @blur="checkLPhone"><br/>
            <input type="password" placeholder="请输入密码" class="register_content_input" v-model="LUserPsd" @blur="checkLPsd"><br>
           <div class="join_formitem">
                <div class = 'captcha'>
                    <input type="text"  placeholder="请输入验证码" class="yanzhengma_input" v-model="picLyanzhengma" />
                    <input type="button"  @click="createCode"  class="verification"  v-model="checkCode"/>
                </div>
            </div>
         <div  class="send_code">
              <input class="auth_input" type="text" v-model="verification"  placeholder="短信验证码" />
              <button @click="send"    class="send_msg" >
              <span v-if="sendMsgDisabled">{{time+'秒后获取'}}</span>
              <span v-if="!sendMsgDisabled">获取验证码</span></button>
          </div>
            <button @click="getview"    class="regist_button"><span>注册</span></button> 
        </div> 
      </div>
    </div>
    <img src="../assets/img/close.png" height="20" width="20" @click="submitPar">
  </div>
</template>
<script>
var code="";
require('../style/coverForSign.scss')
import {mapState} from 'vuex'
export default {
  name: 'coverForSign',
  data () {
    return {
        from:'system',
        coupon:{},
        id:'',
        name:'',
        address:'',
        message:'',
        userPhone:'',
        dialog: false,
        LUserPhone:'',
        LUserPsd:'',
        picLyanzhengma:'',
        checkCode:'', 
        time: 60, // 发送验证码倒计时
        sendMsgDisabled: false  
    }
  },
  mounted(){
    if (this.$route.query.from){
    this.form = this.$route.query.from
    }
    if (this.$route.query.id){
    this.form = this.$route.query.id
    }
    if (this.$route.query.from === 'system')
    {
    this.initCouponInfo()
    }
  },
  created()
  {
    this.createCode();
  },
  computed:{
    ...mapState({
        // ...
      
    })
  },
  methods:{
    submitPar:function(){
      //关闭遮罩层与登录面板
      this.$store.dispatch('SignShowAction',false);
    },
    send:function() {
      let me = this;
      me.sendMsgDisabled = true;
      let interval = window.setInterval(function() {
       if ((me.time--) <= 0) {
        me.time = 60;
        me.sendMsgDisabled = false;
        window.clearInterval(interval);
       }
      }, 1000);
     },
      checkUserPhone:function(){
        if(this.userPhone == ''){
          console.log(111)
          $(".hiddenTanchuang").removeClass('hiddenTanchuang')
        }
      },
      hiddenTanchuang(){
        $(".tanchuang").addClass("hiddenTanchuang")
      },
        
      // 验证登陆手机号格式
      checkLPhone:function(){
          if(this.LUserPhone == ''){
              $(".login_content1 span:eq(0)").removeClass("disappear");
              $(".login_content1 span:eq(0)").text("请输入手机号。")

          }else if(this.LUserPhone.search(/^(0|86|17951)?(13[0-9]|15[012356789]|17[678]|18[0-9]|14[57])[0-9]{8}$/)==0){
              $(".login_content1 span:eq(0)").addClass("disappear")
              return true;

          }else{
              $(".login_content1 span:eq(0)").removeClass("disappear");
              $(".login_content1 span:eq(0)").text("请输入正确手机号。")
          }
      },
      //验证登陆密码格式
      checkLPsd:function(){
          if(this.LUserPsd == ''){
              $(".login_content1 span:eq(1)").text("请输入密码");
              $(".login_content1 span:eq(1)").removeClass("disappear")

          }else if(this.LUserPsd.search(/^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]{6,20}$/) == 0){
              $(".login_content1 span:eq(1)").addClass("disappear")
              return true;
          }else{
              $(".login_content1 span:eq(1)").removeClass("disappear");
              $(".login_content1 span:eq(1)").text("密码必须6-20位，包含字母与数字")
          }
      },
      // 图片验证码
      createCode(){
                 //先清空验证码的输入
                 this.code = "";
                 this.checkCode = "";
                 this.picLyanzhengma = "";
                 //验证码的长度  
                    var codeLength = 4; 
                    //随机数 
          var random = new Array(0,1,2,3,4,5,6,7,8,9,'A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T','U','V','W','X','Y','Z');  
          for(var i = 0; i < codeLength; i++) {
                        //取得随机数的索引（0~35）
                            var index = Math.floor(Math.random()*36);   
                            //根据索引取得随机数加到code上
              this.code += random[index];   
                    }
                    //把code值赋给验证码  
          this.checkCode = this.code; 
      },  
      getview:function(){
        this.$store.dispatch('SignShowAction',false);
        this.$router.push({path:"/home/dataview"});

      },
    }
}
</script>
