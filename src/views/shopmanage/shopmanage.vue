<template>
    <div class="shopmanage">
        <div class="shopinfo"><!-- 页面上方店铺信息 -->
            <div class="toptitle">
                <a-menu
                class="title"
                :selectedKeys="['mail']"
                mode="horizontal">
                    <a-menu-item key="mail">
                        店铺信息
                    </a-menu-item>
                </a-menu>
                <a-button class="editshop" type="primary" @click.native="showedit">编辑</a-button>
            </div>
            <div class="shopcontant">
                <div class="left">
                    <img :src="shopcontant.imgurl" alt="">
                </div>
                <ul class="right">
                    <li>
                        <span class="rtitle">店铺名称：</span>
                        <span class="rcon">{{shopcontant.name}}</span>
                    </li>
                    <li>
                        <span class="rtitle">店铺地址：</span>
                        <span class="rcon">{{shopcontant.address}}</span>
                    </li>
                    <li>
                        <span class="rtitle">店铺负责人：</span>
                        <span class="rcon">{{shopcontant.person}}</span>
                    </li>
                    <li>
                        <span class="rtitle">联系方式：</span>
                        <span class="rcon">{{shopcontant.tel}}</span>
                    </li>
                    <li class="qrlist">
                        <span class="rtitle">首付款二维码：</span>
                        <div class="qrimg">
                            <img :src="shopcontant.qrcode[0]" alt="">
                            <p>微信二维码</p>
                        </div>
                        <div class="qrimg">
                            <img :src="shopcontant.qrcode[1]" alt="">
                            <p>支付宝二维码</p>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
        <div class="brashoplist">
            <div class="toptitle">
                <a-menu
                class="title"
                :selectedKeys="['mail']"
                mode="horizontal">
                    <a-menu-item key="mail">
                        分店列表
                    </a-menu-item>
                </a-menu>
                <a-button class="addshop" type="primary" @click.native="addshop">添加分店</a-button>
            </div>
            <div class="listcontant">
                <a-list
                itemLayout="horizontal"
                :dataSource="bralistcontant"
                >
                    <a-list-item slot="renderItem" slot-scope="item, index">
                        <div class="column1" style="width:30%">
                            <p style="font-weight:600;">{{item.name}}</p>
                            <p >{{item.address}}</p>
                        </div>
                        <div class="column2" style="width:10%">
                            <p>店长：</p>
                            <p>{{item.person}}</p>
                        </div>
                        <div class="column3" style="width:13%">
                            <p>分店员工人数：</p>
                            <p>{{item.pernum}}</p>
                        </div>
                        <div class="column4" style="width:12%">
                            <p>开店时间：</p>
                            <p>{{item.startime}}</p>
                        </div>
                        <div class="column5" style="width:15%">
                            <p>状态：</p>
                            <p>
                                <span v-if="item.statecode==1" style="color:#1890FF;">●</span>
                                <span v-if="item.statecode==2" style="color:#D9001B;">●</span>
                                {{item.state}}
                            </p>
                        </div>
                        <div class="column6" style="width:20%;text-align:right;line-height:60px;">
                            <span @click.prevent="editbra">编辑</span>
                            <a-divider type="vertical" />
                            
                            <span v-if="item.statecode==1" @click.prevent='frozen(2)'>冻结</span>
                            <span v-if="item.statecode==2" @click.prevent='frozen(1)'>解冻</span>
                        </div>
                    </a-list-item>
                </a-list>
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
        </div>


        <!-- 店铺信息弹窗 -->
        <a-modal
            v-model="editshop.show"
            wrapClassName="shopmanage_editshop"
            title='店铺信息'
            width='800px'
            :centered='true'
        >
            <ul class="editlist">
                <li>
                    <span class="title">店铺照片：</span>
                </li>
                <li>
                    <span class="title">店铺名称：</span>
                    <a-input class="einput" placeholder="请输入" v-model="editshop.name"></a-input>
                </li>
                <li>
                    <span class="title">地址：</span>
                    <a-input class="einput" placeholder="请输入" v-model="editshop.address"></a-input>
                </li>
                <li>
                    <span class="title">店铺负责人：</span>
                    <a-input class="einput" placeholder="请输入" v-model="editshop.person"></a-input>
                </li>
                <li>
                    <span class="title">联系方式：</span>
                    <a-input class="einput" placeholder="请输入" v-model="editshop.tel"></a-input>
                </li>
                <li>
                    <span class="title">收付款二维码：</span>
                    <a-upload
                    listType='picture-card'
                    :fileList="editshop.wxcode"
                    :beforeUpload="wxbeforeUpload"
                    :remove="wxremove"
                    :preview="wxpreview"
                    >
                        <div v-if="editshop.wxcode.length < 1">
                            <a-icon v-if="!editshop.wxcode[0]" type="plus" />
                            <div v-if="!editshop.wxcode[0]" class="ant-upload-text">Upload</div>
                            <img v-if="editshop.wxcode[0]" :src="editshop.wxcodeurl" alt="">
                        </div>
                        
                    </a-upload>
                </li>
            </ul>
        </a-modal>

        <!-- 分店信息弹窗 -->
        <a-modal
            v-model="brashopdata.show"
            wrapClassName="shopmanage_brashop"
            :title='brashopdata.title'
            width='800px'
            :centered='true'
            @ok='brafix'
            >
            <ul class="bradatalist">
                <li>
                    <span class="title">分店名称：</span>
                    <a-input class="einput" placeholder="请输入" v-model="brashopdata.name"></a-input>
                </li>
                <li>
                    <span class="title">分店地址：</span>
                    <a-cascader class="einput" v-model="brashopdata.address" @change="adresschange" :options="cityinfo" placeholder="请输入" />
                </li>
                <li>
                    <span class="title">开业时间：</span>
                    <a-date-picker class="einput" @change="datechange" />
                </li>
                <li>
                    <span class="title">分店负责人：</span>
                    <a-input class="einput" placeholder="请输入" v-model="brashopdata.person"></a-input>
                </li>
                <li>
                    <span class="title">联系方式：</span>
                    <a-input class="einput" placeholder="请输入" v-model="brashopdata.tel"></a-input>
                </li>
                <li>
                    <span class="title">状态：</span>
                    <a-radio-group class="state" v-model="brashopdata.statecode">
                        <a-radio :value="1">正常</a-radio>
                        <a-radio :value="2">冻结</a-radio>
                    </a-radio-group>
                </li>
            </ul>
        </a-modal>

        <a-modal
            v-model="frozendata.show"
            title='确认'
            width='800px'
            :centered='true'
            @ok='frok'
        >
            <div style="text-align:center;">
                {{frozendata.str}}
            </div>
        </a-modal>

    </div>
