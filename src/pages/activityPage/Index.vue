<template>
    <div id="app">
        <img src="./img/2.jpg" alt="">
        <div class="title">
            <img src="./img/long.png" alt="">
        </div>
        <div class="top">
            <div class="goods">
                <h3>本次拼团商品</h3>
                <div class="good">
                    <div class="goodsName">
                        <h3>{{goodsInfo.name || '无'}}</h3>
                        <span>{{activity.minCount || 0  }}人可成团</span>
                    </div>
                    <div class="goodsPrice">
                        <span>拼团价{{goodsInfo.sellPrice || 0   | Money}}</span>
                        <em>省{{goodsInfo.saveMoney || 0   | Money}}</em>
                        <strong>价格 <span>{{goodsInfo.originPrice || 0   | Money}}</span></strong>
                        <strong class="countNum">已有{{orderCount || 0  }}人成团</strong>
                    </div>
                </div>
            </div>
            <div class="info">
                <div class="myInfo">
                    <img src="./img/2.jpg" alt="">
                    <h3>张小黑的团还差1人</h3>
                </div>
                <span>快速参团</span>
            </div>
            <div class="rules">
                <h3>活动规则</h3>
                <span>1. 一键开团成为团长，享受折扣优惠；</span>
                <span>2.邀请好友拼团享受折扣优惠，成团后你获得推广返佣</span>
                <span>3.推广返佣与购买人数正相关，每1人购买，推广返佣增加￥338.3</span>
                <span>4.拼团结束48小时后，推广返佣会划入到账户余额，支持提现</span>
                <span>5.工作人员会在每月1号处理上个月的提现申请，1-3天内到账</span>
            </div>
            <div class="goodDetails" @click="goGroupBuy">
                <h3>商品详情</h3>
                <img :src="goodsInfo.imageUrl" alt="" >
            </div>
        </div>
        <div class="start" @click="showOpen = true">
           我要开团
        </div>
        <div class="end" >
            <h3>本次活动已结束</h3>
        </div>
        <div class="income" @click="goIncome">
            <img src="./img/2.png" alt="">
        </div>
        <Popup v-model="showOpen" :close-on-click-overlay="false">
            <div class="wrap">
                <h3>开团方式</h3>
                <Button class="indexBtn" @click="goCheckMobile">A：购买商品，并成为团长</Button>
                <Button class="indexBtn">B：通过邀请好友成为团长</Button>
                <Button class="bottomBtn" @click="showOpen = false">取消</Button>
            </div>
        </Popup>
        <Popup v-model="showMobile" :close-on-click-overlay="false">
            <div class="wrapMobile">
                <h3>填写手机号</h3>
                <input type="number" placeholder="用于获取课程卡号/密码，请务必确认">
                <Button class="bottomBtn" @click="showMobile = false">去支付</Button>
            </div>
        </Popup>
    </div>
</template>

<script>
    import CommonMixin from '../commonMixin.js'
    import {leaderActivity} from "../../api/activity";
    import Config from '../../config/app'
    import {Popup,Button} from 'vant';
    export default {
        name: 'app',
        mixins: [CommonMixin],
        data: function () {
            return {
                showOpen:false,
                showMobile:false,
                mobile:'',    // 用于支付前填写的手机号
                orderCount:'',// 成功购买人数
                leaderHasBuy:'',
                goodsInfo: {},
                activity:{}
            }
        },
        filters:{
            Money:function(value){
                return '￥'+ parseInt(value)/100
            }
        },
        methods: {
            goGroupBuy(){
                window.location.href = './groupBuy.html'
            },
            goIncome(){
                window.location.href = './incomeDetails.html'
            },
            goCheckMobile(){
                this.showOpen = false;
                this.showMobile = true;
            }
        },
        mounted() {
            leaderActivity({activityId:Config.activityId}).then(r=>{
                this.orderCount = r.orderCount;
                this.leaderHasBuy = r.leaderHasBuy;
                this.goodsInfo = {...r.goodsInfo};
                this.activity = {...r.activity}
            }).catch(_=>{})
        },
        beforeDestroy: function () {

        },
        components: {Popup,Button}
    }
</script>
<style>

</style>