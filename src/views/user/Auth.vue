<template>
  <div></div>
</template>

<script>
import { auth } from "@libs/wechat";
export default {
  mounted() {
    console.log(this.$route);
    const { code, state } = this.$route.query;
    auth(code, state)
      .then(() => {
        // location.replace(
        //   decodeURIComponent(decodeURIComponent(this.$route.params.url))
        // );
        location.href = decodeURIComponent(
          decodeURIComponent(this.$route.params.url)
        );
        console.log(location);
      })
      .catch(() => {
        location.replace("/");
      });
  }
};
</script>

<style></style>
