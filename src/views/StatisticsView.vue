<script setup lang="ts">
import { ArrowDownRight, ArrowUpRight, CalendarDays, Download, FileCheck2, FileClock, Layers3, RefreshCw } from 'lucide-vue-next'

const departments = [
  { name: '心血管内科', total: 18620, scanned: 17480, rate: 93.9 },
  { name: '神经内科', total: 16540, scanned: 14830, rate: 89.7 },
  { name: '骨科', total: 15280, scanned: 14420, rate: 94.4 },
  { name: '呼吸内科', total: 13960, scanned: 12150, rate: 87.0 },
  { name: '普外科', total: 12840, scanned: 11980, rate: 93.3 },
]
</script>

<template>
  <div class="page">
    <div class="page-heading">
      <div><h1 class="page-title">病案扫描统计</h1><p class="page-description">避免将每个数字都做成彩色卡片，只突出趋势、异常和需要决策的信息。</p></div>
      <div class="actions"><button class="button"><RefreshCw :size="15" /> 刷新</button><button class="button button--primary"><Download :size="15" /> 导出报表</button></div>
    </div>
    <section class="period-bar surface"><div><CalendarDays :size="16" /><span>统计周期</span></div><button class="period active">近 30 日</button><button class="period">近 90 日</button><button class="period">本年度</button><span class="period-range">2026-06-25 — 2026-07-24</span></section>
    <section class="summary-grid">
      <article class="summary surface"><span>应扫描病案</span><strong>42,680</strong><small class="up"><ArrowUpRight :size="12" /> 同比 8.4%</small></article>
      <article class="summary surface"><span>已扫描病案</span><strong>39,412</strong><small class="up"><ArrowUpRight :size="12" /> 同比 10.2%</small></article>
      <article class="summary surface"><span>总体完成率</span><strong>92.34%</strong><small class="up"><ArrowUpRight :size="12" /> 提升 1.6%</small></article>
      <article class="summary surface"><span>缺失与异常</span><strong>326</strong><small class="down"><ArrowDownRight :size="12" /> 减少 12.8%</small></article>
    </section>
    <section class="stats-grid">
      <article class="surface trend-panel">
        <div class="panel-head"><div><h2 class="section-title">扫描完成趋势</h2><p class="section-description">每日已扫描病案数与完成率</p></div><span class="badge">单位：份</span></div>
        <div class="line-chart">
          <div class="grid-lines"><i/><i/><i/><i/></div>
          <svg viewBox="0 0 700 250" preserveAspectRatio="none" aria-label="扫描趋势示意图">
            <defs><linearGradient id="area" x1="0" y1="0" x2="0" y2="1"><stop offset="0%" stop-color="#5b6cff" stop-opacity=".28"/><stop offset="100%" stop-color="#5b6cff" stop-opacity="0"/></linearGradient></defs>
            <path d="M0,205 C80,184 82,160 145,171 C215,183 215,118 285,132 C350,145 367,83 430,99 C495,115 520,52 575,74 C633,94 650,35 700,42 L700,250 L0,250 Z" fill="url(#area)"/>
            <path d="M0,205 C80,184 82,160 145,171 C215,183 215,118 285,132 C350,145 367,83 430,99 C495,115 520,52 575,74 C633,94 650,35 700,42" fill="none" stroke="#6f7eff" stroke-width="4" vector-effect="non-scaling-stroke"/>
          </svg>
          <div class="x-labels"><span>6/25</span><span>7/01</span><span>7/07</span><span>7/13</span><span>7/19</span><span>7/24</span></div>
        </div>
      </article>
      <article class="surface distribution-panel">
        <div class="panel-head"><div><h2 class="section-title">扫描状态分布</h2><p class="section-description">当前周期内的状态构成</p></div></div>
        <div class="donut-wrap"><div class="donut"><div><strong>42,680</strong><span>病案总数</span></div></div></div>
        <div class="distribution-list"><div><i class="blue"/><span>已完成</span><strong>92.34%</strong></div><div><i class="amber"/><span>待扫描</span><strong>6.90%</strong></div><div><i class="red"/><span>异常</span><strong>0.76%</strong></div></div>
      </article>
    </section>
    <section class="surface department-panel">
      <div class="panel-head"><div><h2 class="section-title">科室完成情况</h2><p class="section-description">按应扫描数量排序，快速识别积压科室</p></div><span class="badge"><Layers3 :size="11" /> 5 个科室</span></div>
      <div class="department-list">
        <div v-for="(department,index) in departments" :key="department.name" class="department-row">
          <span class="rank">{{ String(index+1).padStart(2,'0') }}</span><div class="department-name"><strong>{{ department.name }}</strong><span>{{ department.scanned.toLocaleString() }} / {{ department.total.toLocaleString() }} 份</span></div><div class="progress"><span :style="{width:`${department.rate}%`} "/></div><strong class="rate">{{ department.rate }}%</strong><FileCheck2 v-if="department.rate>92" :size="16" class="good"/><FileClock v-else :size="16" class="waiting"/>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
