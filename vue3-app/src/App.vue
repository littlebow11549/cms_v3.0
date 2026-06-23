<template>
  <AppLayout
    :sidebar-collapsed="sidebarCollapsed"
    :mobile-sidebar-open="mobileSidebarOpen"
    @close-sidebar="mobileSidebarOpen = false"
  >
    <!-- 側邊欄 -->
    <template #sidebar>
      <Sidebar
        v-model:collapsed="sidebarCollapsed"
        v-model:activeCat="activeCat"
        :user="user"
        @sign-in="showSignIn = true"
        @support="showSupport = true"
      />
    </template>

    <!-- 頂部列 -->
    <template #header>
      <TopBar
        :user="user"
        :balance="balance"
        @sign-in="showSignIn = true"
        @logout="user = null"
        @home="activeCat = 'Lobby'"
        @navigate="(cat) => activeCat = cat"
      />
    </template>

    <!-- 頁面內容（placeholder，Phase 3+ 接入實際頁面） -->
    <div style="color: var(--text); padding: 24px 0; font-family: var(--font-display)">
      <h2 style="font-size: 24px; margin-bottom: 8px;">CMS_前台_v3 — Vue 3</h2>
      <p style="color: var(--text-mid)">Active: <strong>{{ activeCat }}</strong></p>
    </div>

    <!-- 頁尾 -->
    <template #footer>
      <AppFooter />
    </template>

    <!-- 手機底部導覽 -->
    <template #mobile-nav>
      <MobileNav
        :cat-tab="catTab"
        :mobile-sidebar-open="mobileSidebarOpen"
        @browse="mobileSidebarOpen = !mobileSidebarOpen"
        @close-sidebar="mobileSidebarOpen = false"
        @navigate="({ cat, tab }) => { activeCat = cat; if (tab) catTab = tab; }"
      />
    </template>
  </AppLayout>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import AppLayout   from '@/layouts/AppLayout.vue';
import Sidebar     from '@/components/layout/Sidebar.vue';
import TopBar      from '@/components/layout/TopBar.vue';
import AppFooter   from '@/components/layout/AppFooter.vue';
import MobileNav   from '@/components/layout/MobileNav.vue';

const sidebarCollapsed  = ref(false);
const mobileSidebarOpen = ref(false);
const activeCat         = ref('Lobby');
const catTab            = ref('Lobby');
const showSignIn        = ref(false);
const showSupport       = ref(false);
const balance           = ref(1284.32);
const user              = ref(null);

// 模擬餘額浮動
let balanceTimer;
onMounted(()    => { balanceTimer = setInterval(() => { balance.value = +(balance.value + (Math.random() * 4 - 1.7)).toFixed(2); }, 5500); });
onUnmounted(()  => clearInterval(balanceTimer));
</script>
