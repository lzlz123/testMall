<template>
    <div class="bottom-menu">
        <CheckButton @click.native="chickitem" class="select-all" v-model="isSelectAll"
                     @checkBtnClick="checkBtnClick"></CheckButton>
        <span>全选</span>
        <span class="total-price">合计:￥{{totalPrice}}</span>
        <span class="buy-product" @click="calcThe">去计算({{cartLength}})</span>
    </div>
</template>

<script>
    import CheckButton from "../../../components/content/checkButton/CheckButton";
    import {mapGetters} from 'vuex'

    export default {
        name: "BottomBar",
        components: {
            CheckButton
        },
        computed: {
            ...mapGetters([
                'cartLength',
                'cartList'
            ]),
            totalPrice() {
                const cartList = this.cartList;
                return cartList.filter(item => {
                    return item.checked
                }).reduce((preValue, item) => {
                    return preValue + item.count * item.price
                }, 0).toFixed(2)
            },


            isSelectAll() {
                //这里属性的改变，会直接传递到 prop  通过监听事件  修改 data
                if (this.cartLength === 0) return false
                else return this.cartList.find(item => item.checked === false) === undefined
            },
        },
        methods: {
            checkBtnClick() {
                this.check = this.cartList.find(item => !item.checked)
            },
            chickitem() {
                let isSelectAll = this.cartList.find(item => !item.checked);
                // 2.有未选中的内容, 则全部选中
                if (isSelectAll) {
                    this.$store.commit('one')
                } else {
                    this.$store.commit('two')
                }
            },
            calcThe() {
                let a = false;
                for (let item of this.cartList) {
                    if (item.checked === true) {
                        a = true
                    }
                }
                if (a === false) {
                    this.$toast.show('请选择一个商品')
                }
            }

        },
    }


</script>

<style scoped>
    .bottom-menu {
        width: 100%;
        height: 44px;
        background-color: #eee;
        position: fixed;
        bottom: 49px;
        left: 0;
        box-shadow: 0 -2px 3px rgba(0, 0, 0, .2);
        font-size: 14px;
        color: #888;
        line-height: 44px;
        padding-left: 35px;
        box-sizing: border-box;
    }

    .bottom-menu .select-all {
        position: absolute;
        line-height: 0;
        left: 12px;
        top: 13px;
    }

    .bottom-menu .total-price {
        margin-left: 15px;
        font-size: 16px;
        color: #666;
    }

    .bottom-menu .buy-product {
        background-color: orangered;
        color: #fff;
        width: 100px;
        height: 44px;
        text-align: center;
        line-height: 44px;
        float: right;
    }
</style>
