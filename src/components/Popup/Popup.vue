<template lang="html">
    <div class="popup">
        <h1>{{seller.name}}</h1>
        <div class="discount">
            <h2>优惠信息</h2>
            <ul>
                <li v-for="(item,index) in seller.supports" :key="index">
                    {{item.description}}
                </li>
            </ul>
        </div>
        <div class="bulletin">
            <h2>商家公告</h2>
            <p>{{seller.bulletin}}</p>
        </div>
        <!-- 关闭按钮 -->
        <div class="close" @click="close">x</div>
    </div>
</template>

<script>
export default {
    methods:{
        close(){
            //向父组件emit事件
            this.$emit('close');
        }
    },
    props : {
        seller : {
            type : Object,
            required : true
        }
    }
}
</script>

<style lang="less" scoped>
    .popup{
    z-index:999;
        position: fixed;
        left : 0;
        right: 0;
        top: 0;
        bottom: 0;
        background: rgba(7,17,27,.8);
        color: #fff;
        h1 {
            margin-top: 64px;
            text-align: center;
            font-size: 16px;
            font-weight: 700;

        }
        .discount,.bulletin {
            margin: 20px 36px;
            font-size: 12px;
            font-weight: 200;
            h2 {
                text-align: center;
                line-height: 50px;
                font-size: 14px;
                font-weight: 700;
            }
        }
        .discount {
            li {
                padding: 6px 0;
            }
        }
        .bulletin {
            p {
                line-height: 24px;
            }
        }
        .close {
            font-size: 32px;
            color: rgba(255,255,255,.5);
            position: absolute;
            bottom: 32px;
            left : 50%;
            transform: translateX(-50%);
        }
    }
</style>
