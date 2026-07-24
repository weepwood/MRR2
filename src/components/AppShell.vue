<script setup lang="ts">
import {
  Activity,
  Archive,
  BarChart3,
  ChevronDown,
  CircleHelp,
  Database,
  FileSearch,
  FolderArchive,
  Menu,
  Moon,
  PanelLeftClose,
  PanelLeftOpen,
  Search,
  Settings,
  ShieldCheck,
  Sun,
  Users,
  X,
} from 'lucide-vue-next'
import { computed, ref } from 'vue'

export type PageKey = 'overview' | 'archive' | 'records' | 'statistics' | 'monitoring' | 'settings'

const props = defineProps<{
  activePage: PageKey
  darkMode: boolean
}>()

const emit = defineEmits<{
  navigate: [page: PageKey]
  toggleTheme: []
}>()

const sidebarCollapsed = ref(false)
const mobileOpen = ref(false)
const searchOpen = ref(false)

const groups = [
  {
    title: '工作台',
    items: [
      { key: 'overview' as const, label: '管理概览', icon: BarChart3 },
    ],
  },
  {
    title: '业务',
    items: [
      { key: 'archive' as const, label: '影像档案袋', icon: FolderArchive },
      { key: 'records' as const, label: '记录管理', icon: Database },
      { key: 'statistics' as const, label: '病案扫描统计', icon: Archive },
    ],
  },
  {
    title: '运维',
    items: [
      { key: 'monitoring' as const, label: '系统监控', icon: Activity },
      { key: 'settings' as const, label: '系统设置', icon: Settings },
    ],
  },
]

const pageName = computed(() => groups.flatMap(group => group.items).find(item => item.key === props.activePage)?.label ?? 'MRR2')

function navigate(page: PageKey) {
  emit('navigate', page)
  mobileOpen.value = false
}
</script>

<template>
  <div class="app-shell" :class="{ 'app-shell--collapsed': sidebarCollapsed }">
    <aside class="sidebar" :class="{ 'sidebar--mobile-open': mobileOpen }">
      <div class="brand">
        <div class="brand-mark">
          M
        </div>
        <div v-if="!sidebarCollapsed" class="brand-copy">
          <strong>MRR2</strong>
          <span>病案管理设计原型</span>
        </div>
        <button class="sidebar-close" type="button" aria-label="关闭导航" @click="mobileOpen = false">
          <X :size="18" />
        </button>
      </div>

      <div v-if="!sidebarCollapsed" class="environment">
        <span class="environment-dot" />
        <div>
          <strong>演示环境</strong>
          <span>Mock data · 无真实病案</span>
        </div>
      </div>

      <nav class="navigation" aria-label="主导航">
        <section v-for="group in groups" :key="group.title" class="nav-group">
          <p v-if="!sidebarCollapsed" class="nav-group-title">
            {{ group.title }}
          </p>
          <button
            v-for="item in group.items"
            :key="item.key"
            class="nav-item"
            :class="{ 'nav-item--active': activePage === item.key }"
            type="button"
            :title="sidebarCollapsed ? item.label : undefined"
            @click="navigate(item.key)"
          >
            <component :is="item.icon" :size="18" :stroke-width="1.8" />
            <span v-if="!sidebarCollapsed">{{ item.label }}</span>
          </button>
        </section>
      </nav>

      <div class="sidebar-footer">
        <button class="nav-item" type="button" :title="sidebarCollapsed ? '访问审计' : undefined">
          <ShieldCheck :size="18" :stroke-width="1.8" />
          <span v-if="!sidebarCollapsed">访问审计</span>
        </button>
        <button class="nav-item" type="button" :title="sidebarCollapsed ? '帮助与文档' : undefined">
          <CircleHelp :size="18" :stroke-width="1.8" />
          <span v-if="!sidebarCollapsed">帮助与文档</span>
        </button>
        <button class="collapse-button" type="button" @click="sidebarCollapsed = !sidebarCollapsed">
          <PanelLeftClose v-if="!sidebarCollapsed" :size="17" />
          <PanelLeftOpen v-else :size="17" />
          <span v-if="!sidebarCollapsed">收起侧栏</span>
        </button>
      </div>
    </aside>

    <div v-if="mobileOpen" class="sidebar-mask" @click="mobileOpen = false" />

    <div class="workspace">
      <header class="topbar">
        <div class="topbar-left">
          <button class="mobile-menu" type="button" aria-label="打开导航" @click="mobileOpen = true">
            <Menu :size="20" />
          </button>
          <div class="breadcrumb">
            <span>MRR</span>
            <span class="breadcrumb-separator">/</span>
            <strong>{{ pageName }}</strong>
          </div>
        </div>

        <div class="topbar-actions">
          <button class="search-trigger" type="button" @click="searchOpen = !searchOpen">
            <Search :size="16" />
            <span>搜索功能或病案</span>
            <kbd>⌘ K</kbd>
          </button>
          <button class="icon-button topbar-icon" type="button" :aria-label="darkMode ? '切换浅色模式' : '切换深色模式'" @click="emit('toggleTheme')">
            <Sun v-if="darkMode" :size="17" />
            <Moon v-else :size="17" />
          </button>
          <button class="profile" type="button">
            <span class="avatar">管</span>
            <span class="profile-copy">
              <strong>系统管理员</strong>
              <small>administrator</small>
            </span>
            <ChevronDown :size="15" />
          </button>
        </div>
      </header>

      <div v-if="searchOpen" class="command-panel surface">
        <Search :size="18" />
        <input autofocus placeholder="搜索病案号、患者、功能或设置..." @keyup.esc="searchOpen = false">
        <button type="button" @click="searchOpen = false">
          ESC
        </button>
        <div class="command-results">
          <span>快捷入口</span>
          <button type="button" @click="navigate('archive')">
            <FolderArchive :size="16" /> 影像档案袋
          </button>
          <button type="button" @click="navigate('records')">
            <FileSearch :size="16" /> 记录管理
          </button>
          <button type="button" @click="navigate('statistics')">
            <Users :size="16" /> 病案扫描统计
          </button>
        </div>
      </div>

      <main class="main-content">
        <div class="content-inner">
          <slot />
        </div>
      </main>
    </div>
  </div>
