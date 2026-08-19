<template>
  <div class="portfolio-wrapper">
    <!-- Header Section -->
    <div class="header">
      <div class="profile-box">
        <div class="dim-label dim-top">{{ imgWidth || '—' }}</div>
        <div class="profile-img-row">
          <span class="dim-label dim-left">{{ imgHeight || '—' }}</span>
          <img 
            src="/profile.jpg" 
            alt="Afwan Apriansyah" 
            class="profile-img" 
            @load="handleImageLoad"
          >
          <span class="dim-label dim-right">{{ imgHeight || '—' }}</span>
        </div>
        <div class="dim-label dim-bottom">{{ imgWidth || '—' }}</div>
      </div>
      <div class="header-content">
        <h1>Afwan Apriansyah</h1>
        <div class="tagline">computer science student — afwanapriansyah@gmail.com</div>
      </div>
    </div>

    <!-- Dual Full-Width Divider Lines -->
    <hr class="divider">
    <hr class="divider">

    <!-- 2 Columns Grid Layout -->
    <div class="grid-container">

      <!-- Column 1: Summary -->
      <div class="col">
        <div class="col-title">summary</div>
        <table class="summary-table">
          <tbody>
            <tr>
              <td class="label">status</td>
              <td>open to fulltime & internship positions</td>
            </tr>
            <tr>
              <td class="label">field</td>
              <td>Fullstack web dev, QA Engineering, SDET</td>
            </tr>
            <tr>
              <td class="label">institution</td>
              <td>Universitas Gunadarma, Depok</td>
            </tr>
            <tr>
              <td class="label">degree</td>
              <td>bsc informatics (computer science)</td>
            </tr>
            <tr>
              <td class="label">gpa</td>
              <td>3.62 / 4.00</td>
            </tr>
          </tbody>
        </table>

        <!-- Featured Thesis Project -->
        <div class="featured-link">
          <div class="thesis-heading"><strong>thesis (under progression)</strong></div>
          <a href="https://github.com/nxstray" target="_blank">
            powerbind — Home Power Usage Management System with Integrated Multimodal AI Agent (Groq)
          </a>
          <div class="meta">
            IoT & Fullstack Application (ESP32, HLK-LD2410C, PZEM-004T, Relays, Vue.js, Spring Boot, PostgreSQL, InfluxDB, Redis)
          </div>
          <div class="meta">
            Associated with Universitas Gunadarma
          </div>
        </div>

        <div class="sub-heading">for recruiters / search committees</div>
        <div style="font-size: 13px;">
          <a href="/cv_afwan_apriansyah_.pdf" target="_blank">cv</a>, 
          <a href="mailto:afwanapriansyah@gmail.com">email</a>, 
          <a href="https://github.com/nxstray" target="_blank">github</a>, 
          <a href="https://linkedin.com/in/afwan-apriansyah" target="_blank">linkedin</a>
        </div>
      </div>

      <!-- Column 2: Projects -->
      <div class="col">
        <div class="col-title">projects</div>

        <div class="entry">
          <div class="entry-title">
            <a href="https://github.com/nxstray/pppl-frontend" target="_blank">pandigi (pt. pandawa digital mandiri)</a>
          </div>
          <div class="entry-repo">
            <a href="https://github.com/nxstray/pppl-frontend" target="_blank">frontend</a> · 
            <a href="https://github.com/nxstray/pppl-backend" target="_blank">backend</a>
          </div>
          <div class="entry-sub">
            angular, spring boot, postgresql, docker, rabbitmq, redis — 2025 – 2026
          </div>
          <div class="entry-desc">enterprise web profile, request management, gemini ai lead scoring, rbac, rate limiting. (7th semester final project)</div>
        </div>

        <div class="entry">
          <div class="entry-title">
            <a href="https://github.com/nxstray/WebPI-frontend" target="_blank">gunadata — statistical analysis tool</a>
          </div>
          <div class="entry-repo">
            <a href="https://github.com/nxstray/WebPI-frontend" target="_blank">frontend</a> · 
            <a href="https://github.com/nxstray/WebPI-backend" target="_blank">backend</a>
          </div>
          <div class="entry-sub">
            angular, spring boot, mysql, docker — 2025
          </div>
          <div class="entry-desc">bivariate pearson correlation & anova calculation tool for student research. (scientific writing)</div>
        </div>

        <!-- Gemono AI Agent Assistant -->
        <div class="entry">
          <div class="entry-title">
            <a href="https://github.com/nxstray" target="_blank">gemono — ai agent assistant</a>
          </div>
          <div class="entry-sub">
            vue.js, spring boot, postgresql, redis — 2026
          </div>
          <div class="entry-desc">a web based ai agent with native tool-calling & SSE streaming. <br>(8th semester final project)</div>
        </div>

        <div class="entry">
          <div class="entry-title">
            <a href="https://github.com/nxstray/RPL2-ujian" target="_blank">car rental management system</a>
          </div>
          <div class="entry-sub">
            java, spring boot, swing, postgresql — 2024
          </div>
          <div class="entry-desc">desktop inventory management, customer registration & transaction tracking. (java practicum project)</div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: 'HomePage',
  data() {
    return {
      imgWidth: 0,
      imgHeight: 0
    }
  },
  methods: {
    handleImageLoad(event) {
      const img = event.target;
      this.imgWidth = img.naturalWidth;
      this.imgHeight = img.naturalHeight;
    }
  }
}
</script>

