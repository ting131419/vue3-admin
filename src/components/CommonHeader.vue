<template>
    <el-header>
        <div class="l-content">
            <!-- 图标的展示 -->
            <el-button size='small' plain @click="handleCollapse">
                <el-icon :size="20">
                    <Menu />
                </el-icon>
            </el-button>
            <el-breadcrumb separator="/" class = 'bread'>
                    <!--首页一定是存在的所以直接写死-->
                    <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                    <el-breadcrumb-item :to="current.path" v-if="current">{{ current.label }}</el-breadcrumb-item> 
             </el-breadcrumb>
        </div>
        <div class="r-content">
            <el-dropdown>
                <span class="el-dropdown-link">
                    <img class='user' :src="getImgSrc('user')" alt="">
                    <el-icon class="el-icon--right">
                        <arrow-down />
                    </el-icon>
                </span>
                <template #dropdown>
                    <el-dropdown-menu>
                        <el-dropdown-item>个人中心</el-dropdown-item>
                        <el-dropdown-item @click="handleLoginOut">退出</el-dropdown-item>
                    </el-dropdown-menu>
                </template>
            </el-dropdown>
        </div>
    </el-header>
</template>

<script>
import { computed, defineComponent } from 'vue';
import { useStore} from 'vuex';
import { useRouter } from 'vue-router'
export default defineComponent({
    setup(){
        let store =useStore();
        // const imgSrc = require('../assets/images/user.png')
        let getImgSrc = (user) =>{
            // console.log(import.meta.url)
            // console.log(new URL(`../assets/images/${user}.png`, import.meta.url))
            return new URL(`../assets/images/${user}.png`, import.meta.url).href;
        };
        let handleCollapse = () =>{
            //调用vuex中的mutations
            store.commit("updateIsCollapse");
        };

        //计算属性
        const current = computed(()=>{
            return store.state.currentMenu;
        });
        const router = useRouter();
        const handleLoginOut = ()=>{
          store.commit('cleanMenu');  
          router.push({
            name:'login',
          });
        };

        return{
            // getImg,
            getImgSrc,
            handleCollapse,
            current,
            handleLoginOut
        };
    }
})
</script>

<style lang="less" scoped>
header{
    display:flex;
    justify-content: space-between;
    align-items: center;
    width:100%;
    background-color: #333;
}
.r-content{
    .user{
        width:40px;
        height:40px;
        border-radius: 50%;
    }
}
.l-content{
    display: flex;
    align-items: center;
    .el-button{
        margin-right: 20px;
    }
    h3{
        color: #fff;
    }
}
// .bread /deep/ span{
//color: #fff !important;
//cursor: pointer !important;
//}

:deep(.bread span){
    color:#fff !important ;
    cursor:pointer !important;
}

</style>


