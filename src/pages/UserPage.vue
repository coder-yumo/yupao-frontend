<template>
  <div v-if="user">
    <van-cell title="当前用户" :value="user.userAccount"/>
    <van-cell title="修改信息" is-link to="/user/update"/>
    <van-cell title="我的好友" is-link to="/user/my/friend"/>
    <van-cell title="我加入的队伍" is-link to="/team/my/join"/>
    <van-cell title="我创建的队伍" is-link to="/team/my/create"/>
    <van-cell title="我的标签"/>
    <van-row gutter="16" v-if="user.tags.length > 0" >
      <van-col v-for="tag in user.tags" style="padding: 15px">
        <van-tag type="success" size="large">{{ tag }}</van-tag>
      </van-col>
    </van-row>
  </div>

</template>

<script setup lang="ts">
import {useRouter} from "vue-router";
import {onMounted, ref} from "vue";
import myAxios from "../plugins/myAxios";
import {showFailToast, showSuccessToast} from "vant";
import {getCurrentUser} from "../services/user.ts";

const router = useRouter();
const user = ref();

onMounted(async () => {
  const token = localStorage.getItem("token");
  if (token == null) {
    window.location.href='/user/login'
  }
      const res = await getCurrentUser();
      if (res.code === 0) {
        if (res.data.tags) {
          res.data.tags = JSON.parse(res.data.tags)
        }
        user.value = res.data;
        console.log(res.data.tags);
      } else {
        showFailToast('获取用户信息失败')
        await router.push('/user/login')
      }
    }
)

const toEdit = (editKey: string, editName: string, currentValue: string) => {
  router.push({
    path: '/user/edit',
    query: {
      editKey,
      editName,
      currentValue
    }
  })
}
</script>

<style scoped>

</style>