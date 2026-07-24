<script setup lang="ts">
import {
  ChevronLeft,
  ChevronRight,
  Download,
  Eye,
  FileImage,
  Filter,
  Grid2X2,
  Info,
  List,
  Search,
  Share2,
  SlidersHorizontal,
  Sparkles,
  UserRound,
} from 'lucide-vue-next'
import { computed, ref } from 'vue'

const searchText = ref('00092318')
const viewMode = ref<'grid' | 'list'>('grid')
const activeType = ref('全部类型')
const selectedImage = ref(0)

const images = Array.from({ length: 12 }, (_, index) => ({
  id: index + 1,
  type: ['住院病案首页', '入院记录', '病程记录', '检查报告', '医嘱单', '出院记录'][index % 6],
  page: index + 1,
  size: `${(1.2 + (index % 4) * 0.35).toFixed(1)} MB`,
}))

const filteredImages = computed(() => activeType.value === '全部类型'
  ? images
  : images.filter(image => image.type === activeType.value))

const types = ['全部类型', '住院病案首页', '入院记录', '病程记录', '检查报告', '医嘱单', '出院记录']
</script>

<template>
  <div class="page archive-page">
    <div class="page-heading">
      <div>
        <div class="title-line">
          <h1 class="page-title">影像档案袋</h1>
          <span class="badge"><Sparkles :size="11" /> 核心场景</span>
        </div>
        <p class="page-description">以病案号、上架号或身份证号检索影像。页面强调快速定位、低干扰浏览与明确的信息层级。</p>
      </div>
      <div class="page-actions">
        <button class="button" type="button"><Share2 :size="15" /> 分享链接</button>
        <button class="button button--primary" type="button"><Download :size="15" /> 导出病案</button>
      </div>
    </div>

    <section class="search-panel surface">
      <div class="search-fields">
        <label class="field field--main">
          <span class="field-label">病案号 / 上架号 / 身份证号</span>
          <span class="search-input-wrap">
            <Search :size="17" />
            <input v-model="searchText" class="input" placeholder="输入检索条件">
            <kbd>Enter</kbd>
          </span>
        </label>
        <label class="field">
          <span class="field-label">检索范围</span>
          <select class="select">
            <option>自动识别</option>
            <option>病案号</option>
            <option>上架号</option>
            <option>身份证号</option>
          </select>
        </label>
        <button class="button button--primary search-button" type="button">
          <Search :size="15" /> 检索病案
        </button>
      </div>
      <div class="search-tip">
        <Info :size="14" />
        演示项目使用模拟数据，不会访问 MRR 数据库或真实影像存储。
      </div>
    </section>

    <section class="patient-card surface">
      <div class="patient-avatar"><UserRound :size="23" /></div>
      <div class="patient-primary">
        <div class="patient-name-row">
          <h2>示例患者 A</h2>
          <span class="badge badge--success">已归档</span>
        </div>
        <p>病案号 <span class="mono">00092318</span> · 上架号 <span class="mono">100092318</span></p>
      </div>
      <div class="patient-facts">
        <div><span>性别 / 年龄</span><strong>男 · 56 岁</strong></div>
        <div><span>出院科室</span><strong>心血管内科</strong></div>
        <div><span>出院日期</span><strong>2026-07-22</strong></div>
        <div><span>影像数量</span><strong>86 页</strong></div>
      </div>
    </section>

    <section class="archive-workspace">
      <aside class="type-sidebar surface">
        <div class="type-sidebar-head">
          <div>
            <h2 class="section-title">影像分类</h2>
            <p class="section-description">共 86 页</p>
          </div>
          <button class="icon-button" type="button"><Filter :size="15" /></button>
        </div>
        <div class="type-list">
          <button
            v-for="(type, index) in types"
            :key="type"
            type="button"
            :class="{ active: activeType === type }"
            @click="activeType = type"
          >
            <span>{{ type }}</span>
            <small>{{ index === 0 ? 86 : [4, 12, 24, 16, 22, 8][index - 1] }}</small>
          </button>
        </div>
        <div class="classification-note">
          <Sparkles :size="16" />
          <div>
            <strong>智能 OCR 分类</strong>
            <span>可在未来接入 MRR 的 OCR 分类能力。</span>
          </div>
        </div>
      </aside>

      <div class="image-content surface">
        <div class="image-toolbar">
          <div>
            <h2 class="section-title">{{ activeType }}</h2>
            <span>{{ filteredImages.length }} 张示例影像</span>
          </div>
          <div class="toolbar-actions">
            <button class="icon-button" type="button" title="筛选"><SlidersHorizontal :size="15" /></button>
            <div class="segmented">
              <button type="button" :class="{ active: viewMode === 'grid' }" @click="viewMode = 'grid'"><Grid2X2 :size="14" /></button>
              <button type="button" :class="{ active: viewMode === 'list' }" @click="viewMode = 'list'"><List :size="15" /></button>
            </div>
          </div>
        </div>

        <div v-if="viewMode === 'grid'" class="image-grid">
          <button
            v-for="image in filteredImages"
            :key="image.id"
            class="image-card"
            :class="{ active: selectedImage === image.id }"
            type="button"
            @click="selectedImage = image.id"
          >
            <div class="mock-document">
              <div class="document-head"><span /><span /></div>
              <div class="document-lines">
                <i v-for="line in 9" :key="line" :style="{ width: `${52 + ((line * 13 + image.id * 7) % 42)}%` }" />
              </div>
              <div class="document-stamp">MRR</div>
              <span class="page-number">{{ image.page }}</span>
            </div>
            <div class="image-card-meta">
              <div><strong>{{ image.type }}</strong><span>第 {{ image.page }} 页 · {{ image.size }}</span></div>
              <Eye :size="14" />
            </div>
          </button>
        </div>

        <div v-else class="image-list">
          <button v-for="image in filteredImages" :key="image.id" type="button" @click="selectedImage = image.id">
            <span class="list-icon"><FileImage :size="17" /></span>
            <span class="list-main"><strong>{{ image.type }}</strong><small>第 {{ image.page }} 页 · JPEG · {{ image.size }}</small></span>
            <span class="badge">来源：archive</span>
            <Eye :size="15" />
          </button>
        </div>

        <div class="pagination">
          <span>第 1–{{ filteredImages.length }} 项，共 86 项</span>
          <div>
            <button class="icon-button" type="button"><ChevronLeft :size="15" /></button>
            <button class="page-current" type="button">1</button>
            <button class="icon-button" type="button"><ChevronRight :size="15" /></button>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
