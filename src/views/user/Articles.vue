<template>
  <div class="content">
    <div class="pd">
      <div class="lk-flexBox">
        <div class="lk_logo"><img src="@assets/images/logo3.png" alt="" /></div>
        <div class="lk_seach">
          <!-- <form action="javascript:return true" @submit.prevent="formSubmit"> -->
          <form action="javascript:return true">
            <!-- <input type="text" value="" placeholder="请输入你想要搜索的内容" v-model="txt" @keydown="search2($event)" /> -->
            <input
              type="text"
              value=""
              placeholder="请输入你想要搜索的内容"
              v-model="txt"
            />
          </form>
          <!-- </form> -->
        </div>
      </div>
    </div>

    <div class="lk_dis" v-if="state != 0">共 {{ count }} 条结果</div>
    <div class="no" v-if="state == 0">请在搜索框中输入关键词</div>
    <div class="no" v-else-if="state == 1">暂无内容</div>
    <div class="lk_content" v-else-if="state == 2">
      <router-link
        :to="{ path: '/Articles_detail/' + item.id }"
        class="lk_li"
        v-for="(item, i) in body"
        :key="i"
      >
        <div class="lk_title">{{ item.title }}</div>
        <div class="lk_des">{{ item.brief }}</div>
      </router-link>
    </div>
  </div>
</template>

<script>
import { getArticles } from "@api/user";
export default {
  data: function() {
    return {
      txt: "",
      state: 0,
      count: "",
      body: [],
      timeOut: ""
    };
  },
  watch: {
    txt(e) {
      var that = this;
      clearTimeout(this.timeOut);
      this.timeOut = setTimeout(function() {
        that.getArt();
      }, 500);
      console.log(e);
    }
  },
  mounted: function() {},
  methods: {
    // formSubmit () {
    // 	return false
    // },
    // search2(ev){
    // 	if(ev.keyCode == 13) {  //键盘回车的编码是13
    // 		this.getArt();
    // 	}
    // },
    getArt() {
      var that = this;
      if (that.txt) {
        getArticles(that.txt).then(res => {
          var w = res.data;
          if (w.count != 0) {
            that.state = 2;
          } else {
            that.state = 1;
          }
          that.count = w.count;
          that.body = w.info;
        });
      } else {
        that.state = 0;
        that.$dialog.toast({ mes: "请输入关键词" });
      }
    }
  }
};
</script>

<style>
.no {
  text-align: center;
  padding-top: 100px;
}
.lk_des {
  font-size: 12px;
  line-height: 20px;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 3;
  overflow: hidden;
}
.lk_li {
  padding: 10px 15px;
  border-bottom: 1px solid #c8c7cc;
  display: block;
}
.lk_li:first-child {
  border-bottom: 1px solid #c8c7cc;
}
.lk_title {
  padding-bottom: 10px;
  font-size: 14px;
}
.pd {
  padding: 0 15px;
}
.lk_dis {
  line-height: 25px;
  font-size: 13px;
  margin-top: 10px;
  background: #eee;
  padding-left: 15px;
}
.lk_seach input {
  width: 100%;
  font-size: 13px;
  background: #eee;
  line-height: 30px;
  height: 30px;
  padding-left: 10px;
}
.lk_seach {
  flex: 1;
  padding-left: 10px;
}
.lk_logo {
  width: 30px;
}
.lk_logo img {
  width: 100%;
}
.content {
  padding-top: 10px;
}
</style>
