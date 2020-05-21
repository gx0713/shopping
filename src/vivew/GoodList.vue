<template>
  <div>
<!-- 遮罩层   -->
     <div class="mask_layer" id="mask_layer" v-show="price_mask_isshow"></div>
    <Header/>
    <NavBread/>
      <!-- 登录 -->
      <!-- <div class="md-modal modal-msg md-modal-transition">
        <div class="md-modal-inner">
          <div class="md-top">
            <div class="md-title">Login in</div>
            <button class="md-close">Close</button>
          </div>
          <div class="md-content">
            <div class="confirm-tips">
              <div class="error-wrap">
                <span class="error error-show" v-show="errorTip">用户名或者密码错误</span>
              </div>
              <ul>
                <li class="regi_form_input">
                  <i class="icon IconPeople"></i>
                  <input
                    type="text"
                    tabindex="1"
                    name="loginname"
                    v-model="userName"
                    class="regi_login_input regi_login_input_left"
                    placeholder="User Name"
                    data-type="loginname"
                  />
                </li>
                <li class="regi_form_input noMargin">
                  <i class="icon IconPwd"></i>
                  <input
                    type="password"
                    tabindex="2"
                    name="password"
                    class="regi_login_input regi_login_input_left login-input-no input_text"
                    placeholder="Password"
                  />
                </li>
              </ul>
            </div>
            <div class="login-wrap">
              <a href="javascript:;" class="btn-login">登 录</a>
            </div>
          </div>
        </div> -->
      <!-- </div> -->

      <!-- <div class="md-overlay"></div> -->


    <div class="accessory-result-page accessory-page">
      <div class="container">
        <div class="filter-nav">
          <span class="sortby">Sort by:</span>
          <a href="javascript:void(0)" class="default cur">Default</a>
          <a href="javascript:void(0)" class="price">
            Price
            <svg class="icon icon-arrow-short">
              <use xlink:href="#icon-arrow-short" />
            </svg>
          </a>
          <a href="javascript:void(0)" class="filterby stopPop" @click="price_menu">Filter by</a>
        </div>
        <div class="accessory-result">
          <!-- filter -->
          <div class="filter stopPop" id="filter" v-bind:class="{'filterby-show':price_menu_isshow}">
            <dl class="filter-price">
              <dt>Price:</dt>
              <dd>
                <a href="javascript:void(0)" v-bind:class="{ 'cur':priceState_all=='active' }" @click="priceState_all_status_all">All</a>
              </dd>
              <dd v-for="(item,index) in priceFilter">
                <a href="javascript:void(0)" @click="priceState_all_ser(index)" v-bind:class="{ 'cur':priceState_all==index }">{{ item.startPrice }}--{{ item.endPrice }}</a>
              </dd>
            </dl>
          </div>
            <!-- 遮罩层 -->
          <!-- search result accessories list -->
          <div class="accessory-list-wrap">
            <div class="accessory-list col-4">
              <ul>
                <li v-for="(item,index) in goodList">
                  <div class="pic">
                    <a href="#">
                      <img v-bind:src="'/static/'+item.productImg" alt />
                    </a>
                  </div>
                  <div class="main">
                    <div class="name">{{ item.productName }}</div>
                    <div class="price">{{ item.productPrice }}</div>
                    <div class="btn-area">
                      <a href="javascript:;" class="btn btn--m">加入购物车</a>
                    </div>
                  </div>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
    <Footer/>
  </div>
</template>
<script>
import "@/assets/css/base.css";
import "@/assets/css/checkout.css";
import "@/assets/css/login.css";
import "@/assets/css/product.css";
import Header from "../components/Header";
import Footer from "../components/Footer";
import NavBread from "../components/NavBread";
import axios from 'axios';
export default {
  data(){
    return{
      goodList:[],
      priceState_all:['active'],
      priceFilter:[
        {startPrice:'0.00', endPrice:'100.00' },
        {startPrice:'100.00', endPrice:'1000.00' },
        {startPrice:'1000.00', endPrice:'10000.00' },
        {startPrice:'10000.00', endPrice:'20000.00' },
      ],
      price_mask_isshow:false,
      price_menu_isshow:false,
    }
  },
  components:{
    Header,
    Footer,
    NavBread
  },
  mounted() {
    this.getGoodList()
  },
  methods:{
    price_menu(){
      this.price_mask_isshow=true,
      this.price_menu_isshow=true
    },
    priceState_all_status_all(){
      this.priceState_all='active',
      this.price_mask_isshow=false,
      this.price_menu_isshow=false
    },
    priceState_all_ser(index){
      this.priceState_all=index,
      this.price_mask_isshow=false,
      this.price_menu_isshow=false
    },
    getGoodList(){
      axios.get("/api/goods",{}).then(
        res=> {
          console.log(res.data.goodlist.result)
          this.goodList = res.data.goodlist.result
        }
      )
    }
  }
};
</script>
<style>
  .mask_layer{width: 100vw;height: 100vh;background: rgba(0,0,0,0.3);z-index: 1;position: fixed}
</style>
