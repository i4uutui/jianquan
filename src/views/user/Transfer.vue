<template>
  <div class="ChangePassword">
    <div class="list">
      <div class="item">
        <input type="number" placeholder="转入账户UID" v-model="uid" />
      </div>
      <div class="item">
        <input type="text" placeholder="转入账户手机号码" v-model="phone" />
      </div>
      <div class="item acea-row row-between-wrapper">
        <input
          type="number"
          placeholder="转入金额"
          class="codeIput"
          v-model="price"
        />
      </div>
    </div>
    <div class="confirmBnt bg-color-red" @click="confirm">确定</div>
    <div class="userinfo" :hidden="userinfostaus">
      <p>转账信息</p>
      <div class="list">
        <div class="item">
          <span>uid:</span>
          <input
            type="number"
            placeholder="转入账户UID"
            v-model="uid"
            disabled="disabled"
          />
        </div>
        <div class="item">
          <span>手机:</span>
          <input type="text" v-model="phone" disabled="disabled" />
        </div>
        <div class="item">
          <span>名称:</span>
          <input type="text" v-model="nickname" disabled="disabled" />
          <img :src="avatar" class="avatar" />
        </div>
        <div class="item ">
          <span>金额:</span>
          <input class="codeIput" v-model="price" disabled="disabled" />
        </div>
        <p>请确认待转入账号信息是否正确</p>
        <div class="confirmBnt bg-color-red" @click="transfer">确认并转入</div>
        <div class="confirmBnt bg-color-default" @click="userinfostaus = true">
          取消
        </div>
      </div>
    </div>
    <div class="zc" :hidden="userinfostaus"></div>
  </div>
</template>
<script>
import { getUidUserInfo, fromTransfer } from "@api/user";
export default {
  name: "Transfer",
  data: function() {
    return {
      uid: "", //用户
      price: "", //金额,
      phone: "",
      userinfostaus: true,
      avatar: "",
      nickname: ""
    };
  },
  mounted: function() {},
  methods: {
    async confirm() {
      this.getuser();
    },
    getuser: function() {
      console.log(123);
      let that = this;
      getUidUserInfo(that.uid, that.phone).then(
        res => {
          if (res.status == 200) {
            that.avatar = res.data.avatar;
            that.nickname = res.data.nickname;
            that.userinfostaus = false;
          } else {
            that.$dialog.message("用户信息不存在");
          }
        },
        error => {
          that.$dialog.message(error.msg);
        }
      );
    },
    transfer: function() {
      let that = this;
      fromTransfer(that.uid, that.price).then(
        res => {
          if (res.status == 200) {
            that.$dialog.message("转账成功");
            //调到账户
            this.$router.replace({ path: "/user/bill/5" });
          } else {
            that.$dialog.message("用户信息不存在");
          }
        },
        error => {
          that.$dialog.message(error.msg);
        }
      );
    }
  }
};
</script>
<style>
.userinfo p {
  text-align: center;
}
.userinfo {
  padding: 0.6rem;
  position: fixed;
  z-index: 120;
  background: #fff;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) !important;
}
.userinfo p {
  margin-top: 0.5rem;
}
.userinfo .list .item {
  display: flex;
  line-height: 1.1rem;
}
.userinfo .list .item span {
  width: 1rem;
}
.userinfo .list .item input {
  flex: 1;
}
.userinfo .confirmBnt {
  margin-top: 0.5rem !important;
}
.avatar {
  width: 1rem;
  height: 1rem;
  border-radius: 50%;
}
.zc {
  position: fixed;
  width: 100%;
  height: 100%;
  background: #000000;
  opacity: 0.4;
  top: 0;
  left: 0;
  z-index: 100;
}
.bg-color-default {
  background: #00aaea;
}
</style>
