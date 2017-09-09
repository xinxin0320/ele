<template lang="html">
    <header>
        <!-- 商家的基本信息  -->
        <div class="seller">
            <img :src="seller.avatar" alt="">
            <div class="seller-info">
                <h2><i></i>{{seller.name}}</h2>
                <p class="delivery">{{seller.description}}/{{seller.deliveryTime}}分钟到达</p>
                <p class="pay">{{seller.supports[0].description}}</p>
            </div>
            <div class="btn" @click="show=true">5个 ></div>
        </div>
        <!-- 公告 -->
        <div class="bulletin" @click="show=true">
            <i></i>
            <p>{{seller.bulletin}}</p>
            <span>></span>
        </div>
        <!-- 背景层 -->
        <div class="bg">
            <img :src="seller.avatar" alt="">
        </div>
        <!-- 弹出层 -->
        <transition name="bounce">
            <ele-popup :seller="seller" v-show="show" @close="show=false"></ele-popup>
        </transition>
    </header>
</template>

<script>
import Popup from '../Popup/Popup'

export default {
    data(){
        return {
            show : false //默认隐藏弹出层
        }
    },
    props : {
        seller : {
            type : Object,
            required : true
        }
    },
    components : {
        'ele-popup' : Popup
    }
}
</script>

<style lang="less" scoped>
    @keyframes bounce-in {
        0%{transform: scale(0);}
        50%{transform: scale(1.5);}
        100%{transform: scale(1);}
    }
    @keyframes bounce-out {
        0%{transform: scale(1);}
        50%{transform: scale(1.5);}
        100%{transform: scale(0);}
    }
    .bounce-enter-active{animation: bounce-in 0.5s;}
    .bounce-leave-active{animation: bounce-out 0.5s;}
    header {
        position: relative;
        width: 100%;
        height: 134px;
        overflow: hidden;
        background: rgba(7,17,27,0.5);
        color: #fff;
        .seller {
            margin: 24px 12px 18px 24px;
            display: flex;
            img {
                width: 64px;
                height: 64px;
                border-radius: 4px;
            }
            .seller-info{
                margin-left: 16px;
                flex-grow: 1;
                display: flex;
                flex-direction: column;
                justify-content : space-between;
                h2 {
                    font-size: 16px;
                    font-weight: 700;
                    line-height: 18px;
                    i {
                        width: 30px;
                        height: 18px;
                        display: inline-block;
                        background: url('./brand@2x.png') no-repeat;
                        background-size: contain;
                        vertical-align: middle;
                        margin-right: 5px;
                    }
                }
                .delivery {
                    font-size: 12px;
                    font-weight: 200;
                }
                .pay {
                    font-size: 10px;
                    font-weight: 200;
                }
            }
            .btn {
                align-self: flex-end;
                font-size: 10px;
                font-weight: 200;
                background: rgba(0,0,0,.2);
                padding: 10px 15px;
                border-radius: 7px 8px;
            }
        }
        .bg{
            position: absolute;
            width: 100%;
            height: 100%;
            left:0;
            top: 0;
            z-index: -1;
            filter: blur(5px);
            img {
                width: 100%;
            }
        }
        .bulletin{
            height: 28px;
            line-height: 28px;
            padding:0 12px;
            font-size: 10px;
            font-weight: 200;
            display: flex;
            align-items: center;
            background: rgba(7,17,27,.2);
            p {
                white-space: nowrap;
                overflow: hidden;
                text-overflow: ellipsis;
            }
            i {
                min-width: 22px;
                min-height: 12px;
                background: url('./bulletin@2x.png') 0 0 no-repeat;
                background-size: contain;
                display: block;
                margin-right: 5px;
            }
        }
    }
</style>
