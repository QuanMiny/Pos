<template>
  <div class="pos">
    <el-row>
      <!-- 点餐界面 -->
      <el-col :span="7" class="pos-order" id="order-list">
        <el-tabs type="border-card">
          <el-tab-pane label="点餐">
            <el-table
              :data="tableData"
              border
              style="width: 100%"
              max-height="400"
            >
              <el-table-column prop="goodsName" label="商品名称">
              </el-table-column>
              <el-table-column prop="count" label="数量" width="60">
              </el-table-column>
              <el-table-column prop="price" label="价格" width="70">
              </el-table-column>
              <el-table-column fixed="right" label="操作" width="100">
                <template slot-scope="scope">
                  <el-button
                    @click="addOrderList(scope.row)"
                    type="text"
                    size="small"
                    >增加</el-button
                  >
                  <el-button
                    @click="delSingleGood(scope.row)"
                    type="text"
                    size="small"
                    >删除</el-button
                  >
                </template>
              </el-table-column>
            </el-table>
            <div class="div-total">
              <small>数量：</small>{{ totalCount }}&nbsp;&nbsp;&nbsp;&nbsp;
              <small>金额：</small>{{ totalMoney }}&nbsp;<small>元</small>
            </div>
            <div class="div-btn">
              <el-button type="warning" size="small">挂单</el-button>
              <el-button type="danger" size="small" @click="delAllGoods()"
                >删除</el-button
              >
              <el-button type="success" size="small" @click="checkOut()"
                >结账</el-button
              >
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">挂单</el-tab-pane>
          <el-tab-pane label="外卖">外卖</el-tab-pane>
        </el-tabs>
      </el-col>
      <!--商品展示-->
      <el-col :span="17">
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li
                v-for="goods in oftenGoods"
                :key="goods.goodsId"
                @click="addOrderList(goods)"
              >
                <span>{{ goods.goodsName }}</span>
                <span class="o-price">￥{{ goods.price }}元</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goods-type">
          <el-tabs type="border-card">
            <el-tab-pane label="汉堡">
              <ul class="cookList">
                <li
                  v-for="goods in type0Goods"
                  :key="goods.goodsId"
                  @click="addOrderList(goods)"
                >
                  <span class="foodImg"
                    ><img :src="goods.goodsImg" width="100%"
                  /></span>
                  <span class="foodName">{{ goods.goodsName }}</span>
                  <span class="foodPrice">￥{{ goods.price }}元</span>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <ul class="cookList">
                <li
                  v-for="goods in type1Goods"
                  :key="goods.goodsId"
                  @click="addOrderList(goods)"
                >
                  <span class="foodImg"
                    ><img :src="goods.goodsImg" width="100%"
                  /></span>
                  <span class="foodName">{{ goods.goodsName }}</span>
                  <span class="foodPrice">￥{{ goods.price }}元</span>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <ul class="cookList">
                <li
                  v-for="goods in type2Goods"
                  :key="goods.goodsId"
                  @click="addOrderList(goods)"
                >
                  <span class="foodImg"
                    ><img :src="goods.goodsImg" width="100%"
                  /></span>
                  <span class="foodName">{{ goods.goodsName }}</span>
                  <span class="foodPrice">￥{{ goods.price }}元</span>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <ul class="cookList">
                <li
                  v-for="goods in type3Goods"
                  :key="goods.goodsId"
                  @click="addOrderList(goods)"
                >
                  <span class="foodImg"
                    ><img :src="goods.goodsImg" width="100%"
                  /></span>
                  <span class="foodName">{{ goods.goodsName }}</span>
                  <span class="foodPrice">￥{{ goods.price }}元</span>
                </li>
              </ul>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Pos',
  data() {
    return {
      tableData: [],
      oftenGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: []
    }
  },
  created() {
    // 读取常用商品
    axios
      .get(
        'https://www.fastmock.site/mock/0bf6a5bae7eab8507e44b56191ddff36/vuepos/oftenGoods'
      )
      .then((response) => {
        // console.log(response.data)
        this.oftenGoods = response.data.oftenGoods
      })
      .catch((error) => {
        console.log(error)
        alert('网络错误，不能访问')
      })
    // 读取分类商品列表
    axios
      .get(
        'https://www.fastmock.site/mock/0bf6a5bae7eab8507e44b56191ddff36/vuepos/typeGoods'
      )
      .then((response) => {
        // console.log(response.data)
        // this.oftenGoods=response.data;
        this.type0Goods = response.data.data[0]
        this.type1Goods = response.data.data[1]
        this.type2Goods = response.data.data[2]
        this.type3Goods = response.data.data[3]
      })
      .catch((error) => {
        console.log(error)
        alert('网络错误，不能访问')
      })
  },
  mounted() {
    const orderHeight = document.body.clientHeight
    document.getElementById('order-list').style.height = orderHeight + 'px'
  },
  // 计算属性的使用
  computed: {
    totalCount() {
      return this.tableData.reduce((pre, goods) => {
        return pre + goods.count
      }, 0)
    },
    totalMoney() {
      return this.tableData.reduce((pre, goods) => {
        return pre + goods.count * goods.price
      }, 0)
    }
  },
  methods: {
    // 添加商品
    addOrderList(goods) {
      // 商品是否存在于订单中
      let isHave = false
      for (const item of this.tableData) {
        if (item.goodsId === goods.goodsId) {
          isHave = true
        }
      }
      // 根据判断值处理业务逻辑
      if (isHave) {
        let arr = this.tableData.filter((obj) => obj.goodsId === goods.goodsId)
        arr[0].count++
      } else {
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        }
        this.tableData.push(newGoods)
      }
    },
    // 删除单个商品
    delSingleGood(good) {
      this.tableData = this.tableData.filter(
        (obj) => obj.goodsId !== good.goodsId
      )
    },
    // 删除全部商品
    delAllGoods() {
      this.tableData = []
      this.totalCount = 0
      this.totalMoney = 0
    },
    // 结账
    checkOut() {
      if (this.totalCount) {
        this.tableData = []
        this.totalCount = 0
        this.totalMoney = 0
        this.$message({
          message: '结账成功!',
          type: 'success'
        })
      } else {
        this.$message.error('账单不能为空！')
      }
    }
  }
}
</script>

<style scoped>
.pos-order {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
}

.div-total {
  background-color: #fff;
  padding: 10px;
  border-bottom: 1px solid #d3dce6;
}

.div-btn {
  margin-top: 8px;
}

.title {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
}
.often-goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 5px;
  background-color: #fff;
  cursor: pointer;
}
.o-price {
  color: #58b7ff;
}

.goods-type {
  clear: both;
}

.cookList li {
  list-style: none;
  width: 23%;
  border: 1px solid #e5e9f2;
  height: auto;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
  cursor: pointer;
}
.cookList li span {
  display: block;
  float: left;
}

.foodImg img {
  width: 80px;
  height: 80px;
  object-fit: cover;
}
.foodName {
  font-size: 18px;
  padding-left: 10px;
  color: brown;
}
.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
</style>
