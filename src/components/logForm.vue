<template>
  <div class="login-form">
    <div class="g-form">
      <div class="g-form-line">
        <span class="g-form-label">用户名：</span>
        <div class="g-form-input">
          <input type="text" v-model="usernameModel" placeholder="输入用户名">
        </div>
        <span class="g-form-error">{{userErrors.errorText}}</span>
      </div>
      <div class="g-form-line">
        <span class="g-form-label">密码：</span>
        <div class="g-form-input">
          <input type="text" v-model="passwordModel" placeholder="输入密码">
        </div>
        <span class="g-form-error">{{passwordErrors.errorText}}</span>
      </div>
      <div class="g-form-line">
        <div class="g-form-btn">
          <a class="button" @click="onLogin">登录</a>
        </div>
      </div>
      <p>{{errorText}}</p>
    </div>
  </div>
</template>
<script>
export default {
  data(){
      return{
        usernameModel:'',
        passwordModel:'',
        errorText:''
      }
  },
  methods:{
    onLogin(){
      if(!this.userErrors.status||!this.passwordErrors.status){
        this.errorText='部分选项未通过'
      }else{
        this.errorText=''
        this.$http.get('api/login')
        .then((res)=>{
          this.$emit('has-log',res.data)
        },(error)=>{
          console.log(error)
        })
      }
    }
  },
  computed:{
    userErrors(){
      let errorText,status
      if(!/@/g.test(this.usernameModel)){
        status=false,
        errorText='不包含@'
      }else{
        status=true,
        errorText='包含@'
      }if(!this.usernameFlag){
        errorText=''
        this.usernameFlag=true
      }
      return{
        status,
        errorText
      }
    },
    passwordErrors(){
      let status,errorText
      if(!/^\w{1,6}$/g.test(this.passwordModel)){
        status=false,
        errorText='密码不是1-6位'
      }else{
        status=true,
        errorText=''
      }if(!this.paswordFlag){
        errorText=''
        this.paswordFlag=true
      }
      return{
        status,
        errorText
      }
    }
  }
}
</script>

