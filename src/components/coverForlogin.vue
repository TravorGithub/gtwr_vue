<template>
  <div class="cover-login">
    <div class="box-container">
      <div class="main">
        <hr color="lightgrey"/>
        <header class="login-title"><b>欢迎登录</b></header>
        <div class="login">
            <input type="text" placeholder="输入用户名..."><br/>
            <input type="password" placeholder="输入密码"><br/>
            <button @click="submitPar" ><span>登录</span></button>
            <div>
              <a style="float:right">忘记密码？</a>
              <a @click="sign"  style="float:left">注册</a>
            </div>
        </div>  
      </div>
    </div>
    <img src="../assets/img/close.png" height="20" width="20" @click="submitPar">
  </div>
</template>

<script>
require('../style/coverLogin.scss')
import {mapState} from 'vuex'

export default {
	  name: 'coverLogin',
  data () {
    return {
      visible:false
    }
  },
  mounted(){
    //this.$router.push({path:'/login/loginIn'});
  },
  computed:{

  },
  methods:{
      submitPar:function(){
        //关闭遮罩层与登录面板
        this.$store.dispatch('LoginShowAction',false);
    },
    sign:function(){
      //关闭遮罩层与登录面板
      this.$store.dispatch('LoginShowAction',false);
      //打开注册页面
      this.$store.dispatch('SignShowAction',true);

    }
  },
  components:{

      },
      
}
</script>
