<template>
    <div id="app">
        <div class="main">
            <h2>您好，请登录！</h2>
            <div class="phone">
                <vantIcon name="phone" class="icon"></vantIcon>
                <input type="number"  class="tel" placeholder="请输入手机号码" v-model="mobile">
            </div>
            <vantButton class="indexBtn" @click="checkCode">下一步</vantButton>
        </div>
    </div>
</template>

<script>
    import {Button as vantButton,Toast} from 'vant';
    import {Icon as vantIcon} from 'vant';
    import CommonMixin from '../commonMixin.js'
    import {sendSmsCode} from '../../api/recommender'
    import Config from '../../config/app'
    export default {
        name: 'app',
        mixins: [CommonMixin],
        data: function () {
            return {
                mobile:''
            }
        },
        methods: {
            checkCode(){
                if(this.mobile.length!= 11){
                    Toast('请输入正确的手机号');
                    return;
                }
                sendSmsCode({mobile:this.mobile,activityId: Config.activityId}).then(r=>{
                    window.location.href = './login.html?mobile='+this.mobile
                }).catch(_=>{});
            }
        },
        mounted() {
        },
        beforeDestroy: function () {

        },
        components: {vantButton,vantIcon}
    }
</script>
<style lang="less" scoped>
    #app{
        background: #fff;
    }
    .main{
        display: flex;
        justify-content: center;
        align-items: flex-start;
        flex-direction: column;
        padding: 0 15px;
        h2{
            font-size: 30px;
            color: #262626;
            margin: 50px 0;
            font-weight: normal;
            padding-left: 10px;
        }
        .phone{
            width: 100%;
            height: 50px;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 0 5px;
            border-bottom: 1px solid rgba(220,220,220,1);
            input{
                height: 50%;
                flex: 1;
                border:0;
                font-size: 19px;
                color: #222;
                margin-left: 15px;
                width: 45%;
                &::placeholder{
                    font-size: 16px;
                    color: #999;
                }
            }
            .get-msg{
                display: flex;
                justify-content: center;
                align-items: center;
                .line{
                    width:1px;
                    height:29px;
                    background:rgba(220,220,220,1);
                    margin-right: 13px;
                }
            }
        }
        .indexBtn{
            width:100%;
            height:40px;
            background:linear-gradient(0deg,rgba(255,64,0,1),rgba(255,119,0,1));
            border-radius: 19px;
            color: #fff;
            margin: 30px 0;
            border:0;
        }
    }
</style>