<!-- Style Non-Scoped untuk Custom Scrollbar Global Browser -->
<style>
::-webkit-scrollbar {
  width: 7px;
  height: 7px;
}
::-webkit-scrollbar-track {
  background: #f0f0f0;
}
::-webkit-scrollbar-thumb {
  background: #000000;
}
::-webkit-scrollbar-thumb:hover {
  background: #444444;
}
</style>

<style scoped>
.portfolio-wrapper {
  font-family: 'Times New Roman', Times, serif;
  font-size: 14px;
  line-height: 1.45;
  color: #000000;
  background-color: #ffffff;
  padding: 30px 40px;
  max-width: 900px;
  margin: 0;
}

a {
  color: #0000ee;
  text-decoration: underline;
}
a:visited {
  color: #551a8b;
}
a:hover {
  color: #0000ff;
}

/* Header Section & Profile Dimensions */
.header {
  display: flex;
  align-items: center;
  gap: 40px;
  margin-bottom: 16px;
}
.profile-box {
  display: flex;
  flex-direction: column;
  align-items: center;
  flex-shrink: 0;
}
.profile-img-row {
  display: flex;
  align-items: center;
  gap: 8px;
}
.profile-img {
  width: 96px;
  height: 128px;
  object-fit: cover;
  border: 1px solid #ccc;
  display: block;
}

.dim-label {
  font-size: 11px;
  color: #666;
  font-family: monospace, sans-serif;
}
.dim-top {
  margin-bottom: 3px;
}
.dim-bottom {
  margin-top: 3px;
}
.dim-left, .dim-right {
  writing-mode: vertical-rl;
  transform: rotate(180deg);
  display: inline-block;
  white-space: nowrap;
}

.header-content {
  padding-top: 2px;
}
.header-content h1 {
  font-size: 28px;
  font-weight: bold;
  color: #000;
  letter-spacing: -0.3px;
  margin-bottom: 4px;
}
.header-content .tagline {
  font-size: 13px;
  color: #333;
}

/* Dual Divider Lines */
hr.divider {
  border: none;
  border-top: 1px solid #000000;
  margin-bottom: 3px;
  width: 100%;
}
hr.divider:last-of-type {
  margin-bottom: 20px;
}

/* 2-Column Grid Layout */
.grid-container {
  display: grid;
  grid-template-columns: 1.1fr 1fr;
  gap: 40px;
}

.col-title {
  font-size: 14px;
  font-weight: bold;
  text-decoration: underline;
  text-transform: lowercase;
  margin-bottom: 12px;
}

/* Summary Table */
.summary-table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 16px;
}
.summary-table tr {
  border-bottom: 1px solid #e5e5e5;
}
.summary-table tr:last-child {
  border-bottom: none;
}
.summary-table td {
  padding: 5px 0;
  vertical-align: top;
}
.summary-table td.label {
  color: #666666;
  width: 75px;
  padding-right: 8px;
}

/* Featured Link / Thesis */
.featured-link {
  margin-top: 12px;
  margin-bottom: 16px;
}
.thesis-heading {
  font-size: 13px;
  margin-bottom: 2px;
}
.featured-link a {
  font-size: 14px;
  line-height: 1.35;
  display: inline-block;
}
.featured-link .meta {
  font-size: 12px;
  color: #666;
  margin-top: 3px;
}

.sub-heading {
  font-weight: bold;
  margin-top: 18px;
  margin-bottom: 4px;
}

/* Entry items */
.entry {
  margin-bottom: 15px;
}
.entry-title a {
  font-weight: normal;
}
.entry-repo {
  font-size: 12px;
  margin-top: 1px;
}
.entry-sub {
  font-size: 12px;
  color: #555555;
  margin-top: 1px;
}
.entry-desc {
  font-size: 13px;
  color: #333333;
  margin-top: 2px;
}

/* Responsive Breakpoints */
@media (max-width: 680px) {
  .portfolio-wrapper {
    padding: 15px;
  }
  .header {
    flex-direction: column;
    align-items: flex-start;
    gap: 12px;
  }
  .grid-container {
    grid-template-columns: 1fr;
  }
}
</style>