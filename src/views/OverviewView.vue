<script setup lang="ts">
import {
  Activity,
  ArrowRight,
  CheckCircle2,
  Clock3,
  Database,
  FolderArchive,
  HardDrive,
  ScanLine,
  Server,
  ShieldCheck,
  Sparkles,
  TrendingUp,
} from 'lucide-vue-next'
import type { PageKey } from '../components/AppShell.vue'

const emit = defineEmits<{
  navigate: [page: PageKey]
}>()

const metrics = [
  { label: '病案记录', value: '206,482', change: '+1,248 本月', icon: Database, tone: 'default' },
  { label: '影像文件', value: '30.24M', change: '+82,310 本周', icon: FolderArchive, tone: 'blue' },
  { label: '今日扫描', value: '1,864', change: '完成率 96.8%', icon: ScanLine, tone: 'green' },
  { label: '待处理异常', value: '17', change: '较昨日减少 6', icon: Activity, tone: 'orange' },
]

const recentRecords = [
  { bah: '00092318', name: '示例患者 A', department: '心血管内科', pages: 86, status: '已归档', time: '14:32' },
  { bah: '00092317', name: '示例患者 B', department: '神经内科', pages: 142, status: '待复核', time: '14:19' },
  { bah: '00092316', name: '示例患者 C', department: '骨科', pages: 64, status: '已归档', time: '13:58' },
  { bah: '00092315', name: '示例患者 D', department: '呼吸内科', pages: 108, status: '扫描中', time: '13:41' },
  { bah: '00092314', name: '示例患者 E', department: '普外科', pages: 73, status: '已归档', time: '13:26' },
]

const services = [
  { name: 'MRR 后端服务', detail: 'Spring Boot · :8002', value: '正常', latency: '23 ms' },
  { name: 'PostgreSQL', detail: 'mrr-app · 连接池 18/40', value: '正常', latency: '11 ms' },
  { name: '影像存储', detail: '本地/NAS · 可用 8.4 TB', value: '正常', latency: '36 ms' },
]
</script>

