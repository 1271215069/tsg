<template>
    <div class="reim_approval">
        <div class="search">
            <a-select v-model="detailtype" class="einput" placeholder="请选择">
                <a-select-option v-for="item in typeopo"  :key="item.val">{{item.label}}</a-select-option>
            </a-select>
            <a-input-search
                placeholder="请按商品编号/产品名称/所属店铺搜索"
                style="width:330px;float:right;"
                @search="searchnum"
            />
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return{
            detailtype:'',
            typeopo:[//审核通过select的选项数据
                {
                    val:'1',
                    label:"采购"
                },
                {
                    val:'2',
                    label:"报销"
                }
            ],
        }
    },
    methods:{
        searchnum(val){//右上方搜索框的方法

        }
    }
}
</script>
<style lang="less" scoped>
.reim_approval{
    background: #fff;
    box-sizing: border-box;
    padding: 20px;
    font-size: 16px;
    .search{
        overflow: hidden;
    }
    .main{
        .detaillist{
            margin: 30px 0;
        }
        .fy{
            margin-top: 50px;
            text-align: center;
        }
    }
}
</style>
