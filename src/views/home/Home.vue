<template>
    <div id="home">
        <nav-bar class="home-nav">
            <template #center>
                <div>
                    购物车
                </div>
            </template>
        </nav-bar>
        <home-swiper :banners="banners"></home-swiper>
        <recommend-view :recommends="recommends"></recommend-view>
        <feature-view/>
        <tab-control :titles="['流行', '新款', '精选']" class="tab-control" @tabClick="tabClick"></tab-control>
        <goods-list :goods="showGoods"></goods-list>
        
    </div>
</template>

<script>
import navBar from '@/components/common/navbar/navBar'
import HomeSwiper from './childComps/HomeSwiper' 
import RecommendView from '@/views/home/childComps/RecommendView'
import FeatureView from '@/views/home/childComps/FeatureView'
import TabControl from '@/components/content/tabControl/TabControl'
import GoodsList from '@/components/content/goods/GoodsList'

import {getHomeMultidata, getHomeGoods} from '@/network/home'


export default {
    name: 'Home',
    components: {
        navBar,
        HomeSwiper,
        RecommendView,
        FeatureView,
        TabControl,
        GoodsList
    },
    data() {
        return {
            banners: [],
            recommends: [],
            goods: {
                'pop': {page: 0, list: []},
                'new': {page: 0, list: []},
                'sell': {page: 0, list: []},
            },
            currentType: 'pop'
        }
    },
    created() {
        // 1 请求多个数据
        this.getHomeMultidata()

        // 2 请求商品数据
        this.getHomeGoods('pop')
        this.getHomeGoods('new')
        this.getHomeGoods('sell')
    },
    computed: {
        showGoods() {
            return this.goods[this.currentType].list
        }
    },
    methods: {
        getHomeMultidata() {
            getHomeMultidata().then(res => {
                this.banners = res.data.banner.list
                this.recommends = res.data.recommend.list
            })
        },
        getHomeGoods(type) {
            const page = this.goods[type].page + 1
            getHomeGoods(type, page).then(res => {
                this.goods[type].list.push(...res.data.list)
                this.goods[type].page += 1
            })
        },
        tabClick(index) {
            switch(index) {
                case 0:
                    this.currentType = 'pop'
                    break
                case 1:
                    this.currentType = 'new'
                    break
                case 2:
                    this.currentType = 'sell'
                    break
            }
        }
    }

}
</script>

<style scoped>
    #home{
        padding-top: 44px;
    }
    .home-nav{
        color: #fff;
        background-color: var(--color-tint);
        position: fixed;
        left: 0;
        right: 0;
        top: 0;
        z-index: 9;
    }
    .tab-control{
        position: sticky;
        top: 44px;
        z-index: 9
    }
</style>