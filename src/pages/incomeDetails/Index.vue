<template>
    <div id="app">
        <div class="money">
            <span>购买人数</span>
            <span>{{income.rewardCount || 0  }}人</span>
        </div>
        <div class="money">
            <span>账户余额</span>
            <span>{{income.surplusMoney || 0  |Money}}</span>
        </div>
        <div class="money">
            <span>申请中金额</span>
            <span>{{income.applyingMoney || 0  |Money}}</span>
        </div>
        <div class="money">
            <span>已提现金额</span>
            <span>{{income.withdrawMoney || 0  |Money}}</span>
        </div>
        <div class="money">
            <span>提现金额</span>
            <input type="number" placeholder="不能超过账户余额" @input="checkMoney" v-model="getMoney">
            <em @click="rulesShow = true"><img src="./img/green.png" alt=""></em>
        </div>
        <Button class="indexBtn">提现</Button>
        <div class="money" @click="goDetails">
            <span>收支明细</span>
            <img src="./img/right.png" alt="">
        </div>
        <div class="money" @click="goRecords">
            <span>提现记录</span>
            <img src="./img/right.png" alt="">
        </div>
        <Popup v-model="rulesShow" :close-on-click-overlay="false">
            <div class="ruleBox">
                <h4>提现规则</h4>
                <span>1.每月1号结算上个月的提现申请，1-5个工作日到账</span>
                <span>2.一个自然月内累计申请提现金额≧800时，需按照国家税务规定缴纳相关税费</span>
                <span>3.如有任何疑问，请拨打客服电话400-8650-512</span>
                <span class="botSpan">最终解释权归护士加所有</span>
                <Button class="ruleBtn" @click="rulesShow = false">知道了</Button>
            </div>
        </Popup>
    </div>
</template>

<script>
    import {Button,Popup} from 'vant'
    import CommonMixin from '../commonMixin.js'
    import {activityReward} from "../../api/activity";
    import Config from '../../config/app'
    export default {
        name: 'app',
        mixins: [CommonMixin],
        data: function () {
            return {
                rulesShow:false,
                income:{},
                getMoney:''
            }
        },
        filters:{
            Money:function(value){
                return '￥'+ parseInt(value)/100
            }
        },
        methods: {
            checkMoney(){
                if(this.getMoney > this.income.surplusMoney){
                    this.getMoney = this.income.surplusMoney
                }
            },
            goDetails(){
                window.location.href = './withdrawDetails.html'
            },
            goRecords(){
                window.location.href = './withdrawRecords.html'
            }
        },
        mounted() {
            activityReward({activityId:Config.activityId}).then(r=>{
                this.income = {...r}
            }).catch(_=>{})
        },
        beforeDestroy: function () {

        },
        components: {Button,Popup}
    }
</script>
<style lang="less" scoped>
#app{
    padding: 10px;
    .money{
        width: 100%;
        height: 50px;
        padding: 0 15px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        input{
            width: 65%;
            height: 35px;
            background:rgba(252,252,252,1);
            border:1px solid rgba(238, 238, 238, 1);
            border-radius:3px;
            margin: 0 15px;
            padding-left: 13px;
            &::placeholder{
                color: #999;
                font-size: 12px;
            }
        }
        em{
            img{
                width: 14px;
                height: 14px;
            }
        }
        img{
            width: 14px;
            height: 13px;
        }
    }
    .indexBtn{
        width:100%;
        height:50px;
        background:linear-gradient(0deg,rgba(255,64,0,1),rgba(255,119,0,1));
        border-radius: 3px ;
        color: #fff;
        margin: 16px 0;
        border:0;
    }
    .ruleBox{
        width:320px;
        height:363px;
        background:rgba(255,255,255,1);
        border-radius:3px;
        display: flex;
        justify-content: flex-start;
        align-items: center;
        flex-direction: column;
        padding: 0 19px;
        h3{
            font-weight: normal;
            color: #333333;
            font-size: 19px;
            margin: 23px 0 33px 0;
        }
        span{
            color: #333333;
            font-size: 15px;
            line-height: 30px;
        }
        .botSpan{
            width: 100%;
            display: block;
            text-align: left;
        }
        .ruleBtn{
            width:100%;
            height:40px;
            background:linear-gradient(0deg,rgba(255,64,0,1),rgba(255,119,0,1));
            border-radius: 19px ;
            color: #fff;
            margin: 28px 0;
            border:0;
        }
    }
}
</style>