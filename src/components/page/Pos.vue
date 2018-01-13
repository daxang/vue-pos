<template>
  <div class="pos">
      <div>
          <el-row >
              <el-col :span='7' id="order-list" class="pos-order" >
                <el-tabs>
                    <el-tab-pane label="点餐">
                        <el-table :data="tableData" border  style="width: 100%" >
                             <el-table-column prop="goodsName" label="商品"  ></el-table-column>
                             <el-table-column prop="count" label="量" width="50"></el-table-column>
                             <el-table-column prop="price" label="金额" width="70"></el-table-column>
                             <el-table-column  label="操作" width="100" fixed="right">
                                    <template scope="scope">
                                    <el-button type="text" size="small" @click="delCurGoods(scope.row)">删除</el-button>
                                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                                     </template>
                                    </el-table-column>
                             </el-table>
                            <div>
                                <span>{{totalCount}}个</span><span>{{totalMoney}}元</span>
                            </div>
                            <div style="margin-top:10px"> 
                              <el-button type="warning" >挂单</el-button>
                             <el-button type="danger" @click="delAllGoods()">删除</el-button>
                             <el-button type="success" @click= "checkOut()">结账</el-button>
                                </div>
                    </el-tab-pane>

                    <el-tab-pane label="挂单">
                    挂单
                    </el-tab-pane>

                    <el-tab-pane label="外卖">
                    外卖
                    </el-tab-pane>
                
                </el-tabs>
               
                
               
              </el-col>
            

            <el-col :span='17' id="order-list" >
                <!--上栏：常用商品-->
                 <div class="often-goods" style="border:1px solid red">
                    <div class="title">常用商品</div>
                     <div class="often-goods-list">               
                         <ul>
                              <li  v-for ="goods in oftenGoods" @click="addOrderList(goods)">
                                 <span>{{goods.goodsName}}</span>
                                 <span class="o-price">￥{{goods.price}}元</span>
                             </li>
                            </ul>
                        </div>
                    </div>     <!--上栏：常用商品-->

                 <!--下栏：具体商品类别-->
                 <div class="all-goods">
                     <el-tabs>
                         <el-tab-pane label="汉堡">
                             <ul class="goods-list">
                                 <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                                     <span class="food-img"><img :src="goods.goodsImg" alt="" width="100%"></span>
                                     <p class="food-name">{{goods.goodsName}}</p>
                                     <p class="food-food-pirce">￥{{goods.price}}元</p>
                                 </li>
                             </ul>
                         </el-tab-pane>
                         <el-tab-pane label="小食">
                              <ul class="goods-list">
                                 <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                                     <span class="food-img"><img :src="goods.goodsImg" alt="" width="100%"></span>
                                     <p class="food-name">{{goods.goodsName}}</p>
                                     <p class="food-food-pirce">￥{{goods.price}}元</p>
                                 </li>
                             </ul>
                         </el-tab-pane>
                         <el-tab-pane label="饮料">
                              <ul class="goods-list">
                                 <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                                     <span class="food-img"><img :src="goods.goodsImg" alt="" width="100%"></span>
                                     <p class="food-name">{{goods.goodsName}}</p>
                                     <p class="food-food-pirce">￥{{goods.price}}元</p>
                                 </li>
                             </ul>
                         </el-tab-pane>
                         <el-tab-pane label="套餐">
                              <ul class="goods-list">
                                 <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                                     <span class="food-img"><img :src="goods.goodsImg" alt="" width="100%"></span>
                                     <p class="food-name">{{goods.goodsName}}</p>
                                     <p class="food-food-pirce">￥{{goods.price}}元</p>
                                 </li>
                             </ul>
                         </el-tab-pane>
                     </el-tabs>
                 </div>
              </el-col>
          </el-row>
      </div>
      	
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Pos",
  data() {
    return {
      tableData: [],
      oftenGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      totalCount: 0,
      totalMoney: 0
    };
  },
  created() {
    axios
      .get("http://jspang.com/DemoApi/oftenGoods.php")
      .then(response => {
        this.oftenGoods = response.data;
      })
      .catch(error => {
        alert("网络访问不大，出了什么问题");
      });
    axios
      .get("http://jspang.com/DemoApi/typeGoods.php")
      .then(response => {
        this.type0Goods = response.data[0];
        this.type1Goods = response.data[1];
        this.type2Goods = response.data[2];
        this.type3Goods = response.data[3];
      })
      .catch(error => {
        alert("网络访问不大，出了什么问题");
      });
  },
  mounted: function() {
    var orderheight = document.body.clientHeight;
    document.getElementById("order-list").style.height = orderheight + "px";
  },
  methods: {
    //添加订单列表的方法
    addOrderList(goods) {
      this.totalCount = 0; //汇总数量清0
      this.totalMoney = 0;
      let isHave = false;
      //判断是否这个商品已经存在于订单列表
      for (let i = 0; i < this.tableData.length; i++) {
        console.log(this.tableData[i].goodsId);
        if (this.tableData[i].goodsId == goods.goodsId) {
          isHave = true; //存在
        }
      }
      //根据isHave的值判断订单列表中是否已经有此商品
      if (isHave) {
        //存在就进行数量添加
        let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
        arr[0].count++;
        //console.log(arr);
      } else {
        //不存在就推入数组
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        };
        this.tableData.push(newGoods);
      }
      this.getCountsMoney();

    },
    //进行数量和价格的汇总计算
    delCurGoods(goods) {
      this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
      this.getCountsMoney();
    },
    //删掉整个订单
    delAllGoods() {
      this.tableData = [];
      this.totalCount = 0;
      this.totalMoney = 0;
    },
    //每次编辑都重新汇总一下数量和金额
    getCountsMoney() {
      this.totalMoney = 0;
      this.totalCount = 0;
      if (this.tableData) {
        this.tableData.forEach(element => {
          this.totalCount += element.count;
          this.totalMoney = this.totalMoney + (element.price * element.count);
        });
      }
    },
    checkOut(){
       if(this.totalCount){
           this.tableData=[];
           this.totalCount=0;
           this.totalMoney=0;
           this.$message({
               message: '结账成功，感谢你又为店里出了一份力',
               type: 'success'
           });
       }else{
           this.$message({
               message: '不能空结哦',
               type: 'error'
           })
       }
    }
  }
};
</script>
<style scoped>
.title {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
}
.often-goods .all-goods {
  display: block;
  width: 100%;
}
.all-goods {
  clear: both;
  margin-top: 10px;
  margin-left: 10px;
}
.often-goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 5px;
  background-color: #fff;
}
.o-price {
  color: #58b7ff;
}

.pos-order {
  background: #f9fafc;
  border-right: 1px solid green;
}

.goods-list li {
  list-style-type: square none;
  width: 23%;
  height: auto;
  border: 1px solid #e5e9f2;
  background: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
  overflow: hidden;
}
.goods-list li span {
  display: block;
  float: left;
}
.food-img {
  width: 40%;
}
.food-name {
  font-size: 18px;
  padding-left: 10px;
  color: brown;
}
.food-price {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
</style>