</template>

<style scoped>
.app-shell {
  min-height: 100vh;
}

.sidebar {
  position: fixed;
  z-index: 30;
  top: 0;
  bottom: 0;
  left: 0;
  display: flex;
  width: var(--sidebar-width);
  flex-direction: column;
  padding: 18px 14px;
  color: var(--sidebar-text);
  border-right: 1px solid var(--sidebar-border);
  background:
    radial-gradient(circle at 50% -10%, rgba(91, 108, 255, 0.20), transparent 30%),
    var(--sidebar-bg);
  transition: width 220ms ease, transform 220ms ease;
}

.app-shell--collapsed .sidebar {
  width: 74px;
}

.brand {
  display: flex;
  min-height: 42px;
  align-items: center;
  gap: 11px;
  padding: 0 7px;
}

.brand-mark {
  display: grid;
  width: 31px;
  height: 31px;
  flex: 0 0 31px;
  place-items: center;
  border: 1px solid rgba(255, 255, 255, 0.14);
  border-radius: 9px;
  background: linear-gradient(145deg, rgba(255, 255, 255, 0.14), rgba(91, 108, 255, 0.24));
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.15);
  font-size: 14px;
  font-weight: 760;
}

.brand-copy {
  display: grid;
  min-width: 0;
  gap: 2px;
}

.brand-copy strong {
  font-size: 14px;
  letter-spacing: 0.02em;
}

.brand-copy span {
  overflow: hidden;
  color: var(--sidebar-muted);
  font-size: 10px;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.sidebar-close {
  display: none;
  margin-left: auto;
  color: var(--sidebar-muted);
  border: 0;
  background: transparent;
}

.environment {
  display: flex;
  align-items: center;
  gap: 10px;
  margin: 20px 4px 12px;
  padding: 11px 12px;
  border: 1px solid var(--sidebar-border);
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.035);
}

.environment-dot {
  width: 8px;
  height: 8px;
  flex: 0 0 8px;
  border-radius: 50%;
  background: #59d3a4;
  box-shadow: 0 0 0 4px rgba(89, 211, 164, 0.11);
}

.environment div {
  display: grid;
  gap: 3px;
}

.environment strong {
  font-size: 11px;
  font-weight: 650;
}

.environment span:last-child {
  color: var(--sidebar-muted);
  font-size: 9px;
}

.navigation {
  min-height: 0;
  flex: 1;
  overflow-y: auto;
  scrollbar-width: none;
}

.nav-group {
  margin-top: 17px;
}

.nav-group-title {
  margin: 0 11px 7px;
  color: #666e7d;
  font-size: 9px;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
}

.nav-item,
.collapse-button {
  display: flex;
  width: 100%;
  min-height: 39px;
  align-items: center;
  gap: 11px;
  padding: 0 11px;
  color: var(--sidebar-muted);
  border: 1px solid transparent;
  border-radius: 8px;
  background: transparent;
  font-size: 12px;
  text-align: left;
  transition: color 160ms ease, border-color 160ms ease, background-color 160ms ease;
}

.app-shell--collapsed .nav-item,
.app-shell--collapsed .collapse-button {
  justify-content: center;
  padding: 0;
}

.nav-item:hover,
.collapse-button:hover {
  color: var(--sidebar-text);
  background: rgba(255, 255, 255, 0.055);
}

.nav-item--active {
  color: #fff;
  border-color: rgba(255, 255, 255, 0.08);
  background: linear-gradient(90deg, rgba(91, 108, 255, 0.22), rgba(91, 108, 255, 0.07));
  box-shadow: inset 2px 0 0 #8995ff;
}

.sidebar-footer {
  display: grid;
  gap: 2px;
  padding-top: 12px;
  border-top: 1px solid var(--sidebar-border);
}

.collapse-button {
  margin-top: 6px;
  border-top: 1px solid transparent;
}

.workspace {
  min-height: 100vh;
  margin-left: var(--sidebar-width);
  transition: margin-left 220ms ease;
}