.archive-page { gap: 18px; }
.title-line, .page-actions, .patient-name-row, .toolbar-actions, .pagination, .pagination > div { display: flex; align-items: center; gap: 10px; }
.search-panel { padding: 18px; }
.search-fields { display: grid; grid-template-columns: minmax(300px, 1fr) 180px auto; gap: 13px; align-items: end; }
.search-input-wrap { position: relative; display: flex; align-items: center; }
.search-input-wrap > svg { position: absolute; z-index: 1; left: 12px; color: var(--text-tertiary); }
.search-input-wrap .input { padding-right: 55px; padding-left: 38px; }
.search-input-wrap kbd { position: absolute; right: 10px; padding: 2px 5px; color: var(--text-tertiary); border: 1px solid var(--border); border-radius: 4px; background: var(--surface-soft); font-size: 9px; }
.search-button { height: 40px; }
.search-tip { display: flex; align-items: center; gap: 7px; margin-top: 13px; padding-top: 13px; color: var(--text-tertiary); border-top: 1px solid var(--border); font-size: 10px; }
.patient-card { display: grid; grid-template-columns: 46px minmax(220px, 0.65fr) 1.35fr; gap: 16px; align-items: center; padding: 17px 20px; }
.patient-avatar { display: grid; width: 44px; height: 44px; place-items: center; color: var(--accent); border: 1px solid var(--border); border-radius: 10px; background: var(--accent-soft); }
.patient-primary h2 { font-size: 16px; letter-spacing: -0.02em; }
.patient-primary p { margin-top: 6px; color: var(--text-secondary); font-size: 10px; }
.patient-facts { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; }
.patient-facts div { display: grid; gap: 4px; padding-left: 15px; border-left: 1px solid var(--border); }
.patient-facts span { color: var(--text-tertiary); font-size: 9px; }
.patient-facts strong { font-size: 11px; font-weight: 630; }
.archive-workspace { display: grid; grid-template-columns: 235px minmax(0, 1fr); gap: 14px; align-items: start; }
.type-sidebar { position: sticky; top: 82px; padding: 14px; }
.type-sidebar-head, .image-toolbar { display: flex; align-items: center; justify-content: space-between; gap: 12px; }
.type-list { display: grid; gap: 3px; margin-top: 14px; }
.type-list button { display: flex; min-height: 36px; align-items: center; justify-content: space-between; padding: 0 10px; color: var(--text-secondary); border: 1px solid transparent; border-radius: 7px; background: transparent; font-size: 11px; text-align: left; }
.type-list button:hover { color: var(--text); background: var(--surface-hover); }
.type-list button.active { color: var(--accent); border-color: rgba(91,108,255,.14); background: var(--accent-soft); font-weight: 650; }
.type-list small { display: grid; min-width: 23px; height: 20px; place-items: center; border-radius: 999px; background: var(--surface-soft); font-size: 9px; }
.classification-note { display: flex; gap: 9px; margin-top: 16px; padding: 12px; color: var(--accent); border: 1px solid rgba(91,108,255,.16); border-radius: 9px; background: var(--accent-soft); }
.classification-note div { display: grid; gap: 4px; }
.classification-note strong { font-size: 10px; }
.classification-note span { color: var(--text-secondary); font-size: 9px; line-height: 1.5; }
.image-content { overflow: hidden; }
.image-toolbar { padding: 15px 17px; border-bottom: 1px solid var(--border); }
.image-toolbar > div:first-child { display: grid; gap: 3px; }
.image-toolbar > div:first-child > span { color: var(--text-tertiary); font-size: 9px; }
.segmented { display: flex; padding: 3px; border: 1px solid var(--border); border-radius: 8px; background: var(--surface-soft); }
.segmented button { display: grid; width: 29px; height: 27px; place-items: center; color: var(--text-tertiary); border: 0; border-radius: 5px; background: transparent; }
.segmented button.active { color: var(--text); background: var(--surface); box-shadow: var(--shadow-sm); }
.image-grid { display: grid; grid-template-columns: repeat(4, minmax(0, 1fr)); gap: 13px; padding: 17px; }
.image-card { min-width: 0; padding: 0; overflow: hidden; color: var(--text); border: 1px solid var(--border); border-radius: 10px; background: var(--surface); text-align: left; transition: transform 180ms ease, border-color 180ms ease, box-shadow 180ms ease; }
.image-card:hover { border-color: var(--border-strong); box-shadow: var(--shadow-md); transform: translateY(-2px); }
.image-card.active { border-color: var(--accent); box-shadow: 0 0 0 3px var(--accent-soft); }
.mock-document { position: relative; aspect-ratio: 4 / 5.1; overflow: hidden; padding: 16px 13px; background: linear-gradient(135deg, #f4f1e9, #fffdf7); }
.document-head { display: flex; justify-content: space-between; padding-bottom: 10px; border-bottom: 2px solid #555a61; }
.document-head span:first-child { width: 42%; height: 6px; background: #42464c; }
.document-head span:last-child { width: 18%; height: 6px; background: #a7a39b; }
.document-lines { display: grid; gap: 8px; margin-top: 14px; }
.document-lines i { height: 3px; border-radius: 3px; background: #bbb7ae; }
.document-stamp { position: absolute; right: 14px; bottom: 18px; display: grid; width: 42px; height: 42px; place-items: center; color: rgba(179,52,52,.5); border: 2px solid rgba(179,52,52,.4); border-radius: 50%; font-family: serif; font-size: 10px; transform: rotate(-12deg); }
.page-number { position: absolute; bottom: 7px; left: 50%; color: #88847c; font-family: serif; font-size: 8px; transform: translateX(-50%); }
.image-card-meta { display: flex; align-items: center; justify-content: space-between; gap: 8px; padding: 11px; }
.image-card-meta div { display: grid; min-width: 0; gap: 4px; }
.image-card-meta strong { overflow: hidden; font-size: 10px; white-space: nowrap; text-overflow: ellipsis; }
.image-card-meta span { color: var(--text-tertiary); font-size: 9px; }
.image-list { display: grid; padding: 8px 17px; }
.image-list button { display: grid; grid-template-columns: 36px 1fr auto 20px; gap: 10px; align-items: center; padding: 10px 4px; color: var(--text); border: 0; border-bottom: 1px solid var(--border); background: transparent; text-align: left; }
.list-icon { display: grid; width: 34px; height: 34px; place-items: center; color: var(--accent); border-radius: 7px; background: var(--accent-soft); }
.list-main { display: grid; gap: 4px; }
.list-main strong { font-size: 11px; }
.list-main small { color: var(--text-tertiary); font-size: 9px; }
.pagination { justify-content: space-between; padding: 13px 17px; color: var(--text-tertiary); border-top: 1px solid var(--border); font-size: 10px; }
.pagination .icon-button { width: 31px; height: 31px; }
.page-current { width: 31px; height: 31px; color: #fff; border: 0; border-radius: 7px; background: var(--accent); }
@media (max-width: 1200px) { .image-grid { grid-template-columns: repeat(3, minmax(0,1fr)); } .patient-card { grid-template-columns: 46px 1fr; } .patient-facts { grid-column: 1 / -1; } }
@media (max-width: 820px) { .search-fields { grid-template-columns: 1fr; } .archive-workspace { grid-template-columns: 1fr; } .type-sidebar { position: static; } .type-list { grid-template-columns: repeat(2,1fr); } .image-grid { grid-template-columns: repeat(2,minmax(0,1fr)); } .patient-facts { grid-template-columns: repeat(2,1fr); } }
@media (max-width: 520px) { .page-actions { width: 100%; } .page-actions .button { flex: 1; } .image-grid { grid-template-columns: 1fr; } .type-list { grid-template-columns: 1fr; } }
</style>