</template>
<script>
import { CityInfo } from "@/api/city-data";
export default {
    name:'shopmanage',
    data(){
        return{
            shopcontant:{//店铺信息的数据
                imgurl:require('@/assets/img/u77.jpg'),
                name:"宁海高校管理（总店）",
                address:"宁波市宁海县岔路镇上金·财神谷",
                person:"蔡晓芬",
                tel:"15958635843",
                qrcode:[require('@/assets/img/nullimg.png'),require('@/assets/img/nullimg.png')]
            },
            bralistcontant:[//分店列表数据
                {
                    name:"宁海体育馆",
                    address:"宁海县人民大道体育馆路385号",
                    person:"林祥柯",
                    pernum:"200",
                    startime:"2019-7-24",
                    state:"正常",
                    statecode:1
                },
                {
                    name:"宁海体育馆",
                    address:"宁海县人民大道体育馆路385号",
                    person:"林祥柯",
                    pernum:"200",
                    startime:"2019-7-24",
                    state:"冻结",
                    statecode:2
                },
                {
                    name:"宁海体育馆",
                    address:"宁海县人民大道体育馆路385号",
                    person:"林祥柯",
                    pernum:"200",
                    startime:"2019-7-24",
                    state:"正常",
                    statecode:1
                },
            ],
            pagedata:{//分页数据
                pagesize:5,
                current:1,
                total:100//总条数
            },
            editshop:{//修改店铺弹窗的信息
                show:false,
                photo:require('@/assets/img/u77.jpg'),
                name:"宁海高校管理（总店）",
                address:"宁波市宁海县岔路镇上金·财神谷",
                person:"蔡晓芬",
                tel:"15958635843",
                wxcode:[],
                wxcodeurl:"",
                zfbcode:[],
                zfbcodeurl:""
            },
            brashopdata:{//分店信息的弹窗
                show:false,
                title:'',
                name:'',
                address:[],//地区绑定的value值
                addreobj:{//地区绑定label值
                    province:'',
                    city:'',
                    area:''
                },
                startime:'',
                person:'',
                tel:'',
                statecode:1//1代表正常2代表冻结
            },
            frozendata:{//冻结解冻弹窗的参数
                show:false,
                str:""
            }
        }
    },
    computed:{
        cityinfo(){//城市信息
            return CityInfo();
        }
    },
    methods:{
        showedit(){//店铺信息点击编辑按钮的方法
            this.editshop.show=true;
        },
        addshop(){//分店列表点击添加分店的方法
            this.brashopdata.show=true;
            this.brashopdata.title="添加分店";
        },
        editbra(){//分店列表编辑按钮的方法
            this.brashopdata.show=true;
            this.brashopdata.title="编辑分店信息";
        }, 
        adresschange(val,selopt){//分店地址发生变化的回调
            this.brashopdata.addreobj.province=selopt[0].label;
            this.brashopdata.addreobj.city=selopt[1].label;
            this.brashopdata.addreobj.area=selopt[2].label;
        },
        datechange(date,strdate){//开业时间变化的值
            console.log(date)
            console.log(strdate)
            this.brashopdata.startime=strdate;
        },
        brafix(){//分店信息弹窗点击确定的回调
            console.log(this.brashopdata)
        },
        frozen(state){//列表冻结解冻的选项
            this.frozendata.show=true;
            if(state==1){
                this.frozendata.str='确定解冻该分店吗？'
            }else if(state==2){
                this.frozendata.str='确定冻结该分店吗？'
            }

        },
        frok(){//冻结解冻弹窗确定的按钮

        },



        getObjectURL(file){//获取文件的显示路劲
            var reader = new FileReader();
            reader.readAsDataURL(file);
            var that = this;
            reader.onload = function(x){
                that.editshop.wxcodeurl=this.result;
            }
        },
        wxbeforeUpload(file){//上传wx二维码文件的回调
            this.editshop.wxcode=[];
            this.editshop.wxcode.push(file);
            console.log(this.editshop.wxcode,55555)
            this.getObjectURL(file);
            console.log(this.editshop.wxcodeurl,333)
            return true;
        },
        wxremove(){//删除上传微信二维码文件的回调
            this.editshop.wxcode=[];
            this.editshop.wxcodeurl="";
            return false;
        },
        wxpreview(){//预览上传微信二维码文件的回调
            console.log(this.editshop.wxcodeurl)
        },

    }
}
</script>
<style lang="less" scoped>
.shopmanage{
    // background: #fff;
    font-size: 16px;
    .shopinfo{
        box-sizing: border-box;
        padding: 20px;
        background: #fff;
        .toptitle{
            position: relative;
            .editshop{
                position: absolute;
                right: 20px;
                top: 50%;
                transform: translateY(-50%);
            }
        }
        .shopcontant{
            overflow: hidden;
            .left{
                float: left;
                width: 17%;
                img{
                    display: block;
                    width: 150px;
                    height: 150px;
                    margin: 30px auto 0;
                    border-radius: 50%;
                }
            }
            .right{
                float: right;
                width: 83%;
                list-style: none;
                overflow: hidden;
                margin: 30px 0 0;
                li{
                    width: 35%;
                    float: left;
                    line-height: 45px;
                    font-size: 14px;
                    .rtitle{
                        font-weight: 600;
                    }
                }
                .qrlist{
                    width: 100%;
                    overflow: hidden;
                    .rtitle{
                        float: left;
                    }
                    .qrimg{
                        float: left;
                        width: 120px;
                        img{
                            width: 80%;
                            margin: 10px auto 0;
                            display: block;
                        }
                        p{
                            text-align: center;
                        }
                    }
                }
            }
        }
    }
    .brashoplist{
        margin-top: 30px;
        box-sizing: border-box;
        padding: 20px;
        background: #fff;
        .toptitle{
            position: relative;
            .addshop{
                position: absolute;
                right: 20px;
                top: 50%;
                transform: translateY(-50%);
            }
        }
        .listcontant{
            box-sizing: border-box;
            padding: 0 20px;
            .ant-list-item{
                box-sizing: border-box;
                padding: 20px 20px;
                .ant-list-item-content{
                    p{
                        margin: 0;
                        line-height:30px;
                    }
                    span{
                        cursor: pointer;
                        color: #1890FF;
                    }
                }
            }
            .fy{
                margin-top: 50px;
                text-align: center;
            }
        }
    }

}
</style>
