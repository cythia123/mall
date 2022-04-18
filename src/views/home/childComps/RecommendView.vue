<template>
    <div class="recommend">
        <div v-for="item in recommends" :key="item.id" class="recommend-item">
            <a :href="item.link">
                <img :src="item.image" alt="">
            </a>
            <div class="recommend-title">{{item.title}}</div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'RecommendView',
    props: {
        recommends: {
        type: Array,
        default() {
            return []
        }
      }  
    },
}
</script>

<style scoped>
    .recommend{
        display: flex;
        text-align: center;
        border-bottom: 10px solid #eee;
    }
    .recommend-item{
        flex: 1;
        padding: 10px 0 20px;
    }
    .recommend-item img{
        width: 70px;
        height: 70px;
        margin-bottom: 10px;
    }
    .recommend-title{
        font-size: 12px;
    }
</style>