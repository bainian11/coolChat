<template>
  <div id="app">
    <div id="wrap">
      <!-- 认证 -->
      <div v-if="information.attestCode==1" class="config">已认证</div>
      <div v-else class="selfconfig">本人认证</div>
      <!-- 头像 -->
      <header>
        {{information.nickname}}
        <div class="sex girl" v-if="information.sex==='女'">♀{{ information.age}}</div>
        <div class="sex boy" v-else>♂{{ information.age}}</div>
        <div class="myid">ID:{{information.userId}}</div>
        <img :src="this.dizhi" alt />
        <p class="sign">签名：{{information.signature}}</p>
        <ul>
          <li>
            <span class="follow">关注</span>
            {{information.caresNum}}
          </li>
          <li @click="fans(information.userId)">
            <span class="follow">粉丝</span>
            {{information.fansNum}}
          </li>
          <li>
            <span class="follow">访问</span>
            {{information.visitNum}}
          </li>
        </ul>
      </header>
      <!-- 视频 -->
      <section class="video">
        <div class="video-left">
          <h6>视频聊天</h6>
          <van-icon name="arrow" class="enter-video" />
        </div>
        <div class="video-right">
          <template>
            <span class="switch" v-if="checked">已开启</span>
            <span class="switch" v-else>已关闭</span>
          </template>
          <!-- {{checked}} -->
          <van-switch v-model="checked" active-color="#07c160" size="24px" />
        </div>
      </section>
      <!-- 我的账户 -->
      <section class="wrap-account">
        <div class="account-inner account" @click="Account(information.gold)">
          <van-icon name="peer-pay" />
          <div>我的账户：{{information.gold}}</div>
        </div>
        <div class="account-inner coin">
          <van-icon name="points" />
          <div>我的金币：{{information.gold}}</div>
        </div>
      </section>
      <!-- 礼物 -->
      <section class="mygift">
        <div class="mygift-header">
          <h6>我的礼物</h6>
          <van-icon name="arrow" class="enter-video" />
        </div>
        <div class="getgift">
          <div class="gift-inner receive">
            <van-icon name="ascending" class="font-gift" />
            <h6>收到的礼物：{{information.sendGift}}</h6>
          </div>
          <div class="gift-inner sended">
            <van-icon name="descending" class="font-gift" />
            <h6>送出的礼物：{{information.getGift}}</h6>
          </div>
        </div>
      </section>
      <!-- 小件导航 -->
      <footer>
        <div class="part">
          <div>
            <van-icon name="add-square" />
            <p>账号安全</p>
          </div>
          <div @click="upphoto">
            <van-icon name="photo" />
            <p>个人相册</p>
          </div>
          <div class="last" @click="Black">
            <van-icon name="clear" />
            <p>黑名单</p>
          </div>
        </div>
        <div class="part">
          <div @click="data">
            <van-icon name="manager" />
            <p>编辑资料</p>
          </div>
          <div>
            <van-icon name="question" />
            <p>常见问题</p>
          </div>
          <div class="last" @click="set">
            <van-icon name="setting" />
            <p>设置</p>
          </div>
        </div>
      </footer>
    </div>
  </div>
