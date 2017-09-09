<template lang="html">
    <div class="food">
        <div class="back" @click="close">
            <
        </div>
        <div class="pic">
            <img :src="food.image" alt="">
        </div>
        <div class="info">
            <h1>{{food.name}}</h1>
            <p class="sale">月售份{{food.sellCount}} 好评率{{food.rating}}</p>
            <p class="price">
                <strong><span>￥</span>{{food.price}}</strong>
                <del v-if="food.oldPrice">{{food.oldPrice}}</del>
            </p>
        </div>
        <div class="line"></div>
        <div class="intro">
            <h2>商品介绍</h2>
            <p>{{food.info}}</p>
        </div>
        <div class="line"></div>
        <div class="ratings">
            <h2>商品评价</h2>
            <p>{{food.info}}</p>
        </div>
    </div>
</template>

<script>
export default {
    props : {
        food : {
            type : Object,
            required : true
        }
    },
    methods : {
        close(){
            this.$emit('close');
        }
    }
}
</script>

<style lang="less" scoped>
    .food {
        position: fixed;
        left : 0;
        right: 0;
        top: 0;
        bottom: 48px;
        background: #fff;
        .back{
            position: absolute;
            left : 20px;
            top : 15px;
            font-size:32px;
            color: #f00;
        }
        .pic{
            height: 375px;
            img {
                width: 100%;
                height: 375px;
            }
        }
        .info,.intro,.ratings{
            margin:18px;
        }
        .info {
            h1 {
                font-size: 16px;
                font-weight: bold;
                color: rgb(7,17,27);
            }
            .sale {
                font-size: 10px;
                color: rgb(147,153,159);
                margin: 8px 0;
            }
            .price {
                strong {
                    color: rgb(240,20,20);
                    font-size: 14px;
                    font-weight: 700;
                    span {
                        font-size: 10px;
                        font-weight: normal;
                    }
                }
                del {
                    font-size: 10px;
                    color: rgb(147,153,159);
                }
            }
        }
        .line {
            height: 16px;
            background: #f3f5f7;
            border: 1px solid rgba(7,17,27,.1);
            border-left: none;
            border-right: none;
        }
        .intro,.ratings {
            h2 {
                color: rgb(7,17,27);
                font-weight: 700;
                font-size: 12px;
                margin-bottom: 10px;
            }
        }
        .intro {
            p {
                font-size: 12px;
                line-height: 24px;
                color: rgb(77,85,93);
            }
        }
    }
</style>
