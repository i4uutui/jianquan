<template>
  <div class="content">
    <div class="lk_ddtitle">{{ body.title }}</div>
    <div class="lk_ssdes" v-html="body.text"></div>
  </div>
</template>

<script>
import { getArticlesDetail } from "@api/user";
export default {
  data: function() {
    return {
      body: ""
    };
  },
  mounted: function() {
    this.articleDetails();
  },
  methods: {
    articleDetails: function() {
      let that = this,
        id = this.$route.params.id;
      getArticlesDetail(id).then(res => {
        var w = res.data;
        that.body = w.details;
      });
    }
  }
};
</script>

<style>
.content {
  padding: 15px;
}

.lk_ddtitle {
  font-size: 20px;
  color: #333333;
}

.lk_ssdes {
  font-size: 14px;
  padding-top: 10px;
}
</style>
