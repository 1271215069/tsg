<template>
    <div class="order_approval">
        <div class="search">
            <a-input-search
                placeholder="请按商品编号/产品名称/所属店铺搜索"
                style="width:330px;float:right;"
                @search="searchnum"
            />
        </div>
        <div class="main">
            <a-table class="detaillist" :columns="columns" :dataSource="tabdata" :pagination="false">
                <span slot="voucher" slot-scope="text,record">
                    <a @click.prevent="see(record)">点击查看</a>
                </span>
                <span slot="ope" slot-scope="text,record">
                    <a @click.prevent="examine(record)">审核</a>
                </span>
            </a-table>
            <a-pagination
                    class="fy"
                    :total="pagedata.total"
                    :showTotal="total => `总共 ${total} 条`"
                    :pageSize="pagedata.pagesize"
                    :current="pagedata.current"
                    :defaultCurrent="1"
                    :hideOnSinglePage='true'
                />
        </div>

        <!-- 查看支付凭证的弹窗 -->
        <a-modal    
            v-model="voucherimg.show"
            wrapClassName="approval_voucherimg"
            :title='voucherimg.title'
            width='800px'
            :centered='true'
            :footer="null"
            >
            <img v-if="voucherimg.data.voucher" :src="voucherimg.data.voucher" style="display:block;width:80%;margin:20px auto;" alt="">
        </a-modal>

        <!-- 审核的弹窗 -->
        <a-modal
            v-model="examinedata.show"
            wrapClassName="approval_examine"
            :title='examinedata.title'
            width='800px'
            :centered='true'
            @ok="examinecheck"
        >
            <ul class="examinelist">
                <li>
                    <span class="title">是否通过：</span>
                    <a-select v-model="examinedata.result" class="einput" placeholder="请选择">
                        <a-select-option v-for="item in examineopo"  :key="item.val">{{item.label}}</a-select-option>
                    </a-select>
                </li>
                <li>
                    <span class="title">理由：</span>
                    <a-textarea v-model="examinedata.reason" class="einput" placeholder="请输入备注" :rows="4" :autosize="false" />
                </li>
            </ul>
        </a-modal>

    </div>
</template>
<script>
export default {
    data(){
        return{
            columns:[
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
                    title:"结算方式",
                    dataIndex: 'type',
                    key: 'type',
                    align:"center",
                },
                {
                    title: '支付凭证',
                    key: 'voucher',
                    scopedSlots: { customRender: 'voucher' },
                    align:"center",
                },
                {
                    title: '操作',
                    key: 'ope',
                    scopedSlots: { customRender: 'ope' },
                    align:"center",
                }
            ],
            tabdata:[
                {
                    key:'1',
                    id:'20',
                    num:'1354654654655316',
                    proname:'紫砂罐',
                    salesman:'林大美',
                    customer:'张春春',
                    price:'20000',
                    amount:'2',
                    type:'现金支付',
                    voucher:"https://hbimg.huabanimg.com/0d925bd33b9671d6ecf84c4b7c9d1113cb675b211d20e-ZCPm71_fw658",
                }
            ],
            pagedata:{//分页数据
                pagesize:5,
                current:1,
                total:100//总条数
            },
            voucherimg:{//支付凭证弹窗的配置
                show:false,
                title:"支付凭证",
                data:{}
            },
            examinedata:{//审核弹窗的配置
                show:false,
                title:"审核结果",
                data:{},
                result:"",
                reason:""
            },
            examineopo:[//审核通过select的选项数据
                {
                    val:'1',
                    label:"是"
                },
                {
                    val:'2',
                    label:"否"
                }
            ],
        }
    },
    methods:{
        searchnum(val){//右上方搜索框的方法

        },
        see(record){//列表点击查看凭证的方法
            this.voucherimg.show=true;
            this.voucherimg.data=record;
        },
        examine(record){//列表点击审核的方法
            this.examinedata.show=true;
            this.examinedata.data=record;
        },
        examinecheck(){//审核弹窗的确定按钮
            console.log(this.examinedata)
        }
    }
}
</script>
<style lang="less" scoped>
.order_approval{
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