.actions,.period-bar,.period-bar>div,.panel-head,.distribution-list>div{display:flex;align-items:center;gap:9px}.period-bar{padding:10px 13px}.period-bar>div{padding-right:9px;color:var(--text-secondary);font-size:11px;font-weight:650}.period{min-height:31px;padding:0 10px;color:var(--text-secondary);border:0;border-radius:6px;background:transparent;font-size:10px}.period.active{color:var(--accent);background:var(--accent-soft);font-weight:650}.period-range{margin-left:auto;color:var(--text-tertiary);font-size:9px}.summary-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:14px}.summary{display:grid;gap:8px;padding:18px}.summary>span{color:var(--text-secondary);font-size:10px}.summary>strong{font-size:26px;letter-spacing:-.04em}.summary small{display:flex;align-items:center;gap:4px;font-size:9px}.up{color:var(--success)}.down{color:var(--success)}.stats-grid{display:grid;grid-template-columns:minmax(0,1.6fr) minmax(300px,.65fr);gap:14px}.trend-panel,.distribution-panel,.department-panel{padding:19px}.panel-head{justify-content:space-between}.line-chart{position:relative;height:290px;margin-top:18px;padding:8px 0 24px}.line-chart svg{position:relative;z-index:2;width:100%;height:100%;overflow:visible}.grid-lines{position:absolute;inset:10px 0 28px;display:flex;flex-direction:column;justify-content:space-between}.grid-lines i{width:100%;border-top:1px solid var(--border)}.x-labels{display:flex;justify-content:space-between;color:var(--text-tertiary);font-size:9px}.donut-wrap{display:grid;place-items:center;padding:29px}.donut{display:grid;width:176px;height:176px;place-items:center;border-radius:50%;background:conic-gradient(var(--accent) 0 92.34%,#e7a84a 92.34% 99.24%,#d45c5c 99.24%);box-shadow:inset 0 0 0 1px var(--border)}.donut>div{display:grid;width:126px;height:126px;place-content:center;border-radius:50%;background:var(--surface);text-align:center}.donut strong{font-size:23px;letter-spacing:-.04em}.donut span{margin-top:4px;color:var(--text-tertiary);font-size:9px}.distribution-list{display:grid;gap:9px}.distribution-list>div{padding:9px 0;border-bottom:1px solid var(--border);font-size:10px}.distribution-list i{width:8px;height:8px;border-radius:3px}.distribution-list span{flex:1;color:var(--text-secondary)}.blue{background:var(--accent)}.amber{background:#e7a84a}.red{background:#d45c5c}.department-list{display:grid;margin-top:14px}.department-row{display:grid;grid-template-columns:32px 170px 1fr 60px 20px;gap:13px;align-items:center;padding:14px 0;border-bottom:1px solid var(--border)}.rank{color:var(--text-tertiary);font-family:monospace;font-size:10px}.department-name{display:grid;gap:4px}.department-name strong{font-size:11px}.department-name span{color:var(--text-tertiary);font-size:9px}.progress{height:7px;overflow:hidden;border-radius:999px;background:var(--surface-soft)}.progress span{display:block;height:100%;border-radius:999px;background:linear-gradient(90deg,var(--accent-strong),#8793ff)}.rate{text-align:right;font-size:11px}.good{color:var(--success)}.waiting{color:var(--warning)}
@media(max-width:1000px){.summary-grid{grid-template-columns:repeat(2,1fr)}.stats-grid{grid-template-columns:1fr}}
@media(max-width:650px){.summary-grid{grid-template-columns:1fr}.period-range{display:none}.department-row{grid-template-columns:28px 1fr 55px 18px}.progress{grid-column:2/-1}.department-name{grid-column:2}.rate{grid-column:3;grid-row:1}.department-row>svg{grid-column:4;grid-row:1}}
</style>
