<template lang="html">
    <div>
        <div class="goods">
            <!-- 左边分类 -->
            <div class="goods-menu" id="goods-menu">
                <ul>
                    <li v-for="(item,index) in goods" :key="index" :class="{active:index===current}"
                    @click="selectCategory(index)"
                    >
                        <p>{{item.name}}</p>
                    </li>
                </ul>
            </div>
            <!-- 右边商品列表 -->
            <div class="goods-list" id="goods-list">
                <div ref="list">
                    <dl v-for="(item,index) in goods" :key="index">
                        <dt>{{item.name}}</dt>
                        <dd v-for="(food,idx) in item.foods" :key="idx" @click="selectFood(food)">
                            <img :src="food.icon" alt="">
                            <div class="food">
                                <h2>{{food.name}}</h2>
                                <p class="desc" v-if="food.description">{{food.description}}</p>
                                <p class="sale">月售份{{food.sellCount}} 好评率{{food.rating}}</p>
                                <p class="price">
                                    <strong><span>￥</span>{{food.price}}</strong>
                                    <del v-if="food.oldPrice">￥{{food.oldPrice}}</del>
                                </p>
                                <!-- 添加购物车组件  -->
                                <div class="ele-buy">
                                    <ele-buy :food="food"></ele-buy>
                                </div>
                            </div>
                        </dd>
                    </dl>
                </div>
            </div>
        </div>
        <!-- ele-food -->
        <transition name="slide">
            <ele-food :food="currentFood" v-show="show" @close="show=false"></ele-food>
        </transition>
        <!--购物车组件  -->
        <ele-cart :cartGoods="cartGoods"
                  :minPrice="seller.minPrice"
                  :deliveryPrice="seller.deliveryPrice">
        </ele-cart>
    </div>

</template>

<script>
const shop = require('../../api/data.json')
//引入iscroll插件
import IScroll from '../../../static/js/iscroll-probe'
import Food from '../Food/Food'
import Buy from '../Buy/Buy'
import Cart from '../Cart/Cart'

export default {
    data(){
        return {
            goods : [], //该商家所有的外卖商品
            current : 0, //表示当前所在的分类，默认为0，表示第一个分类
            listHeight : [], //表示右侧商品分类容器的高度
            show : false , //是否显示food组件，默认不显示
            currentFood : {}, //表示当前选中的外卖商品
            seller : {}, //表示商家的信息
        }
    },
    computed:{
        //获取添加到购物车中的商品
        cartGoods(){
            let cart = [];
            this.goods.forEach(item => {
                item.foods.forEach(food => {
                    if (food.count){
                        cart.push(food);
                    }
                });
            });
            return cart;
        }
    },
    methods:{
        //点击food，进入详情页面
        selectFood(food){
            //先设置show为true
            this.show = true;
            //设置food的值
            this.currentFood = food;
        },
        selectCategory(index){
            this.current = index; //修改current的值
            //获取所有的dl
            let dls = this.$refs.list.getElementsByTagName('dl');
            //调用scrollToElement方法
            this.listScroll.scrollToElement(dls[this.current]);
        },
        //获取商品分类容器的高度
        _getListHeight(){
            let arr = [0];
            //获取所有的dl
            let dls = this.$refs.list.getElementsByTagName('dl');
            for (let i = 0; i < dls.length; i++) {
                arr.push(dls[i].clientHeight + arr[i]);
            };
            return arr;
        }
    },
    created(){
        this.goods = shop.goods;
        this.seller = shop.seller;
    },
    mounted(){
        //组件挂载成功，dom已经形成了，此处可以获取dom元素
        this.menuScroll =  new IScroll("#goods-menu",{click : true});
        this.listScroll =  new IScroll("#goods-list",{probeType: 2,click : true});
        //调用_getListHeight获取商品列表容器的高度
        this.listHeight = this._getListHeight();
        console.log(this.listHeight);
        //给this.listScroll注册scroll事件
        let _this = this;
        this.listScroll.on('scroll',function(){
            let dis = Math.abs(this.y); //取绝对值
            console.log(dis);
            //需要根据dis的值和this.listHeight的值，来设置current的值
            for (let i = 0; i < _this.listHeight.length; i++) {
                let start = _this.listHeight[i];  //第一个值
                let end = _this.listHeight[i+1]; //第二个值
                if (!end || (dis >= start && dis < end) ) {
                    _this.current = i;
                    break;
                }
            }
        });
    },
    components : {
        'ele-food' : Food,
        'ele-buy' : Buy,
        'ele-cart' : Cart
    }
}
</script>

<style lang="less" scoped>
    .slide-enter,.slide-leave-active{transform: translateX(100%); opacity: 0;}
    .slide-enter-active,.slide-leave-active{transition: all 0.5s;}
    .goods {
        position: fixed;
        left: 0;
        right: 0;
        top: 175px;
        bottom: 48px;
        overflow: hidden;
        display: flex;
        .goods-menu {
            background: #f3f5f7;
            min-width: 80px;
            width: 80px;
            li {
                height: 54px;
                padding: 0 12px;
                p {
                    height: 54px;
                    border-bottom: 1px solid rgba(7,17,27,.1);
                    font-size: 12px;
                    font-weight: 200;
                    color: rgb(77,85,93);
                    display: flex;
                    align-items: center;
                }
                &.active{
                    background: #fff;
                    p {
                        border-bottom: none;
                    }
                }
            }
        }
        .goods-list{
            flex-grow: 1;
            dt {
                background: #f3f5f7;
                border-left: 3px solid #d9dde1;
                height: 26px;
                line-height: 26px;
                font-size: 12px;
                color: rgb(147,153,159);
                padding-left: 14px;
            }
            dd {
                margin:18px;
                display: flex;

                img {
                    width: 57px;
                    height: 57px;
                    margin-right: 10px;
                }
                .food{
                    position: relative;
                    flex-grow: 1;
                    h2 {
                        font-size: 14px;
                        color: rgb(7,17,27);
                    }
                    .desc,.sale {
                        font-size: 10px;
                        color: rgb(147,153,159);
                        margin:8px 0;
                    }
                    .price {
                        strong {
                            color: rgb(240,20,20);
                            font-weight: 700;
                            font-size: 14px;
                            span {
                                font-size: 10px;
                            }
                        }
                        del {
                            color: rgb(147,153,159);
                            font-weight: 700;
                            font-size: 10px;
                        }
                    }
                    .ele-buy{
                        position: absolute;
                        right : 0;
                        bottom: 0;
                    }
                }
            }
        }
    }
</style>
