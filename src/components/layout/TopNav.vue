<template>
  <div class="topnav">
    <div class="tn-left">
      <el-button :icon="Fold" text size="large" class="toggle-btn" @click="$emit('toggle')" />
      <div class="quick-nav">
        <button v-for="m in quickMenus" :key="m.path" class="qn-btn" :class="{ active: isActive(m.path) }" @click="$router.push(m.path)">{{ m.title }}</button>
      </div>
    </div>
    <div class="tn-right">
      <el-dropdown trigger="click">
        <div class="user-chip"><el-avatar :size="30" icon="UserFilled" /><span class="uname">{{ roleLabel }}</span><el-icon><ArrowDown /></el-icon></div>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item @click="$router.push('/login')">退出登录</el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { useRoute } from 'vue-router'
import { Fold, ArrowDown } from '@element-plus/icons-vue'

defineProps<{ role: string }>()
defineEmits(['toggle'])
const route = useRoute()

const roleLabel = computed(() => {
  const r = route.path.split('/')[1]
  return { student: '学生端', teacher: '教师端', admin: '管理员端', leader: '领导端' }[r] || '未知'
})

const quickMenus = computed(() => {
  const base = `/${route.path.split('/')[1]}`
  const menus: { path: string; title: string }[] = [{ path: `${base}/home`, title: '首页' }]
  const role = route.path.split('/')[1]
  if (role === 'student') {
    menus.push({ path: `${base}/cultivation`, title: '培养' }, { path: `${base}/mentorship`, title: '导学' }, { path: `${base}/degree`, title: '学位' }, { path: `${base}/research-work`, title: '研工管理' }, { path: `${base}/others`, title: '其他' })
  }
  return menus
})

function isActive(path: string) { return route.path.startsWith(path) }
</script>

<style scoped>
.topnav{display:flex;justify-content:space-between;align-items:center;padding:0 20px;height:52px}
.tn-left{display:flex;align-items:center;gap:12px;overflow:hidden;flex:1}
.quick-nav{display:flex;gap:2px}
.qn-btn{padding:6px 16px;border:none;background:transparent;color:#1e293b;font-size:13px;cursor:pointer;border-radius:8px;font-family:inherit}
.qn-btn:hover{background:#f1f5f9}
.qn-btn.active{background:#eff6ff;color:#153D97;font-weight:600}
.user-chip{display:flex;align-items:center;gap:8px;padding:5px 8px;border-radius:12px;cursor:pointer}
.user-chip:hover{background:#f8fafc}
.uname{font-size:13px;font-weight:600;color:#1e293b}
</style>
