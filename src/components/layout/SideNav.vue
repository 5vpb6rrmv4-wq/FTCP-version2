<template>
  <div class="side-wrapper">
    <div class="logo-area"><el-icon :size="22" color="#fff"><School /></el-icon><span v-show="!collapsed" class="logo-text">人才选拔全流程平台</span></div>
    <el-menu :default-active="activeMenu" :collapse="collapsed" background-color="transparent" text-color="rgba(255,255,255,.7)" active-text-color="#fff" router class="side-menu">
      <el-menu-item v-for="item in menuItems" :key="item.path" :index="item.path">
        <el-icon><component :is="item.icon" /></el-icon>
        <template #title>{{ item.title }}</template>
      </el-menu-item>
    </el-menu>
    <div v-show="!collapsed" class="side-footer"><span class="ver-tag">FTCP v2.0</span></div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { useRoute } from 'vue-router'
defineProps<{ collapsed: boolean }>()

const route = useRoute()
const activeMenu = computed(() => {
  const segs = route.path.split('/').filter(Boolean)
  return '/' + segs.slice(0, 2).join('/')
})

const menuItems = computed(() => {
  const role = route.path.split('/')[1]
  const base = `/${role}`
  const items: { path: string; title: string; icon: string }[] = []
  if (role === 'student') {
    items.push({ path: `${base}/home`, title: '个人学业中心', icon: 'HomeFilled' })
    items.push({ path: `${base}/cultivation`, title: '2.1 培养', icon: 'Reading' })
    items.push({ path: `${base}/mentorship`, title: '2.2 导学', icon: 'Connection' })
    items.push({ path: `${base}/degree`, title: '2.3 学位', icon: 'Trophy' })
    items.push({ path: `${base}/research-work`, title: '2.4 研工管理', icon: 'Stamp' })
    items.push({ path: `${base}/others`, title: '2.5 其他', icon: 'Setting' })
  } else {
    items.push({ path: `${base}/home`, title: '首页', icon: 'HomeFilled' })
  }
  return items
})
</script>

<style scoped>
.side-wrapper{display:flex;flex-direction:column;height:100%}
.logo-area{display:flex;align-items:center;padding:16px 14px;gap:10px;height:60px;border-bottom:1px solid rgba(255,255,255,.1)}
.logo-text{color:#fff;font-size:14px;font-weight:700;white-space:nowrap}
.side-menu{border-right:none;flex:1;padding:8px 0}
.side-menu .el-menu-item{margin:3px 10px!important;border-radius:10px!important;height:44px;line-height:44px}
.side-menu .el-menu-item:hover{background:rgba(255,255,255,.1)!important}
.side-menu .el-menu-item.is-active{background:rgba(16,139,150,.3)!important;font-weight:600}
.side-footer{padding:12px 16px;border-top:1px solid rgba(255,255,255,.1)}
.ver-tag{font-size:10px;color:rgba(255,255,255,.3);background:rgba(255,255,255,.06);padding:2px 8px;border-radius:99px}
</style>