.app-shell--collapsed .workspace {
  margin-left: 74px;
}

.topbar {
  position: sticky;
  z-index: 20;
  top: 0;
  display: flex;
  height: var(--topbar-height);
  align-items: center;
  justify-content: space-between;
  padding: 0 28px;
  border-bottom: 1px solid var(--border);
  background: rgba(246, 247, 249, 0.92);
  backdrop-filter: blur(14px);
}

:global([data-theme="dark"]) .topbar {
  background: rgba(13, 15, 20, 0.88);
}

.topbar-left,
.topbar-actions,
.profile,
.search-trigger,
.breadcrumb {
  display: flex;
  align-items: center;
}

.topbar-left,
.topbar-actions {
  gap: 10px;
}

.breadcrumb {
  gap: 8px;
  color: var(--text-tertiary);
  font-size: 12px;
}

.breadcrumb strong {
  color: var(--text);
  font-weight: 600;
}

.breadcrumb-separator {
  opacity: 0.45;
}

.mobile-menu {
  display: none;
  padding: 8px;
  color: var(--text);
  border: 0;
  background: transparent;
}

.search-trigger {
  min-width: 238px;
  min-height: 36px;
  gap: 8px;
  padding: 0 10px;
  color: var(--text-tertiary);
  border: 1px solid var(--border);
  border-radius: 8px;
  background: var(--surface);
  box-shadow: var(--shadow-sm);
  font-size: 11px;
}

.search-trigger span {
  flex: 1;
  text-align: left;
}

.search-trigger kbd {
  padding: 2px 5px;
  color: var(--text-tertiary);
  border: 1px solid var(--border);
  border-radius: 4px;
  background: var(--surface-soft);
  font-family: inherit;
  font-size: 9px;
}

.topbar-icon {
  width: 36px;
  height: 36px;
}

.profile {
  gap: 9px;
  padding: 3px 7px 3px 3px;
  border: 0;
  border-radius: 9px;
  background: transparent;
}

.profile:hover {
  background: var(--surface-hover);
}

.avatar {
  display: grid;
  width: 31px;
  height: 31px;
  place-items: center;
  color: #fff;
  border-radius: 8px;
  background: linear-gradient(135deg, #5869f2, #7e6be6);
  font-size: 11px;
  font-weight: 700;
}

.profile-copy {
  display: grid;
  gap: 1px;
  text-align: left;
}

.profile-copy strong {
  font-size: 11px;
  font-weight: 650;
}

.profile-copy small {
  color: var(--text-tertiary);
  font-size: 9px;
}

.main-content {
  padding: 28px;
}

.content-inner {
  width: 100%;
  max-width: var(--content-max);
  margin: 0 auto;
}

.command-panel {
  position: fixed;
  z-index: 50;
  top: 78px;
  left: 50%;
  display: grid;
  width: min(580px, calc(100vw - 32px));
  grid-template-columns: auto 1fr auto;
  align-items: center;
  gap: 10px;
  padding: 14px;
  transform: translateX(-50%);
  box-shadow: var(--shadow-md);
}

.command-panel > input {
  min-width: 0;
  color: var(--text);
  border: 0;
  outline: 0;
  background: transparent;
  font-size: 13px;
}

.command-panel > button {
  padding: 4px 6px;
  color: var(--text-tertiary);
  border: 1px solid var(--border);
  border-radius: 5px;
  background: var(--surface-soft);
  font-size: 9px;
}

.command-results {
  display: grid;
  grid-column: 1 / -1;
  gap: 4px;
  padding-top: 12px;
  border-top: 1px solid var(--border);
}

.command-results > span {
  padding: 4px 7px;
  color: var(--text-tertiary);
  font-size: 9px;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.command-results button {
  display: flex;
  align-items: center;
  gap: 9px;
  padding: 10px 9px;
  color: var(--text-secondary);
  border: 0;
  border-radius: 7px;
  background: transparent;
  text-align: left;
}

.command-results button:hover {
  color: var(--text);
  background: var(--surface-hover);
}

.sidebar-mask {
  display: none;
}

@media (max-width: 980px) {
  .sidebar {
    width: var(--sidebar-width);
    transform: translateX(-100%);
  }

  .sidebar--mobile-open {
    transform: translateX(0);
  }

  .app-shell--collapsed .sidebar {
    width: var(--sidebar-width);
  }

  .workspace,
  .app-shell--collapsed .workspace {
    margin-left: 0;
  }

  .mobile-menu,
  .sidebar-close {
    display: inline-flex;
  }

  .sidebar-mask {
    position: fixed;
    z-index: 25;
    inset: 0;
    display: block;
    background: rgba(0, 0, 0, 0.45);
  }
}

@media (max-width: 760px) {
  .topbar {
    padding: 0 16px;
  }

  .main-content {
    padding: 20px 16px;
  }

  .search-trigger {
    min-width: 36px;
    width: 36px;
    justify-content: center;
    padding: 0;
  }

  .search-trigger span,
  .search-trigger kbd,
  .profile-copy,
  .profile svg {
    display: none;
  }

  .profile {
    padding-right: 3px;
  }
}
</style>