</template>
<script>
export default {
  name: "Geren",
  data() {
    return {
      checked: true,
      information: [
        {
          id: 1,
          attestCode: 0,
          nickname: "momomo",
          userId: 2589484,
          sex: "女",
          age: 26,
          signature: "大碗宽面哈哈哈哈哈哈哈哈哈哈哈",
          pic:
            "http://pic4.zhimg.com/50/v2-a763daa0fd9dd56c6d4b6412cac68211_hd.jpg",
          caresNum: 26,
          fansNum: 26,
          visitNum: 26,
          myaccount: 22,
          gold: 23,
          sendGift: 26,
          getGift: 26
        }
      ],
      account: [{ id: 1 }],
      dizhi: ""
    };
  },
  methods: {
    change(ev) {
      // console.log(ev.targrt.checked);
    },
    Black() {
      this.$router.push({ path: "/Black" });
      // console.log(this.$router)
    },
    upphoto() {
      this.$router.push({ path: "/Upphoto" });
    },
    set() {
      this.$router.push({ path: "/Set" });
    },
    Account(coin) {
      this.$router.push({ path: "/Account" ,query:{coin:coin}});
    },
    // 粉丝
    fans(userId){
      this.$router.push({path:"/fans",query:{userId:userId}})
    },
    data() {
      this.$router.push({ path: "/Data" });
    }
  },
  created() {
    // console.log(this.$route);
    let _this = this;
    this.axios
      .get("/mine", {
        params: { userId: this.$store.state.userId }
      })
      .then(result => {
        _this.dizhi = "http://139.9.116.201:8888/" + result.data.data.headImage;
        _this.information = result.data.data;
      });
  }
};
</script>
<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  margin: 0 15px;
}
.last {
  width: 52px;
}
.config {
  width: 65px;
  height: 25px;
  background: #cccccc;
  border-radius: 10px;
  margin: 15px 0;
  font-size: 12px;
  color: #1a1a1a;
  line-height: 25px;
  text-align: center;
}
.selfconfig {
  width: 65px;
  height: 25px;
  background: linear-gradient(to right, #fe4c68, #ff8a5f);
  border-radius: 10px;
  margin: 15px 0;
  font-size: 12px;
  color: #ffffff;
  line-height: 25px;
  text-align: center;
}
header {
  position: relative;
  border-bottom: 0.5px solid #e6e6e6;
  /* float: left; */
  text-align: left;
}
header .sex {
  width: 30px;
  height: 15px;
  display: inline-block;
  border-radius: 3px;
  color: #fff;
  font-size: 8px;
  line-height: 15px;
  text-align: center;
}
header .girl {
  background: #ff5277;
}
header .boy {
  background: #2b6cdb;
}
header .myid {
  font-size: 11px;
  color: #999999;
}
header img {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  position: absolute;
  right: 2px;
  top: 0;
}
header .sign {
  width: 213px;
  font-size: 13px;
  color: #1a1a1a;
  line-height: 22px;
}
header ul {
  width: 213px;
  margin-top: 15px;
  margin-bottom: 15px;
  display: flex;
  justify-content: space-between;
}
header ul li {
  font-size: 13px;
  color: #1a1a1a;
}
header ul li .follow {
  color: #999999;
}
/* 视频 */
.video {
  height: 65px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 13px;
  color: #1a1a1a;
  position: relative;
  border-bottom: 0.5px solid #e6e6e6;
}
.video .enter-video {
  position: absolute;
  left: 62px;
  top: 25px;
  color: #b3b3b3;
  font-weight: 900;
}
.switch {
  display: inline-block;
  margin-right: 10px;
  position: relative;
  top: -10px;
}
/* 我的账户 */
.wrap-account {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 20px 0;
  color: #fff;
  font-size: 13px;
}
.wrap-account .account-inner {
  width: 165px;
  height: 65px;
  border-radius: 3px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.wrap-account .van-icon {
  font-size: 20px;
  margin-right: 6px;
}
.wrap-account .account {
  background: linear-gradient(to right, #fe4c68, #ff8a5f);
}
.wrap-account .coin {
  background: linear-gradient(to right, #2c6ddc, #32a9ff);
}
/* 礼物 */
.mygift-header {
  height: 45px;
  border-top: 0.5px solid #e6e6e6;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 13px;
  color: #1a1a1a;
}
.getgift {
  height: 55px;
  border-radius: 4px;
  display: flex;
  justify-content: space-around;
  align-items: center;
  background: #f2f2f2;
}
.getgift .gift-inner {
  display: flex;
  justify-content: space-around;
  align-items: center;
  font-size: 13px;
  color: #1a1a1a;
}
.getgift .font-gift {
  font-size: 25px;
  margin-right: 5px;
}
footer {
  font-size: 13px;
  color: #999999;
  margin-top: 20px;
  border-top: 0.5px solid #e6e6e6;
}
.van-icon-add-square {
  color: #2c6fdd;
}
.van-icon-photo {
  color: #fb894f;
}
.van-icon-clear {
  color: #12af8b;
}
.van-icon-setting {
  color: #fb864f;
}
.van-icon-manager {
  color: #7542c4;
}
.van-icon-question {
  color: #fa3e60;
}
footer .part > div {
  display: flex;
  flex-direction: column;
  align-items: center;
}
footer .van-icon {
  font-size: 30px;
  margin-bottom: 3px;
}
.part {
  display: flex;
  justify-content: space-around;
  align-items: center;
  align-content: center;
  margin-top: 20px;
}
</style>