<template>
  <div class="page overview">
    <section class="hero">
      <div class="hero-grid" />
      <div class="hero-glow hero-glow--one" />
      <div class="hero-glow hero-glow--two" />
      <div class="hero-content">
        <div class="hero-eyebrow">
          <Sparkles :size="14" />
          Vercel 的秩序 · Linear 的质感
        </div>
        <h1>让复杂的病案管理，<br>看起来依然清晰。</h1>
        <p>MRR2 是独立的视觉验证项目。它保留 MRR 的业务结构，用细边框、稳定网格与克制光感重新组织工作界面。</p>
        <div class="hero-actions">
          <button class="button button--primary" type="button" @click="emit('navigate', 'archive')">
            打开影像档案袋
            <ArrowRight :size="15" />
          </button>
          <button class="button hero-secondary" type="button" @click="emit('navigate', 'settings')">
            查看设计规范
          </button>
        </div>
      </div>
      <div class="hero-status">
        <div class="hero-status-head">
          <span>系统实时状态</span>
          <span class="badge badge--success"><CheckCircle2 :size="12" /> 全部正常</span>
        </div>
        <div class="signal-chart" aria-label="系统运行趋势示意图">
          <span v-for="height in [34, 42, 38, 58, 47, 61, 54, 70, 65, 81, 73, 88, 78, 91, 84, 94]" :key="height" :style="{ height: `${height}%` }" />
        </div>
        <div class="hero-status-foot">
          <div>
            <strong>99.98%</strong>
            <span>近 30 日可用性</span>
          </div>
          <div>
            <strong>31 ms</strong>
            <span>平均响应时间</span>
          </div>
        </div>
      </div>
    </section>

    <section class="metrics-grid">
      <article v-for="metric in metrics" :key="metric.label" class="metric-card surface" :class="`metric-card--${metric.tone}`">
        <div class="metric-icon">
          <component :is="metric.icon" :size="18" :stroke-width="1.8" />
        </div>
        <span>{{ metric.label }}</span>
        <strong>{{ metric.value }}</strong>
        <small><TrendingUp :size="12" /> {{ metric.change }}</small>
      </article>
    </section>

    <section class="overview-grid">
      <article class="surface activity-panel">
        <div class="panel-heading">
          <div>
            <h2 class="section-title">扫描活动</h2>
            <p class="section-description">最近 7 日扫描页数与归档完成情况</p>
          </div>
          <span class="badge">最近 7 日</span>
        </div>
        <div class="chart-area">
          <div class="chart-y-axis">
            <span>12k</span><span>8k</span><span>4k</span><span>0</span>
          </div>
          <div class="bar-chart">
            <div v-for="day in [{ d: '周一', a: 46, b: 60 }, { d: '周二', a: 58, b: 72 }, { d: '周三', a: 51, b: 66 }, { d: '周四', a: 70, b: 82 }, { d: '周五', a: 63, b: 76 }, { d: '周六', a: 38, b: 49 }, { d: '今日', a: 72, b: 90 }]" :key="day.d" class="bar-column">
              <div class="bar-track">
                <span class="bar bar--secondary" :style="{ height: `${day.b}%` }" />
                <span class="bar bar--primary" :style="{ height: `${day.a}%` }" />
              </div>
              <small>{{ day.d }}</small>
            </div>
          </div>
        </div>
        <div class="chart-legend">
          <span><i class="legend-dot legend-dot--primary" /> 已归档</span>
          <span><i class="legend-dot legend-dot--secondary" /> 扫描总量</span>
        </div>
      </article>

      <article class="surface health-panel">
        <div class="panel-heading">
          <div>
            <h2 class="section-title">基础设施</h2>
            <p class="section-description">关键依赖的运行状态</p>
          </div>
          <button class="text-button" type="button" @click="emit('navigate', 'monitoring')">
            查看监控 <ArrowRight :size="13" />
          </button>
        </div>
        <div class="service-list">
          <div v-for="(service, index) in services" :key="service.name" class="service-row">
            <div class="service-icon">
              <Server v-if="index === 0" :size="17" />
              <Database v-else-if="index === 1" :size="17" />
              <HardDrive v-else :size="17" />
            </div>
            <div class="service-copy">
              <strong>{{ service.name }}</strong>
              <span>{{ service.detail }}</span>
            </div>
            <div class="service-meta">
              <span class="service-state"><i />{{ service.value }}</span>
              <small>{{ service.latency }}</small>
            </div>
          </div>
        </div>
        <div class="security-note">
          <ShieldCheck :size="17" />
          <div>
            <strong>安全策略已启用</strong>
            <span>登录鉴权、访问审计与图片权限检查均正常工作。</span>
          </div>
        </div>
      </article>
    </section>

    <section class="surface records-panel">
      <div class="panel-heading records-heading">
        <div>
          <h2 class="section-title">最近处理的病案</h2>
          <p class="section-description">仅展示 Mock 数据，用于验证表格密度与视觉层级</p>
        </div>
        <button class="button" type="button" @click="emit('navigate', 'records')">
          查看全部记录 <ArrowRight :size="14" />
        </button>
      </div>
      <div class="table-scroll">
        <table class="data-table records-table">
          <thead>
            <tr><th>病案号</th><th>患者</th><th>出院科室</th><th>影像页数</th><th>状态</th><th>更新时间</th></tr>
          </thead>
          <tbody>
            <tr v-for="record in recentRecords" :key="record.bah">
              <td class="mono record-id">{{ record.bah }}</td>
              <td>{{ record.name }}</td>
              <td class="muted">{{ record.department }}</td>
              <td>{{ record.pages }} 页</td>
              <td>
                <span class="badge" :class="{ 'badge--success': record.status === '已归档', 'badge--warning': record.status !== '已归档' }">
                  <Clock3 v-if="record.status !== '已归档'" :size="11" />
                  <CheckCircle2 v-else :size="11" />
                  {{ record.status }}
                </span>
              </td>
              <td class="muted">今天 {{ record.time }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>
  </div>
</template>

<style scoped>
.overview {
  gap: 20px;
}

.hero {
  position: relative;
  display: grid;
  min-height: 348px;
  grid-template-columns: minmax(0, 1.45fr) minmax(320px, 0.75fr);
  gap: 32px;
  align-items: center;
  overflow: hidden;
  padding: clamp(32px, 5vw, 64px);
  color: #f7f7f8;
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 18px;
  background: #0c0f15;
  box-shadow: 0 24px 60px rgba(9, 11, 16, 0.15);
}

.hero-grid {
  position: absolute;
  inset: 0;
  opacity: 0.22;
  background-image:
    linear-gradient(rgba(255, 255, 255, 0.05) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255, 255, 255, 0.05) 1px, transparent 1px);
  background-size: 48px 48px;
  mask-image: linear-gradient(to bottom, black, transparent 92%);
}

.hero-glow {
  position: absolute;
  width: 380px;
  height: 380px;
  border-radius: 50%;
  filter: blur(90px);
  pointer-events: none;
}

