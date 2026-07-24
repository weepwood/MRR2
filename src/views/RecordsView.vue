<script setup lang="ts">
import { CheckCircle2, ChevronLeft, ChevronRight, Download, Filter, MoreHorizontal, Plus, RefreshCw, Search, SlidersHorizontal } from 'lucide-vue-next'
import { computed, ref } from 'vue'

const query = ref('')
const records = [
  { bah: '00092318', sjh: '100092318', name: '示例患者 A', department: '心血管内科', discharge: '2026-07-22', pages: 86, status: '已归档' },
  { bah: '00092317', sjh: '100092317', name: '示例患者 B', department: '神经内科', discharge: '2026-07-22', pages: 142, status: '待复核' },
  { bah: '00092316', sjh: '100092316', name: '示例患者 C', department: '骨科', discharge: '2026-07-21', pages: 64, status: '已归档' },
  { bah: '00092315', sjh: '100092315', name: '示例患者 D', department: '呼吸内科', discharge: '2026-07-21', pages: 108, status: '扫描中' },
  { bah: '00092314', sjh: '100092314', name: '示例患者 E', department: '普外科', discharge: '2026-07-20', pages: 73, status: '已归档' },
  { bah: '00092313', sjh: '100092313', name: '示例患者 F', department: '泌尿外科', discharge: '2026-07-20', pages: 95, status: '待复核' },
  { bah: '00092312', sjh: '100092312', name: '示例患者 G', department: '肿瘤内科', discharge: '2026-07-19', pages: 121, status: '已归档' },
]
const filteredRecords = computed(() => records.filter(record => Object.values(record).join(' ').toLowerCase().includes(query.value.toLowerCase())))
</script>

<template>
  <div class="page">
    <div class="page-heading">
      <div><h1 class="page-title">记录管理</h1><p class="page-description">用更轻的工具栏、更稳定的表格密度和明确状态色，降低大量病案数据的阅读负担。</p></div>
      <div class="actions"><button class="button"><Download :size="15" /> 导出</button><button class="button button--primary"><Plus :size="15" /> 新增记录</button></div>
    </div>
    <section class="filter-bar surface">
      <div class="query-wrap"><Search :size="16" /><input v-model="query" class="input" placeholder="搜索病案号、上架号、患者或科室"></div>
      <select class="select"><option>全部状态</option><option>已归档</option><option>待复核</option><option>扫描中</option></select>
      <select class="select"><option>全部科室</option><option>心血管内科</option><option>神经内科</option><option>骨科</option></select>
      <button class="button"><Filter :size="14" /> 更多筛选</button>
    </section>
    <section class="surface table-card">
      <div class="table-heading">
        <div><h2 class="section-title">病案记录</h2><p class="section-description">显示 {{ filteredRecords.length }} 条 Mock 数据</p></div>
        <div class="actions"><button class="icon-button" title="刷新"><RefreshCw :size="15" /></button><button class="icon-button" title="列设置"><SlidersHorizontal :size="15" /></button></div>
      </div>
      <div class="table-wrap inner-table">
        <table class="data-table">
          <thead><tr><th><input type="checkbox"></th><th>病案号</th><th>上架号</th><th>患者</th><th>出院科室</th><th>出院日期</th><th>页数</th><th>状态</th><th></th></tr></thead>
          <tbody>
            <tr v-for="record in filteredRecords" :key="record.bah">
              <td><input type="checkbox"></td><td class="mono link-cell">{{ record.bah }}</td><td class="mono muted">{{ record.sjh }}</td><td><strong>{{ record.name }}</strong></td><td class="muted">{{ record.department }}</td><td>{{ record.discharge }}</td><td>{{ record.pages }}</td>
              <td><span class="badge" :class="{ 'badge--success': record.status === '已归档', 'badge--warning': record.status !== '已归档' }"><CheckCircle2 :size="11" />{{ record.status }}</span></td>
              <td><button class="row-more"><MoreHorizontal :size="16" /></button></td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="table-footer"><span>已选择 0 项 · 共 206,482 条记录</span><div class="actions"><button class="icon-button"><ChevronLeft :size="14" /></button><button class="page-button active">1</button><button class="page-button">2</button><button class="page-button">3</button><button class="icon-button"><ChevronRight :size="14" /></button></div></div>
    </section>
  </div>
</template>

<style scoped>
.actions { display:flex; align-items:center; gap:9px; }
.filter-bar { display:grid; grid-template-columns:minmax(280px,1fr) 150px 150px auto; gap:11px; padding:15px; }
.query-wrap { position:relative; display:flex; align-items:center; }
.query-wrap svg { position:absolute; z-index:1; left:12px; color:var(--text-tertiary); }
.query-wrap .input { padding-left:37px; }
.table-card { overflow:hidden; }
.table-heading,.table-footer { display:flex; align-items:center; justify-content:space-between; gap:16px; padding:17px; }
.inner-table { border:0; border-top:1px solid var(--border); border-bottom:1px solid var(--border); border-radius:0; box-shadow:none; }
.link-cell { color:var(--accent); font-weight:650; }
.row-more { display:grid; width:30px; height:30px; place-items:center; color:var(--text-tertiary); border:0; border-radius:6px; background:transparent; }
.row-more:hover { color:var(--text); background:var(--surface-hover); }
.table-footer { color:var(--text-tertiary); font-size:10px; }
.table-footer .icon-button,.page-button { width:31px; height:31px; }
.page-button { color:var(--text-secondary); border:1px solid transparent; border-radius:7px; background:transparent; }
.page-button.active { color:#fff; background:var(--accent); }
@media (max-width:900px){.filter-bar{grid-template-columns:1fr 1fr}.query-wrap{grid-column:1/-1}}
@media (max-width:580px){.filter-bar{grid-template-columns:1fr}.query-wrap{grid-column:auto}.table-heading,.table-footer{align-items:flex-start;flex-direction:column}}
</style>
