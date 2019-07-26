<template>
    <div class="orderlist">
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
            <a-input-search
                placeholder="请按商品编号/产品名称/所属店铺搜索"
                style="width:330px;float:right;"
                @search="searchnum"
            />
        </div>
        <div class="main">
            <a-table class="detaillist" :columns="columns" :dataSource="tabdata" :pagination="false">
                <span slot="ope" slot-scope="text,record">
                    <a @click.prevent="com(record)">详情</a>
                    <a-divider type="vertical" />
                    <a @click.prevent="logistics(record)">查看物流</a>
                </span>
            </a-table>
        </div>
    </div>
</template>
<script>
export default {
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
            ],
            columns:[//table的表头
                {
                    title:"订单编号",
                    dataIndex: 'num',
                    key: 'num',
                    align:"center",
                },
                {
                    title:"产品",
                    dataIndex: 'proname',
                    key: 'proname',
                    align:"center",
                },
                {
                    title:"业务员",
                    dataIndex: 'salesman',
                    key: 'salesman',
                    align:"center",
                },
                {
                    title:"购买客户",
                    dataIndex: 'customer',
                    key: 'customer',
                    align:"center",
                },
                {
                    title:"购买金额",
                    dataIndex: 'price',
                    key: 'price',
                    align:"center",
                },
                {
                    title:"数量",
                    dataIndex: 'amount',
                    key: 'amount',
                    align:"center",
                },
                {
                    title:"支付方式",
                    dataIndex: 'type',
                    key: 'type',
                    align:"center",
                },
                {
                    title:"订单状态",
                    dataIndex: 'state',
                    key: 'state',
                    align:"center",
                },
                {
                    title: '操作',
                    key: 'ope',
                    scopedSlots: { customRender: 'ope' },
                    align:"center",
                }
            ],
            tabdata:[//table内的数据
                {
                    key:'1',
                    id:"20",
                    num:'1354654654655316',
                    proname:'紫砂罐',
                    salesman:'林大美',
                    customer:'张春春',
                    price:'20000',
                    amount:'2',
                    type:'现金支付',
                    state:'已完成',
                },
            ],
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
        },


        com(record){//列表点击详情的按钮

        },
        logistics(record){//列表点击查看物流的方法

        }
    }
}
</script>
<style lang="less" scoped>
.orderlist{
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
    }
}
</style>
