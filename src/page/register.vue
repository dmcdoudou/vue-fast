<template>
    <section class="register">
        <h3 class="title">注册</h3>
        <div class="form">
            <mt-field label="用户名：" placeholder="请输入用户名" v-model.trim="form.userName" @input="$v.form.userName.$touch()"></mt-field>
            <mt-field label="密码：" placeholder="请输入密码" type="password" v-model.trim="form.password" @input="$v.form.password.$touch()"></mt-field>
            <mt-field label="确认密码：" placeholder="请输入密码" type="password" v-model.trim="form.confirm_pwd" @input="$v.form.confirm_pwd.$touch()"></mt-field>
        </div>
        <mt-button class="register_btn" type="primary" size="large" @click="onRegister">注册</mt-button>
        <router-link tag="div" class="to_login" to="/">已有账号？立马登陆</router-link>
    </section>
</template>

<script>
    import { required, minLength,sameAs } from 'vuelidate/lib/validators'
    import API from '../API/axios'
    export default {
        name: '',
        data () {
            return {
                form: {
                    userName: 'fwlst',
                    password: 'fwlst520/',
                    confirm_pwd: 'fwlst520/'
                }
            }
        },
        validations: {
            form: {
                userName: {
                    required,
                    minLength: minLength(5)
                },
                password: {
                    required,
                    minLength: minLength(8)
                },
                confirm_pwd: {
                    sameAsPassword: sameAs('password')
                }
            }
        },
        methods: {
            onRegister(){
                let form = this.$v.form
                if(!form.userName.required){
                    this.$toast('用户名不能为空');
                }else if(!form.userName.minLength){
                    this.$toast('用户名不能少于' + form.userName.$params.minLength.min + '个字符');
                }else if(!form.password.required){
                    this.$toast('密码不能为空');
                }else if(!form.password.minLength){
                    this.$toast('用户名不能少于' + form.password.$params.minLength.min + '个字符');
                }else if(!form.confirm_pwd.sameAsPassword){
                    this.$toast('两个密码不一致');
                }else {
                    API.userRegister(this.form).then(({data})=>{
                        if(data.code == 200){
                            this.$router.push({
                                name: 'Login'
                            });
                        }else {
                            this.$toast(data.msg);
                        }
                    })
                }
            }
        },
        mounted () { // 代替ready

        }

    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style rel="stylesheet/less" lang="less" scoped>
    .register{
        .title{
            font-size: 30px;
            color: #bd00ff;
            text-align: center;
            margin: 30px 0;
        }
        .form{
            margin-bottom: 30px;
        }
        .register_btn{
            width: 90%;
            margin: 0 auto;
            border-radius: 20px;
            background: #bd00ff;
        }
        .to_login{
            font-size: 12px;
            text-align: center;
            margin-top: 30px;
            color: #999;
        }
    }
</style>
