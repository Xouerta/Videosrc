<template>
  <div>
      <login-top Text="注册VideoSrc">
          <div slot="right" style="font-size:3.611vw" @click="$router.push('/login')">用户登录</div>
      </login-top>
        <login-text label="姓名" 
            style="margin:15.001px 0;" 
            placeholder="请输入姓名"

            @contentWatch="res => model.name = res"
            >
        </login-text>

        <login-text label="账号" 
        placeholder="请输入账号"
         @contentWatch="res => model.username = res"
         >
        </login-text>

        <login-text label="密码" 
            placeholder="请输入密码" 
            type="password"

            @contentWatch="res => model.password = res"
        >
        </login-text>
    <login-text label="邮箱"
                placeholder="请输邮箱"
                type="text"
                v-model="from.email"
    >
    </login-text>
    <login-text label="验证码"
                placeholder="请输入验证码"
                type="text"
                v-model="from.code"
    >
    </login-text>

        <login-btn BtnText="注册" @TextClick="AjaxInsert"></login-btn>
        <button type="primary" @click="getCode">获取验证码</button>

  </div>
</template>

<script>
import LoginTop from '@/components/common/LoginTop.vue'
import LoginText from '@/components/common/LoginText.vue'
import LoginBtn from '@/components/common/LoginBtn.vue'
export default {
    data() {
        return {
            model:{},
          form:{
              email:"",
            code:""
          }
        }
    },
    components:{
        LoginTop,
        LoginText,
        LoginBtn
    },
    methods:{
        async AjaxInsert() {
            let rulg = /^.{6,16}/
            if(rulg.test(this.model.name) && rulg.test(this.model.username)&& rulg.test(this.model.password)){
                const res =  await this.$http.post('/user/register',this.model)
                this.$msg.fail(res.data.msg)
                localStorage.setItem('token',res.data.objtoken) 
                localStorage.setItem('id',res.data.id) 
                setTimeout(() => {
                    this.$router.push('/userinfo')
                },2000)
            }else{
                this.$msg.fail('格式不正确,请重新输入!')
            }
        },
        getCode(){
          axios.get("url",{
            params:{
              email:this.form.email
            }
          })
              .then(()=>{
                this.$message({
                  message:"send code",
                  type:"success"
                });
              })
              .catch(()=>{
                this.message({
                  message:"request timeout!",
                  type:"error"
                })
              })
        },
      submit() {
        const data = {
          email: this.email,
          code: this.code
        };
        axios
            .post("url", data)
            .then(res => {
              if ((res.data.code = '')) {
                this.$message({
                  message: "验证码错误",
                  type: "error"
                });
                return;
              } else {
                this.$router.push({ path: "/Login" });
                this.$message({
                  message: "验证通过,注册成功",
                  type: "success"
                });
              }
            })
            .catch(() => {
              this.$message({
                message: "请求超时，请检查网络连接",
                type: "error"
              });
            });
      },

    }
}
</script>

<style lang="less">

</style>