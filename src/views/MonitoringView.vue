<script setup lang="ts">
import { Activity, CheckCircle2, Cpu, Database, HardDrive, MemoryStick, RefreshCw, Server, ShieldCheck, TimerReset, Wifi } from 'lucide-vue-next'

const services = [
  { name: 'MRR API', endpoint: 'http://localhost:8002', uptime: '18 天 07:42', latency: 23, icon: Server },
  { name: 'PostgreSQL', endpoint: 'mrr-app / PostgreSQL 16', uptime: '31 天 12:18', latency: 11, icon: Database },
  { name: '本地影像存储', endpoint: 'D:\\MRR\\images', uptime: '可用 8.4 TB', latency: 36, icon: HardDrive },
  { name: '外部系统接入', endpoint: 'HMAC Ticket Gateway', uptime: '最近调用 2 分钟前', latency: 41, icon: Wifi },
]
</script>

<template>
  <div class="page">
    <div class="page-heading">
      <div><h1 class="page-title">系统监控</h1><p class="page-description">监控页面使用更深的局部面板承载实时数据，同时保持表格和诊断信息的可读性。</p></div>
      <button class="button"><RefreshCw :size="15" /> 刷新指标</button>
    </div>
    <section class="status-banner">
      <div class="status-glow" />
      <div class="status-main"><span class="status-icon"><CheckCircle2 :size="24" /></span><div><span>系统运行状态</span><h2>所有核心服务均正常</h2><p>最近检查于 2026-07-24 20:58，未发现影响业务的异常。</p></div></div>
      <div class="status-meta"><div><strong>99.98%</strong><span>30 日可用性</span></div><div><strong>31 ms</strong><span>P50 响应</span></div><div><strong>0.03%</strong><span>错误率</span></div></div>
    </section>
    <section class="resource-grid">
      <article class="resource surface"><div class="resource-head"><span><Cpu :size="17" /> CPU 使用率</span><strong>28%</strong></div><div class="gauge"><span style="width:28%"/></div><small>8 核 · 负载 1.42</small></article>
      <article class="resource surface"><div class="resource-head"><span><MemoryStick :size="17" /> 内存使用率</span><strong>62%</strong></div><div class="gauge"><span style="width:62%"/></div><small>9.9 / 16 GB</small></article>
      <article class="resource surface"><div class="resource-head"><span><HardDrive :size="17" /> 数据盘使用率</span><strong>71%</strong></div><div class="gauge warning"><span style="width:71%"/></div><small>20.6 / 29 TB</small></article>
      <article class="resource surface"><div class="resource-head"><span><Activity :size="17" /> 活跃请求</span><strong>18</strong></div><div class="spark"><i v-for="h in [24,38,31,47,42,56,39,63,51,70,48,62,54,76,59,68]" :key="h" :style="{height:`${h}%`} "/></div><small>峰值 64 / 分钟</small></article>
    </section>
    <section class="monitor-grid">
      <article class="surface services-panel">
        <div class="panel-head"><div><h2 class="section-title">服务健康检查</h2><p class="section-description">接口、数据库和存储依赖</p></div><span class="badge badge--success">4 / 4 正常</span></div>
        <div class="service-list">
          <div v-for="service in services" :key="service.name" class="service-row"><span class="service-icon"><component :is="service.icon" :size="17" /></span><div><strong>{{ service.name }}</strong><small>{{ service.endpoint }}</small></div><div><strong>{{ service.uptime }}</strong><small>运行状态</small></div><div><strong>{{ service.latency }} ms</strong><small>响应延迟</small></div><span class="health-dot" /></div>
        </div>
      </article>
      <article class="surface requests-panel">
        <div class="panel-head"><div><h2 class="section-title">接口响应分布</h2><p class="section-description">过去 60 分钟</p></div></div>
        <div class="latency-value"><strong>31</strong><span>ms<br>P50 响应时间</span></div>
        <div class="latency-list"><div><span>&lt; 100 ms</span><div><i style="width:88%"/></div><strong>88%</strong></div><div><span>100–500 ms</span><div><i style="width:10%"/></div><strong>10%</strong></div><div><span>500 ms–2 s</span><div><i class="warning" style="width:2%"/></div><strong>2%</strong></div><div><span>&gt; 2 s</span><div><i class="danger" style="width:.2%"/></div><strong>0.2%</strong></div></div>
        <div class="monitor-note"><ShieldCheck :size="16" /><span>当前无持续性慢接口告警。</span></div>
      </article>
    </section>
    <section class="surface event-panel"><div class="panel-head"><div><h2 class="section-title">最近系统事件</h2><p class="section-description">只保留需要判断的事件，普通运行日志不在此堆叠</p></div></div><div class="event-list"><div><span class="event-icon success"><CheckCircle2 :size="15" /></span><div><strong>数据库备份检查完成</strong><small>备份文件可读，最近恢复验证成功。</small></div><time>19:42</time></div><div><span class="event-icon"><TimerReset :size="15" /></span><div><strong>影像缓存完成周期清理</strong><small>释放 12.8 GB 临时文件空间。</small></div><time>18:00</time></div><div><span class="event-icon"><Activity :size="15" /></span><div><strong>接口延迟短暂升高</strong><small>/api/v1/statistics 在 16:21 达到 812 ms，现已恢复。</small></div><time>16:21</time></div></div></section>
  </div>