.hero-glow--one {
  top: -260px;
  left: 22%;
  background: rgba(91, 108, 255, 0.42);
}

.hero-glow--two {
  right: -180px;
  bottom: -300px;
  background: rgba(122, 81, 211, 0.28);
}

.hero-content,
.hero-status {
  position: relative;
  z-index: 2;
}

.hero-eyebrow {
  display: inline-flex;
  align-items: center;
  gap: 7px;
  margin-bottom: 18px;
  color: #aeb6ff;
  font-size: 11px;
  font-weight: 650;
  letter-spacing: 0.05em;
}

.hero h1 {
  max-width: 720px;
  font-size: clamp(36px, 4.4vw, 64px);
  font-weight: 660;
  line-height: 1.04;
  letter-spacing: -0.055em;
}

.hero-content > p {
  max-width: 660px;
  margin-top: 20px;
  color: #a7adba;
  font-size: 14px;
  line-height: 1.8;
}

.hero-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 28px;
}

.hero-secondary {
  color: #e5e7eb;
  border-color: rgba(255, 255, 255, 0.12);
  background: rgba(255, 255, 255, 0.06);
  box-shadow: none;
}

.hero-secondary:hover {
  color: #fff;
  border-color: rgba(255, 255, 255, 0.2);
  background: rgba(255, 255, 255, 0.1);
}

.hero-status {
  align-self: stretch;
  padding: 20px;
  border: 1px solid rgba(255, 255, 255, 0.09);
  border-radius: 14px;
  background: rgba(255, 255, 255, 0.045);
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(14px);
}

