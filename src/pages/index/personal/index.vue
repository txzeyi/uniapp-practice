<template>
  <view class="h-full">
    <view
      class="flex items-center bg-gradient-to-l to-primary-300 from-primary-500 px-4 h-45"
      @click="handleLogin"
    >
      <view class="overflow-hidden rounded-full w-20 h-20">
        <image :src="userInfo.avatar" alt="" class="w-full h-full" />
      </view>
      <view class="text-white text-xl ml-4">
        <view v-if="isLogin" class="">
          {{ userInfo.username }}
        </view>
        <view v-else>
          立即登录
        </view>
      </view>
    </view>
    <view class="">
      <view
        v-for="(item, index) of listModel"
        :key="index"
        class="flex items-center py-4 px-4 border-b border-gray-200 bg-white"
        @click="handleInfo(item)"
      >
        <view class="w-10 flex-none truncate text-primary-500 text-2xl">
          <via-icon :name="item.icon"></via-icon>
        </view>
        <view class="flex-1 w-0">
          {{ item.text }}
        </view>
        <view class="flex-none w-10 text-right text-gray-500">
          <via-icon name="arrow-right-bold"></via-icon>
        </view>
      </view>
    </view>
    <view v-if="isLogin" class="mt-8 px-4">
      <u-button type="success" plain ripple @click="handleLogout">
        退出登录
      </u-button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {}
  },
  computed: {
    isLogin() {
      return !!this.$store.user.token
    },
    listModel() {
      return [
        {
          icon: 'kefu',
          text: '联系我们',
          path: '/pages/contact/index',
        },
      ]
    },
    userInfo() {
      return this.$store.user.userInfo
    },
  },
  methods: {
    handleInfo(item) {
      this.$Router.push({
        path: item.path,
        query: item.query || {},
      })
    },
    handleLogin() {
      if (this.isLogin) {
        return
      }

      this.$Router.push({
        path: '/login',
      })
    },
    async handleLogout() {
      try {
        await this.$dialog('确定要退出登录吗?', {
          showCancelButton: true,
          confirmButtonText: '确定',
          cancelButtonText: '取消',
        })
      }
      catch (error) {
        console.log(error)
        return
      }

      await this.$store.user.logout()
      await this.$toast('退出登录成功', { type: 'success' })
    },
  },
}
</script>

<style></style>
