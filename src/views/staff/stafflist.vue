<template>
    <div class="stafflist">
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
                <span slot="member" slot-scope="text,record">
                    <a @click.prevent="lookup(record)">查看</a>
                </span>
                <span slot="ope" slot-scope="text,record">
                    <a>详情</a>
                    <a-divider type="vertical" />
                    <a>冻结</a>
                </span>
            </a-table>
        </div>
        <a-modal    
            v-model="memberdata.show"
            wrapClassName="approval_member"
            width='800px'
            :centered='true'
            :footer="null"
            >
            <div class="memberdetil">
                <p>团队成员</p>
                
            </div>
        </a-modal>
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
            columns:[//表格头部数据
                {
                    title:"员工姓名",
                    dataIndex: 'name',
                    key: 'name',
                    align:"center",
                },
                {
                    title:"手机号码",
                    dataIndex: 'tel',
                    key: 'tel',
                    align:"center",
                },
                {
                    title: '团队成员',
                    key: 'member',
                    scopedSlots: { customRender: 'member' },
                    align:"center",
                },
                {
                    title:"业务等级",
                    dataIndex: 'levl',
                    key: 'levl',
                    align:"center",
                },
                {
                    title:"个人总业绩（元）",
                    dataIndex: 'achieve',
                    key: 'achieve',
                    align:"center",
                },
                {
                    title:"个人总订单（件）",
                    dataIndex: 'ordernum',
                    key: 'ordernum',
                    align:"center",
                },
                {
                    title:"回款率",
                    dataIndex: 'moneyback',
                    key: 'moneyback',
                    align:"center",
                },
                {
                    title:"店内排名",
                    dataIndex: 'ranking',
                    key: 'ranking',
                    align:"center",
                },
                {
                    title: '操作',
                    key: 'ope',
                    scopedSlots: { customRender: 'ope' },
                    align:"center",
                }
            ],
            tabdata:[//表格列表数据
                {
                    key:'1',
                    id:'20',
                    name:'杨大礼',
                    tel:'15959400909',
                    levl:'Lv.5',
                    achieve:'300000.00',
                    ordernum:'3495',
                    moneyback:'100%',
                    ranking:'1'
                }
            ],
            memberdata:{//团队成员弹窗配置
                show:false,
                data:{}
            }
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
        lookup(record){//列表点击查看的按钮回调
            this.memberdata.show=true;
            this.memberdata.data=record;
        }
    }
}
</script>
<style lang="less" scoped>
.stafflist{
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
    