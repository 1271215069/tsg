<template>
    <div class="product">
        <div class="search">
            <a-select
                v-model="chosestore"
                placeholder="请选择分馆"
                showSearch
                optionFilterProp="children"
                :filterOption='filterOption'
                style="width: 200px;float:left;"
                @change="searchstore"
            >
                <a-select-option v-for="(item,index) in branchlist" :key="index" :value='item.val'>{{item.label}}</a-select-option>
            </a-select>
            <a-button type="primary" style="float:right;margin-left:20px;">新增</a-button>
            <a-input-search
                placeholder="请按商品编号/产品名称/所属店铺搜索"
                style="width:330px;float:right;"
                @search="searchnum"
            />
        </div>
        <div class="main">
            
        </div>
    </div>
</template>
<script>
export default {
    name:"product",
    data(){
        return{
            chosestore:"",//分店的选中值绑定
            branchlist:[//分店select的可选项
                {
                    val:'1',
                    label:"杭州分店"
                },
                {
                    val:'2',
                    label:"宁波分店"
                },
                {
                    val:'3',
                    label:"上海分店"
                },
            ]
        }
    },
    methods:{
        filterOption(input, option) {//根据输入项进行筛选
            return option.componentOptions.children[0].text.toLowerCase().indexOf(input.toLowerCase()) >= 0
        },
        searchstore(val,opt){//获取到输入框值改变的回调,执行查询更新table的数据
            console.log(val,666)
            console.log(opt,333)
        },
        searchnum(val){//点击右侧搜索图标的回调
            console.log(val,555)
        }
    }
}
</script>
<style lang="less" scoped>
.product{
    background: #fff;
    box-sizing: border-box;
    padding: 20px;
    .search{
        overflow: hidden;
    }
}
</style>
