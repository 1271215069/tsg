<template>
    <div class="staffdetail">
        <p class="modtitle">
            <span class="title">员工详情</span>
            <a-button class="edit" type="primary">编辑</a-button>
        </p>
        <ul class="toplist">
            <li>
                <span class="title">员工姓名：</span>张美丽
            </li>
            <li>
                <span class="title">手机：</span>15958123784
            </li>
            <li>
                <span class="title">业务等级：</span>LV.5
            </li>
            <li>
                <span class="title">个人总业绩（元）：</span>500000.0
            </li>
            <li>
                <span class="title">个人总订单（件）：</span>5423542
            </li>
            <li>
                <span class="title">回款率：</span>100%
            </li>
            <li>
                <span class="title">本店业绩排名：</span>第5名
            </li>
            <li>
                <span class="title">全部业务员业绩排名：</span>第5名
            </li>
        </ul>
        <a-divider />
        <div class="btolist">
            <p class="title">团队成员</p>
            <div class="supperson">
                <p>
                    <span class="title">上级业务员：</span>林梅西
                </p>
                <p>
                    <span class="title">上上级业务员：</span>陈丽梅
                </p>
            </div>
            <div class="subperson">
                <span class="title">下级业务员：</span>
                <ul class="sublist">
                    <li>陈丽妃（提供收益：25894.53元，下级：490个）<a-icon @click.native="showmedetail" type="down-square" style="color:#1890FF;cursor: pointer;"/></li>
                    <li>陈丽妃（提供收益：25894.53元，下级：490个）<a-icon @click.native="showmedetail" type="down-square" style="color:#1890FF;cursor: pointer;"/></li>
                </ul>
            </div>
        </div>
        <a-modal
            v-model="medetaildata.show"
            wrapClassName="staff_member"
            width='800px'
            :centered='true'
            :footer="null"
            :zIndex='1200'
        >
            <ul class="subperlist">
                <li>陈丽妃（提供收益：25894.53元）</li>
                <li>陈丽妃（提供收益：25894.53元）</li>
            </ul>
        </a-modal>
    </div>
</template>
<script>
export default {
    data(){
        return{
            medetaildata:{//下下级业务员弹窗配置
                show:false,
                data:{}
            }
        }
    },
    methods:{
        showmedetail(){//查看下下级业务员的按钮方法
            this.medetaildata.show=true;
        }
    }
}
</script>
<style lang="less" scoped>
.staffdetail{
    background: #fff;
    box-sizing: border-box;
    padding: 20px;
    font-size: 16px;
    .modtitle{
        overflow: hidden;
        .title{
            font-size: 18px;
            font-weight: 600;
            display: block;
            line-height: 32px;
            float: left;
        }
        .edit{
            float: right;
        }
    }
    .toplist{
        padding: 0;
        margin: 0;
        list-style: none;
        overflow: hidden;
        li{
            float: left;
            width: 33%;
            margin: 10px 0;
            font-size: 14px;
            .title{
                font-weight: 600;
            }
        }
    }
    .btolist{
        .title{
            margin: 0;
            line-height: 32px;
            font-size: 18px;
            font-weight: 600;
        }
        .supperson{
            p{
                width:50%;
                display: inline-block;
                margin: 10px 0;
                font-size: 14px;
                .title{
                    font-weight: 600;
                    font-size: 14px;
                }
            }
        }
        .subperson{
            overflow: hidden;
            
            .title{
                font-size: 14px;
                font-weight: 600;
                float: left;
            }
            .sublist{
                float: left;
                width: 1000px;
                padding: 0;
                margin: 0;
                overflow: hidden;
                li{
                    list-style: none;
                    width: 45%;
                    float: left;
                    line-height: 32px;
                    margin-bottom: 10px;
                    font-size: 14px;
                }
            }
        }
    }
}
</style>