.hero-status-head,
.hero-status-foot {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.hero-status-head > span:first-child {
  color: #c9ced8;
  font-size: 11px;
  font-weight: 650;
}

.signal-chart {
  display: flex;
  height: 142px;
  align-items: flex-end;
  gap: 7px;
  margin: 24px 0;
  padding: 0 3px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
}

.signal-chart span {
  flex: 1;
  min-width: 4px;
  border-radius: 3px 3px 0 0;
  background: linear-gradient(to top, rgba(91, 108, 255, 0.34), #8d98ff);
  box-shadow: 0 0 18px rgba(91, 108, 255, 0.16);
}

.hero-status-foot {
  gap: 20px;
}

.hero-status-foot div {
  display: grid;
  gap: 4px;
}

.hero-status-foot strong {
  font-size: 19px;
  font-weight: 640;
  letter-spacing: -0.03em;
}

.hero-status-foot span {
  color: #888f9d;
  font-size: 9px;
}

.metrics-grid {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  gap: 14px;
}

.metric-card {
  position: relative;
  display: grid;
  min-height: 138px;
  grid-template-columns: 38px 1fr;
  grid-template-rows: 30px auto auto;
  align-items: center;
  padding: 18px;
  overflow: hidden;
}

.metric-card::after {
  position: absolute;
  top: -45px;
  right: -55px;
  width: 120px;
  height: 120px;
  border-radius: 50%;
  background: var(--metric-glow, rgba(91, 108, 255, 0.08));
  content: '';
  filter: blur(30px);
}

.metric-card--green { --metric-glow: rgba(21, 131, 95, 0.13); }
.metric-card--orange { --metric-glow: rgba(167, 101, 22, 0.13); }

.metric-icon {
  display: grid;
  width: 32px;
  height: 32px;
  place-items: center;
  color: var(--accent);
  border: 1px solid var(--border);
  border-radius: 8px;
  background: var(--surface-soft);
}

.metric-card > span {
  color: var(--text-secondary);
  font-size: 11px;
  font-weight: 600;
}

.metric-card > strong {
  grid-column: 1 / -1;
  margin-top: 11px;
  font-size: 27px;
  font-weight: 660;
  letter-spacing: -0.04em;
}

.metric-card > small {
  display: inline-flex;
  grid-column: 1 / -1;
  align-items: center;
  gap: 5px;
  margin-top: 5px;
  color: var(--text-tertiary);
  font-size: 10px;
}

.overview-grid {
  display: grid;
  grid-template-columns: minmax(0, 1.5fr) minmax(320px, 0.8fr);
  gap: 14px;
}

.activity-panel,
.health-panel,
.records-panel {
  padding: 20px;
}

.panel-heading {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: 16px;
}

.text-button {
  display: inline-flex;
  align-items: center;
  gap: 5px;
  padding: 4px 0;
  color: var(--text-secondary);
  border: 0;
  background: transparent;
  font-size: 11px;
  font-weight: 600;
}

.text-button:hover {
  color: var(--accent);
}

.chart-area {
  display: grid;
  height: 255px;
  grid-template-columns: 30px 1fr;
  gap: 8px;
  margin-top: 24px;
}

.chart-y-axis {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding-bottom: 27px;
  color: var(--text-tertiary);
  font-size: 9px;
}

.bar-chart {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: clamp(8px, 2vw, 24px);
  border-bottom: 1px solid var(--border);
  background-image: linear-gradient(var(--border) 1px, transparent 1px);
  background-size: 100% 33.33%;
}

.bar-column {
  display: grid;
  grid-template-rows: 1fr 26px;
  align-items: end;
  gap: 6px;
  text-align: center;
}

.bar-track {
  position: relative;
  display: flex;
  height: 100%;
  align-items: flex-end;
  justify-content: center;
}

.bar {
  position: absolute;
  bottom: 0;
  width: min(24px, 70%);
  border-radius: 5px 5px 1px 1px;
}

.bar--secondary {
  width: min(36px, 90%);
  background: var(--accent-soft);
}

.bar--primary {
  background: linear-gradient(to top, var(--accent-strong), #8592ff);
  box-shadow: 0 4px 14px rgba(91, 108, 255, 0.16);
}

.bar-column small {
  color: var(--text-tertiary);
  font-size: 9px;
}

.chart-legend {
  display: flex;
  justify-content: center;
  gap: 18px;
  margin-top: 14px;
  color: var(--text-secondary);
  font-size: 10px;
}

.chart-legend span {
  display: inline-flex;
  align-items: center;
  gap: 6px;
}

.legend-dot {
  width: 7px;
  height: 7px;
  border-radius: 2px;
}

.legend-dot--primary { background: var(--accent); }
.legend-dot--secondary { background: var(--accent-soft); border: 1px solid rgba(91, 108, 255, 0.2); }

.service-list {
  display: grid;
  margin-top: 16px;
}

.service-row {
  display: grid;
  grid-template-columns: 35px minmax(0, 1fr) auto;
  gap: 11px;
  align-items: center;
  padding: 14px 0;
  border-bottom: 1px solid var(--border);
}

.service-icon {
  display: grid;
  width: 34px;
  height: 34px;
  place-items: center;
  color: var(--text-secondary);
  border: 1px solid var(--border);
  border-radius: 8px;
  background: var(--surface-soft);
}

.service-copy,
.service-meta {
  display: grid;
  gap: 4px;
}

.service-copy strong {
  font-size: 11px;
  font-weight: 650;
}

.service-copy span,
.service-meta small {
  color: var(--text-tertiary);
  font-size: 9px;
}

.service-meta {
  text-align: right;
}

.service-state {
  color: var(--success);
  font-size: 10px;
  font-weight: 650;
}

.service-state i {
  display: inline-block;
  width: 6px;
  height: 6px;
  margin-right: 5px;
  border-radius: 50%;
  background: var(--success);
  box-shadow: 0 0 0 3px var(--success-soft);
}

.security-note {
  display: flex;
  gap: 10px;
  margin-top: 16px;
  padding: 12px;
  color: var(--success);
  border: 1px solid rgba(21, 131, 95, 0.14);
  border-radius: 9px;
  background: var(--success-soft);
}

.security-note div {
  display: grid;
  gap: 3px;
}

.security-note strong {
  font-size: 10px;
}

.security-note span {
  color: var(--text-secondary);
  font-size: 9px;
  line-height: 1.5;
}

.records-panel {
  padding-bottom: 0;
  overflow: hidden;
}

.records-heading {
  padding-bottom: 18px;
}

.table-scroll {
  overflow-x: auto;
  margin: 0 -20px;
  border-top: 1px solid var(--border);
}

.records-table th:first-child,
.records-table td:first-child {
  padding-left: 20px;
}

.records-table th:last-child,
.records-table td:last-child {
  padding-right: 20px;
}

.record-id {
  color: var(--accent);
  font-weight: 650;
}

@media (max-width: 1100px) {
  .hero,
  .overview-grid {
    grid-template-columns: 1fr;
  }

  .hero-status {
    min-height: 250px;
  }

  .metrics-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}

@media (max-width: 620px) {
  .hero {
    padding: 28px 22px;
  }

  .hero h1 {
    font-size: 38px;
  }

  .metrics-grid {
    grid-template-columns: 1fr;
  }

  .records-heading {
    align-items: flex-start;
    flex-direction: column;
  }
}
</style>