</template>

<style scoped>
.status-banner{position:relative;display:flex;align-items:center;justify-content:space-between;gap:30px;overflow:hidden;padding:27px 30px;color:#f6f7fb;border:1px solid rgba(255,255,255,.08);border-radius:16px;background:#0d1016}.status-glow{position:absolute;top:-180px;left:20%;width:420px;height:300px;border-radius:50%;background:rgba(75,201,151,.18);filter:blur(90px)}.status-main,.status-meta,.resource-head,.panel-head{position:relative;display:flex;align-items:center;gap:14px}.status-icon{display:grid;width:48px;height:48px;place-items:center;color:#70dcb2;border:1px solid rgba(112,220,178,.2);border-radius:12px;background:rgba(112,220,178,.08)}.status-main>div{display:grid;gap:5px}.status-main>div>span{color:#70dcb2;font-size:10px;font-weight:650}.status-main h2{font-size:22px;letter-spacing:-.03em}.status-main p{color:#8f97a5;font-size:10px}.status-meta{gap:28px}.status-meta div{display:grid;gap:4px;padding-left:22px;border-left:1px solid rgba(255,255,255,.09)}.status-meta strong{font-size:20px}.status-meta span{color:#868e9c;font-size:9px}.resource-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:14px}.resource{display:grid;gap:12px;padding:17px}.resource-head{justify-content:space-between}.resource-head span{display:flex;align-items:center;gap:7px;color:var(--text-secondary);font-size:10px}.resource-head strong{font-size:17px}.gauge{height:6px;overflow:hidden;border-radius:999px;background:var(--surface-soft)}.gauge span{display:block;height:100%;border-radius:999px;background:linear-gradient(90deg,var(--accent-strong),#8591ff)}.gauge.warning span{background:linear-gradient(90deg,#bd7a28,#edae58)}.resource small{color:var(--text-tertiary);font-size:9px}.spark{display:flex;height:24px;align-items:flex-end;gap:3px}.spark i{flex:1;border-radius:2px;background:var(--accent)}.monitor-grid{display:grid;grid-template-columns:minmax(0,1.45fr) minmax(300px,.65fr);gap:14px}.services-panel,.requests-panel,.event-panel{padding:19px}.panel-head{justify-content:space-between}.service-list{display:grid;margin-top:13px}.service-row{display:grid;grid-template-columns:38px minmax(170px,1.3fr) 1fr 90px 10px;gap:12px;align-items:center;padding:13px 0;border-bottom:1px solid var(--border)}.service-icon{display:grid;width:36px;height:36px;place-items:center;color:var(--text-secondary);border:1px solid var(--border);border-radius:8px;background:var(--surface-soft)}.service-row>div{display:grid;gap:4px}.service-row strong{font-size:10px}.service-row small{color:var(--text-tertiary);font-size:9px}.health-dot{width:7px;height:7px;border-radius:50%;background:var(--success);box-shadow:0 0 0 4px var(--success-soft)}.latency-value{display:flex;align-items:flex-end;gap:10px;margin:24px 0}.latency-value strong{font-size:48px;line-height:.8;letter-spacing:-.06em}.latency-value span{color:var(--text-tertiary);font-size:9px;line-height:1.5}.latency-list{display:grid;gap:13px}.latency-list>div{display:grid;grid-template-columns:83px 1fr 34px;gap:8px;align-items:center}.latency-list span,.latency-list strong{font-size:9px}.latency-list span{color:var(--text-secondary)}.latency-list>div>div{height:6px;overflow:hidden;border-radius:999px;background:var(--surface-soft)}.latency-list i{display:block;height:100%;border-radius:999px;background:var(--accent)}.latency-list i.warning{background:var(--warning)}.latency-list i.danger{background:var(--danger)}.monitor-note{display:flex;align-items:center;gap:8px;margin-top:22px;padding:11px;color:var(--success);border-radius:8px;background:var(--success-soft);font-size:9px}.event-list{display:grid;margin-top:13px}.event-list>div{display:grid;grid-template-columns:34px 1fr auto;gap:12px;align-items:center;padding:13px 0;border-bottom:1px solid var(--border)}.event-icon{display:grid;width:32px;height:32px;place-items:center;color:var(--accent);border-radius:8px;background:var(--accent-soft)}.event-icon.success{color:var(--success);background:var(--success-soft)}.event-list>div>div{display:grid;gap:4px}.event-list strong{font-size:10px}.event-list small,.event-list time{color:var(--text-tertiary);font-size:9px}
@media(max-width:1100px){.resource-grid{grid-template-columns:repeat(2,1fr)}.monitor-grid{grid-template-columns:1fr}.status-banner{align-items:flex-start;flex-direction:column}}
@media(max-width:620px){.resource-grid{grid-template-columns:1fr}.status-meta{width:100%;justify-content:space-between;gap:8px}.status-meta div{padding-left:10px}.service-row{grid-template-columns:38px 1fr 10px}.service-row>div:nth-of-type(2),.service-row>div:nth-of-type(3){display:none}}
</style>
