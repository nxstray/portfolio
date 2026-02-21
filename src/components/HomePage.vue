<template>
  <div class="homepage">
    <!-- Particle Background Animation -->
    <ParticleBackground />

    <!-- Background overlay (subtle, for white bg) -->
    <div class="background-overlay"></div>
    
    <!-- Header Navigation -->
    <div class="header-nav">
      <div class="nav-links">
        <a href="#" class="nav-link">Gmail</a>
        <a href="#" class="nav-link">Images</a>
      </div>
      <div class="nav-icons">
        <button class="nav-icon-btn lab-btn">
          <img :src="labIcon" alt="Lab" />
        </button>
        <button class="nav-icon-btn app-btn">
          <img :src="appIcon" alt="Apps" />
        </button>
        <button class="profile-btn" :class="{ active: showModal }" @click="toggleModal">
          <img :src="profileImg" alt="Profile" />
        </button>
      </div>
    </div>
    
    <div class="content">
      <!-- Logo/Title -->
      <img :src="googleLogo" alt="Portfolio" class="logo" />
      
      <!-- Search Bar -->
      <div class="search-container">
        <div class="search-bar">
          <button class="search-icon">
            <img :src="addIcon" alt="Add" />
          </button>
          <input 
            type="text" 
            placeholder="Ask Google or type a URL"
            class="search-input"
            @keyup.enter="handleSearch"
          />
          <div class="search-actions">
            <button class="action-btn">
              <img :src="micIcon" alt="Mic" />
            </button>
            <button class="action-btn">
              <img :src="cameraIcon" alt="Camera" />
            </button>
            <button class="ai-mode-btn">
              <img :src="searchSparkIcon" alt="AI" class="ai-icon" />
              <span class="ai-text">AI Mode</span>
            </button>
          </div>
        </div>
      </div>
      
      <!-- Shortcut Icons -->
      <div class="shortcuts">
        <a href="https://www.linkedin.com/in/afwan-apriansyah/" target="_blank" class="shortcut-item">
          <div class="shortcut-icon">
            <img :src="linkedinIcon" alt="LinkedIn" />
          </div>
          <span class="shortcut-label">LinkedIn</span>
        </a>
        
        <a href="https://github.com/nxstray" target="_blank" class="shortcut-item">
          <div class="shortcut-icon">
            <img :src="githubIcon" alt="Github" />
          </div>
          <span class="shortcut-label">Github</span>
        </a>
      </div>
    </div>
    
    <!-- Customize button -->
    <button class="customize-btn">
      <img :src="editIcon" alt="Edit" />
    </button>
    
    <!-- Profile Modal -->
    <Transition name="modal-fade">
      <div v-if="showModal" class="modal-overlay" @click="toggleModal">
        <div class="modal-content" @click.stop>
          <div class="modal-header">
            <span class="modal-email">afwanapriansyah@gmail.com</span>
            <button class="close-btn" @click="toggleModal">
              <img :src="closeIcon" alt="Close" />
            </button>
          </div>
          
          <div class="modal-body">
            <div class="profile-section">
              <div class="profile-photo">
                <img :src="profileImg" alt="Profile" />
                <button class="photo-edit-btn">
                  <img :src="photoIcon" alt="Edit Photo" />
                </button>
              </div>
              <h2 class="profile-name">Hi, Afwan Apriansyah!</h2>
            </div>
            
            <button class="manage-account-btn">
              Manage your Google Account
            </button>
          </div>
        </div>
      </div>
    </Transition>

    <!-- Projects Modal -->
    <Transition name="modal-fade">
      <div v-if="showProjectsModal" class="projects-modal-overlay" @click="closeProjectsModal">
        <div class="folder" @click.stop>
          <div class="tabs">
            <button 
              v-for="(tab, index) in projectTabs" 
              :key="index"
              :class="['tab', { active: activeTab === index }]"
              @click="openTab(index)"
            >
              <div>
                <span>{{ tab.name }}</span>
              </div>
            </button>
          </div>
          
          <div class="content-wrapper">
            <div 
              v-for="(tab, index) in projectTabs" 
              :key="index"
              :class="['content__inner', { active: activeTab === index }]"
            >
              <div class="page">
                <!-- Project Images Grid -->
                <div v-if="tab.images" class="project-gallery">
                  <!-- Main Image -->
                  <div class="gallery-main" @click="openLightbox(0)">
                    <img :src="tab.images[0]" :alt="`${tab.name} 1`" />
                  </div>
                  
                  <!-- More Images Indicator -->
                  <div v-if="tab.images.length > 1" class="gallery-more" @click="openLightbox(1)">
                    <img :src="tab.images[1]" alt="Preview" class="more-bg" />
                    <span class="more-text">+{{ tab.images.length - 1 }}</span>
                  </div>
                </div>

                <!-- Project Overview -->
                <div class="project-overview">
                  <div class="overview-header">
                    <div class="role-badge"
                      :style="{ backgroundColor: currentBadgeColor.bg, color: currentBadgeColor.text }"
                      @mouseenter="randomizeBadgeColor"
                      @mouseleave="resetBadgeColor"
                    >
                      {{ tab.role }}
                    </div>
                  </div>
                  <p class="overview-text">{{ tab.overview }}</p>
                </div>

                <!-- Skills & Competencies -->
                <div v-if="tab.skills" class="project-skills">
                  <div class="skills-header">Key Skills Demonstrated</div>
                  <div class="skills-grid">
                    <div 
                      v-for="(skill, skillIndex) in tab.skills" 
                      :key="skillIndex"
                      class="skill-item"
                      :class="{ 'skill-active': isSkillActive(tab.name, skillIndex) }"
                      @mouseenter="toggleSkill(tab.name, skillIndex)"
                    >
                      <img :src="checkIcon" alt="Check" class="skill-check" />
                      <span>{{ skill }}</span>
                    </div>
                  </div>
                </div>
                
                <!-- Tools Used -->
                <div class="project-tools">
                  <strong>Tools:</strong>
                  <div class="tools-list">
                    <span 
                      v-for="(tool, toolIndex) in tab.tools" 
                      :key="toolIndex" 
                      class="tool-badge"
                      @mouseenter="$event.currentTarget.style.backgroundColor = getToolColor(tool) + '22'; $event.currentTarget.style.borderColor = getToolColor(tool)"
                      @mouseleave="$event.currentTarget.style.backgroundColor = ''; $event.currentTarget.style.borderColor = ''"
                    >
                      <span class="tool-dot" :style="{ backgroundColor: getToolColor(tool) }"></span>
                      {{ tool }}
                    </span>
                  </div>
                </div>

                <!-- GitHub Repositories -->
                <div v-if="tab.repos" class="project-repos">
                  <div 
                    v-for="(repo, repoIndex) in tab.repos" 
                    :key="repoIndex"
                    :href="repo.isPrivate ? '#' : repo.url"
                    :target="repo.isPrivate ? '_self' : '_blank'"
                    :class="['repo-card', { 'has-cat': repo.name.toLowerCase().includes('backend') }]"
                    @click="repo.isPrivate ? $event.preventDefault() : null"
                  >
                    <!-- Sleeping Cat Animation (only for backend repo) -->
                    <div v-if="repo.name.toLowerCase().includes('backend')" class="sleeping-cat">
                      <div class="sleep-symbol">
                        <span>Z</span>
                        <span>z</span>
                        <span>z</span>
                      </div>
                      <div class="cat-svg">
                        <svg width="45px" height="35px" viewBox="0 0 45.952225 35.678726" xmlns="http://www.w3.org/2000/svg">
                          <g transform="translate(-121.80376,-101.90461)">
                            <path style="fill:#000000" d="m 144.95859,104.74193 c 6.01466,-2.1201 14.02915,-0.85215 17.62787,2.77812 3.59872,3.63027 2.91927,7.6226 -0.0661,11.80703 -2.98542,4.18443 -9.54667,3.58363 -15.1474,3.43959 -5.60073,-0.14404 -10.30411,-0.0586 -11.67474,-3.9026 7.85671,-2.22341 3.24576,-12.00205 9.26042,-14.12214 z"/>
                            <path style="fill:#000000" d="m 156.30732,121.30486 c 0,0 -3.82398,2.52741 -4.14054,3.7997 -0.31656,1.2723 0.31438,2.18109 0.95701,2.55128 0.64264,0.3702 1.59106,-0.085 2.13559,-0.75306 0.54452,-0.6681 1.5629,-2.25488 2.47945,-3.20579 0.91654,-0.95091 2.96407,-2.74361 2.96407,-2.74361 l 0.73711,-3.60348 z"/>
                            <path style="fill:#000000" d="m 136.93356,123.08347 c 0,0 -3.20149,3.2804 -3.24123,4.59088 -0.0397,1.31049 0.60411,1.83341 1.3106,2.05901 0.7065,0.22559 1.60304,-0.55255 1.99363,-1.32084 0.39056,-0.76832 1.14875,-2.30337 2.04139,-3.29463 0.89264,-0.99126 3.37363,-3.37561 3.37363,-3.37561 l -1.30007,-3.61169 z"/>
                            <path style="fill:#000000" d="m 130.12859,121.60522 c -2.15849,1.92962 -3.38576,3.23532 -3.61836,4.5256 -0.23257,1.2903 0.0956,1.80324 0.76105,2.13059 0.66549,0.32733 1.66701,-0.31006 2.16665,-1.01233 0.49961,-0.70231 1.04598,-1.14963 2.83575,-3.05671 1.78977,-1.90708 5.91823,-3.27102 5.91823,-3.27102 l -0.75313,-3.99546 c 0,0 -5.15171,2.7497 -7.31019,4.67933 z"/>
                            <path style="fill:#000000" d="m 147.59927,113.85404 c 0.68896,4.40837 -4.04042,7.93759 -10.51533,8.9455 -6.47491,1.00791 -12.24344,-0.88717 -12.9324,-5.29555 -0.68895,-4.40838 3.44199,-9.94186 9.9169,-10.94977 6.47491,-1.0079 12.84186,2.89144 13.53083,7.29982 z"/>
                            <path style="fill:#000000" d="m 126.36446,111.82609 c 0,0 -2.37067,-6.28072 -0.86724,-7.10855 1.50342,-0.82783 5.87139,3.72617 5.87139,3.72617 z"/>
                            <path style="fill:#000000" d="m 143.50182,108.85407 c 0,0 -0.0544,-6.71302 -1.75519,-6.94283 -1.70081,-0.22982 -4.13211,5.59314 -4.13211,5.59314 z"/>
                            <g>
                              <path style="fill:none;stroke:#000000;stroke-width:0.529167" d="m 125.27102,116.06007 -2.97783,-1.05373"/>
                              <path style="fill:none;stroke:#000000;stroke-width:0.529167" d="m 124.91643,116.80991 -2.84808,0.0754"/>
                              <path style="fill:none;stroke:#000000;stroke-width:0.529167" d="m 124.97798,118.00308 -2.53111,0.5156"/>
                            </g>
                            <g transform="rotate(-23.188815,49.755584,71.047761)" style="fill:none;stroke:#000000">
                              <path style="fill:none;stroke:#000000;stroke-width:0.529167" d="m 121.77448,146.87682 3.00963,-0.95912"/>
                              <path style="fill:none;stroke:#000000;stroke-width:0.529167" d="m 122.10521,147.63749 2.84427,0.16537"/>
                              <path style="fill:none;stroke:#000000;stroke-width:0.529167" d="m 122.00599,148.82812 2.51354,0.59531"/>
                            </g>
                            <ellipse style="fill:#ffffff" cx="142.61723" cy="108.6707" rx="3.0261719" ry="3.0757811" transform="rotate(1.8105864)"/>
                            <ellipse style="fill:#000000" cx="112.57543" cy="138.29808" rx="1.0380507" ry="1.3097118" transform="matrix(0.98048242,-0.19660678,0.20800608,0.97812753,0,0)"/>
                            <ellipse style="fill:#f9f9f9" cx="112.70263" cy="137.817" rx="0.32146212" ry="0.40558979" transform="matrix(0.98048242,-0.19660678,0.20800608,0.97812753,0,0)"/>
                            <ellipse style="fill:#ffffff" cx="135.40735" cy="110.12592" rx="3.0261719" ry="3.0757811" transform="rotate(1.8105864)"/>
                            <ellipse style="fill:#000000" cx="105.22613" cy="138.07497" rx="1.0380507" ry="1.3097118" transform="matrix(0.98048242,-0.19660678,0.20800608,0.97812753,0,0)"/>
                            <ellipse style="fill:#f9f9f9" cx="105.35332" cy="137.59389" rx="0.32146212" ry="0.40558979" transform="matrix(0.98048242,-0.19660678,0.20800608,0.97812753,0,0)"/>
                            <path id="tail" style="fill:#000000" d="m 163.77708,109.27292 c 4.36563,2.71198 4.26447,17.63497 3.70417,21.03437 -0.5603,3.3994 -1.86906,4.06275 -4.53099,4.49791 -5.87463,0.96037 -8.39724,-5.87134 -5.7547,-5.72161 2.64254,0.14973 3.15958,3.46446 5.95314,2.05052 2.79356,-1.41394 -1.42214,-13.46068 -1.42214,-13.46068 z"/>
                            <path style="fill:#000000" d="m 159.74981,121.34445 c 0,0 -2.98896,3.47517 -2.94624,4.78555 0.0427,1.31039 0.89775,2.01247 1.61702,2.1932 0.71928,0.18075 1.50745,-0.51603 1.84897,-1.30735 0.34149,-0.79135 0.88811,-2.59584 1.51032,-3.76081 0.62219,-1.16497 2.10268,-3.44845 2.10268,-3.44845 l -0.27441,-3.66785 z"/>
                            <g id="lefteyelid">
                              <ellipse style="fill:#000000" cx="131.94429" cy="114.29948" rx="3.1571214" ry="3.2155864"/>
                              <path style="fill:#000000;stroke:#ffffff;stroke-width:0.529167" d="m 129.32504,114.80228 c 2.54908,-1.14592 4.60706,-0.65481 4.60706,-0.65481"/>
                            </g>
                            <g id="righteyelid">
                              <ellipse style="fill:#000000" cx="139.07704" cy="113.0834" rx="3.1571214" ry="3.2155864"/>
                              <path style="fill:#000000;stroke:#ffffff;stroke-width:0.529167" d="m 136.48089,113.70683 c 2.48528,-1.2784 4.56624,-0.89621 4.56624,-0.89621"/>
                            </g>
                            <g id="eyesdown">
                              <ellipse style="fill:#ffffff" cx="139.12122" cy="113.61373" rx="1.8686198" ry="2.0422525"/>
                              <ellipse style="fill:#000000" cx="112.24622" cy="139.77037" rx="1.0380507" ry="1.3097118" transform="matrix(0.98048242,-0.19660678,0.20800608,0.97812753,0,0)"/>
                              <ellipse style="fill:#f9f9f9" cx="112.37342" cy="139.28929" rx="0.32146212" ry="0.40558979" transform="matrix(0.98048242,-0.19660678,0.20800608,0.97812753,0,0)"/>
                              <ellipse style="fill:#ffffff" cx="131.994" cy="114.92011" rx="1.8686198" ry="2.0422525"/>
                              <ellipse style="fill:#000000" cx="105.00267" cy="139.64998" rx="1.0380507" ry="1.3097118" transform="matrix(0.98048242,-0.19660678,0.20800608,0.97812753,0,0)"/>
                              <ellipse style="fill:#f9f9f9" cx="105.12987" cy="139.1689" rx="0.32146212" ry="0.40558979" transform="matrix(0.98048242,-0.19660678,0.20800608,0.97812753,0,0)"/>
                            </g>
                          </g>
                        </svg>
                      </div>
                    </div>
                    <div class="repo-header">
                      <img :src="githubIcon" alt="GitHub" class="repo-icon" />
                      <a 
                        class="repo-name" 
                        style="margin-right: auto;"
                        :href="repo.isPrivate ? '#' : repo.url"
                        :target="repo.isPrivate ? '_self' : '_blank'" 
                        @click="repo.isPrivate ? $$event.preventDefault() : null"
                      >{{ repo.name }}</a>
                      <span class="repo-badge">{{ repo.isPrivate ? 'Private' : 'Public' }}</span>
                    </div>
                    <div class="repo-language">
                      <span class="language-dot" :style="{ backgroundColor: getToolColor(repo.language) }"></span>
                      <span class="language-name">{{ repo.language }}</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </Transition>

    <!-- Notification Popup -->
    <Transition name="slide">
      <div v-if="showNotification" class="notification-popup">
        <div class="notification-progress"></div>
        <img :src="helpIcon" alt="Help" class="notification-icon" />
        <div class="notification-content">
          <div class="notification-title">Tips</div>
          <div class="notification-message">
            Try to type 'download cv' or 'projects'.
          </div>
        </div>
        <button class="notification-close" @click="closeNotification">
          <img :src="closeIcon" alt="Close" />
        </button>
      </div>
    </Transition>

    <!-- Lightbox Modal -->
    <Transition name="modal-fade">
      <div v-if="showLightbox" class="lightbox-overlay" @click="closeLightbox">
        <button class="lightbox-close" @click="closeLightbox">
          <img :src="closeIcon" alt="Close" />
        </button>
        <button v-if="currentImageIndex > 0" class="lightbox-nav prev" @click.stop="prevImage">
          <img :src="arrowBackIcon" alt="Previous" />
        </button>
        <div class="lightbox-content" @click.stop>
          <img :src="projectTabs[activeTab].images[currentImageIndex]" alt="Full size image" />
          <div class="lightbox-counter">{{ currentImageIndex + 1 }} / {{ projectTabs[activeTab].images.length }}</div>
        </div>
        <button v-if="currentImageIndex < projectTabs[activeTab].images.length - 1" class="lightbox-nav next" @click.stop="nextImage">
          <img :src="arrowForwardIcon" alt="Next" />
        </button>
      </div>
    </Transition>

    <!-- Footer -->
    <footer class="footer">
      <span class="footer-text">
        © Google | Tab overlay inspired by 
        <a href="https://codepen.io/oliviale/pen/bGWXEWK" target="_blank" rel="noopener noreferrer">Olivia Ng</a>
        | Sleeping cat inspired by 
        <a href="https://codepen.io/scjaabkw-the-looper/pen/JoXKvwP" target="_blank" rel="noopener noreferrer">Marcel</a>
      </span>
    </footer>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import ParticleBackground from '@/components/ParticleBackground.vue'

import googleLogo from '@/assets/main/google.png'
import profileImg from '@/assets/main/profile.jpg'

import addIcon from '@/assets/icons/add.svg'
import micIcon from '@/assets/icons/mic.svg'
import appIcon from '@/assets/icons/app.svg'
import labIcon from '@/assets/icons/labs.svg'
import helpIcon from '@/assets/icons/help.svg'
import editIcon from '@/assets/icons/edit.svg'
import checkIcon from '@/assets/icons/check.svg'
import photoIcon from '@/assets/icons/photo.svg'
import closeIcon from '@/assets/icons/close.svg'
import cameraIcon from '@/assets/icons/camera.svg'
import arrowBackIcon from '@/assets/icons/arrow-back.svg'
import arrowForwardIcon from '@/assets/icons/arrow-forward.svg'
import searchSparkIcon from '@/assets/icons/search_spark.svg'

import linkedinIcon from '@/assets/links/linkedin.png'
import githubIcon from '@/assets/links/github.png'

import gunadata1 from '@/assets/contents/gunadata-1.png'
import gunadata2 from '@/assets/contents/gunadata-2.png'
import gunadata3 from '@/assets/contents/gunadata-3.png'
import gunadata4 from '@/assets/contents/gunadata-4.png'
import gunadata5 from '@/assets/contents/gunadata-5.png'
import gunadata6 from '@/assets/contents/gunadata-6.png'

import pandigi1 from '@/assets/contents/pandigi-1.png'
import pandigi2 from '@/assets/contents/pandigi-2.png'
import pandigi3 from '@/assets/contents/pandigi-3.png'
import pandigi4 from '@/assets/contents/pandigi-4.png'
import pandigi5 from '@/assets/contents/pandigi-5.png'
import pandigi6 from '@/assets/contents/pandigi-6.png'

import carrental1 from '@/assets/contents/carrental-1.png'
import carrental2 from '@/assets/contents/carrental-2.png'
import carrental3 from '@/assets/contents/carrental-3.png'

const showModal = ref(false)
const showNotification = ref(false)
const showProjectsModal = ref(false)
const activeTab = ref(0)
const activeSkills = ref({})

const showLightbox = ref(false)
const currentImageIndex = ref(0)

const toolColors = {
  'Angular': '#dd0031',
  'Spring Boot': '#6db33f',
  'MySQL Workbench': '#00758f',
  'PostgreSQL': '#4169e1',
  'Docker': '#2496ed',
  'RabbitMQ': '#ff6600',
  'React': '#61dafb',
  'Vue': '#42b883',
  'Node.js': '#339933',
  'Python': '#3776ab',
  'Java': '#b07219',
  'JavaScript': '#f7df1e',
  'TypeScript': '#3178c6'
}

const badgeColors = [
  { bg: '#e8f5e9', text: '#2e7d32' },
  { bg: '#e3f2fd', text: '#1565c0' },
  { bg: '#fce4ec', text: '#c62828' },
  { bg: '#fff8e1', text: '#f57f17' },
  { bg: '#f3e5f5', text: '#6a1b9a' },
  { bg: '#e0f7fa', text: '#00695c' },
]
const currentBadgeColor = ref({ bg: '#dadadb', text: '#3a3939' })

const randomizeBadgeColor = () => {
  const pick = badgeColors[Math.floor(Math.random() * badgeColors.length)]
  currentBadgeColor.value = pick
}
const resetBadgeColor = () => {
  currentBadgeColor.value = { bg: '#dadadb', text: '#3a3939' }
}

const getToolColor = (tool) => {
  return toolColors[tool] || '#666666'
}

const openLightbox = (index) => {
  currentImageIndex.value = index
  showLightbox.value = true
}

const closeLightbox = () => {
  showLightbox.value = false
}

const nextImage = () => {
  if (currentImageIndex.value < projectTabs.value[activeTab.value].images.length - 1) {
    currentImageIndex.value++
  }
}

const prevImage = () => {
  if (currentImageIndex.value > 0) {
    currentImageIndex.value--
  }
}

const projectTabs = ref([
  {
    name: 'GunaData',
    images: [gunadata1, gunadata2, gunadata3, gunadata4, gunadata5, gunadata6],
    overview: 'A statistical analysis tool designed for students to simplify complex calculations in Bivariate Pearson Correlation and One-Way ANOVA methods, featuring automated computation and clean data visualization.',
    role: 'Full Stack Developer',
    skills: [
      'User-Friendly Interface Design',
      'RESTful API',
      'Database Design & Management',
      'Form Validation & State Management',
    ],
    tools: ['Angular', 'Spring Boot', 'MySQL Workbench', 'Docker'],
    repos: [
      { name: 'WebPI-frontend', url: 'https://github.com/nxstray/WebPI-frontend', language: 'TypeScript' },
      { name: 'WebPI-backend', url: 'https://github.com/nxstray/WebPI-backend', language: 'Java' }
    ]
  },
  {
    name: 'Pandigi',
    images: [pandigi1, pandigi2, pandigi3, pandigi4, pandigi5, pandigi6],
    overview: 'A comprehensive client and service request management system with AI-powered lead scoring capabilities. Features real-time notifications, role-based access control, caching, rate limiting, and intelligent lead prioritization using Google Gemini AI to optimize sales team workflow.',
    role: 'Full Stack Developer',
    skills: [
      'Responsive UI/UX Design',
      'RESTful API',
      'Database Design & Management',
      'JWT Authentication & Authorization',
      'Message Broker & Cache Integration',
      'AI/ML Implementation'
    ],
    tools: ['Angular', 'Spring Boot', 'PostgreSQL', 'Docker', 'RabbitMQ'],
    repos: [
      { name: 'pppl-frontend', url: 'https://github.com/nxstray/pppl-frontend', language: 'TypeScript' },
      { name: 'pppl-backend', url: 'https://github.com/nxstray/pppl-backend', language: 'Java' }
    ],
  },
  {
    name: 'Car Rental',
    images: [carrental1, carrental2, carrental3],
    overview: 'A car rental management system designed to handle vehicle inventory, customer registration, rental transaction processing, and featuring an intuitive desktop GUI.',
    role: 'Backend Developer',
    skills: [
      'Desktop Application Development',
      'CRUD Operations',
      'Database Design & Management'
    ],
    tools: ['Spring Boot', 'PostgreSQL'],
    repos: [
      { name: 'RPL2-ujian', url: '#', language: 'Java', isPrivate: true }
    ]
  }
])

const toggleModal = () => {
  showModal.value = !showModal.value
}

const closeNotification = () => {
  showNotification.value = false
}

const closeProjectsModal = () => {
  showProjectsModal.value = false
  activeTab.value = 0
  activeSkills.value = {}
}

const openTab = (index) => {
  activeTab.value = index
  activeSkills.value = {}
}

const toggleSkill = (tabName, skillIndex) => {
  const key = `${tabName}-${skillIndex}`
  activeSkills.value[key] = true
}

const isSkillActive = (tabName, skillIndex) => {
  return activeSkills.value[`${tabName}-${skillIndex}`] || false
}

onMounted(() => {
  setTimeout(() => {
    showNotification.value = true
    
    setTimeout(() => {
      showNotification.value = false
    }, 10000)
  }, 2000)
})

const handleSearch = (event) => {
  const query = event.target.value.toLowerCase().trim()
  
  if (query === 'download cv') {
    downloadCV()
    event.target.value = ''
  } else if (query === 'projects') {
    showProjectsModal.value = true
    event.target.value = ''
  }
}

const downloadCV = () => {
  const link = document.createElement('a')
  link.href = '/CV_Afwan_Apriansyah_.pdf'
  link.download = 'Afwan_Apriansyah_CV.pdf'
  document.body.appendChild(link)
  link.click()
  document.body.removeChild(link)

  showNotification.value = false
}
</script>

<style scoped>
/* Mobile Styles */

/* Modal Fade Transition */
.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.3s ease;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
}

.modal-fade-enter-active .modal-content,
.modal-fade-enter-active .folder {
  transition: transform 0.3s ease, opacity 0.3s ease;
}

.modal-fade-enter-from .modal-content,
.modal-fade-enter-from .folder {
  transform: scale(0.9);
  opacity: 0;
}

.modal-fade-leave-to .modal-content,
.modal-fade-leave-to .folder {
  transform: scale(0.9);
  opacity: 0;
}

/* Notification Popup */
.notification-popup {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  background: white;
  border-radius: 8px;
  padding: 12px 16px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.2);
  max-width: calc(100% - 40px);
  width: 320px;
  z-index: 2000;
  display: flex;
  align-items: center;
  gap: 12px;
  overflow: hidden;
}

.notification-progress {
  position: absolute;
  bottom: 0;
  left: 0;
  height: 3px;
  background: linear-gradient(90deg, #4285f4, #1a73e8);
  width: 100%;
  animation: progressBar 10s linear forwards;
  border-radius: 16px 0 8px 8px;
}

@keyframes progressBar {
  from { width: 100%; }
  to { width: 0%; }
}

.slide-enter-active,
.slide-leave-active {
  transition: all 0.3s ease;
}

.slide-enter-from {
  transform: translate(-50%, 100px);
  opacity: 0;
}

.slide-leave-to {
  transform: translate(-50%, 100px);
  opacity: 0;
}

.notification-icon {
  width: 20px;
  height: 20px;
  flex-shrink: 0;
  filter: brightness(0) saturate(100%) invert(42%) sepia(93%) saturate(1352%) hue-rotate(202deg) brightness(97%) contrast(96%);
}

.notification-content {
  flex: 1;
}

.notification-title {
  font-size: 13px;
  font-weight: 500;
  color: #202124;
  margin-bottom: 4px;
}

.notification-message {
  font-size: 12px;
  color: #5f6368;
  line-height: 1.4;
}

.notification-close {
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s;
  flex-shrink: 0;
}

.notification-close:hover {
  background: rgba(0, 0, 0, 0.08);
}

.notification-close img {
  width: 18px;
  height: 18px;
  filter: brightness(0) saturate(100%) opacity(0.6);
}

.homepage {
  position: relative;
  width: 100%;
  min-height: 100vh;
  background-color: #ffffff;
  overflow-x: hidden;
}

.background-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  pointer-events: none;
}

.content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 60px 15px 40px;
}

.logo {
  height: 100px !important;
  max-width: 400px !important;
  margin-top: 30px !important;
  margin-bottom: 50px !important;
}

.search-container {
  width: 100%;
  max-width: 550px;
  margin-bottom: 25px;
  padding: 0 10px;
}

.search-bar {
  display: flex;
  align-items: center;
  background: rgb(255, 255, 255);
  border-radius: 30px;
  padding: 5px 8px;
  border: 1px solid #dfe1e5;
  transition: box-shadow 0.2s;
}

.search-bar:hover {
  box-shadow: 0 1px 6px rgba(32, 33, 36, 0.28);
  border-color: rgba(223, 225, 229, 0);
}

.search-icon {
  background: none;
  border: none;
  cursor: pointer;
  margin-right: 8px;
  padding: 6px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  transition: background 0.2s;
  flex-shrink: 0;
}

.search-icon:hover {
  background: rgba(60, 64, 67, 0.08);
}

.search-icon img {
  width: 18px;
  height: 18px;
  filter: brightness(0) saturate(100%);
}

.search-input {
  flex: 1;
  border: none;
  outline: none;
  font-size: 14px;
  background: transparent;
  color: #333333;
  font-family: 'Segoe UI', Tahoma, sans-serif;
  min-width: 0;
}

.search-input::placeholder {
  color: #999999d3;
  font-size: 13px;
}

.search-actions {
  display: flex;
  align-items: center;
  gap: 4px;
  flex-shrink: 0;
}

.action-btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 6px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  transition: background 0.2s;
}

.action-btn:hover {
  background: rgba(60, 64, 67, 0.08);
}

.action-btn img {
  width: 16px;
  height: 16px;
  filter: brightness(0) saturate(100%);
}

.ai-mode-btn {
  background: rgb(243, 246, 246);
  border: 2px solid transparent;
  padding: 6px 10px 6px 6px;
  border-radius: 20px;
  font-size: 11px;
  font-weight: 600;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 4px;
  color: #353535;
  position: relative;
  z-index: 1;
  background-clip: padding-box;
  font-family: 'Segoe UI', Tahoma, sans-serif;
  white-space: nowrap;
}

.ai-mode-btn::before {
  content: '';
  position: absolute;
  inset: -2px;
  border-radius: 18px;
  background: linear-gradient(90deg, #4285f4 0%, #ea4335 25%, #fbbc04 50%, #34a853 75%, #4285f4 100%);
  background-size: 200% 100%;
  mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
  -webkit-mask-composite: xor;
  mask-composite: exclude;
  padding: 2px;
  z-index: -1;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.ai-mode-btn:hover::before {
  opacity: 1;
  animation: flowBorder 2s linear infinite;
}

@keyframes flowBorder {
  0% { background-position: 0% 0%; }
  100% { background-position: 200% 0%; }
}

.ai-icon {
  width: 14px;
  height: 14px;
  filter: brightness(0) saturate(100%);
  flex-shrink: 0;
}

.ai-text {
  display: none;
}

.shortcuts {
  display: flex;
  gap: 35px;
  justify-content: center;
  max-width: 1000px;
  margin-bottom: 20px;
}

.shortcut-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-decoration: none;
  transition: transform 0.2s;
  position: relative;
}

.shortcut-item::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 70px;
  height: 70px;
  background: rgba(0, 0, 0, 0.05);
  border-radius: 8px;
  opacity: 0;
  transition: opacity 0.2s;
  z-index: 0;
}

.shortcut-item:hover::before {
  opacity: 1;
}

.shortcut-icon {
  width: 38px;
  height: 38px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 6px;
  background: rgba(228, 226, 227, 1.00);
  position: relative;
  z-index: 1;
}

.shortcut-icon img {
  width: 20px;
  height: 20px;
  object-fit: contain;
}

.shortcut-label {
  font-size: 12px;
  color: #5f6368;
  text-align: center;
  position: relative;
  z-index: 1;
}

.customize-btn {
  position: fixed;
  bottom: 12px;
  right: 12px;
  width: 28px;
  height: 28px;
  border-radius: 50%;
  background: rgba(0, 0, 0, 0.15);
  border: none;
  cursor: pointer;
  transition: background 0.2s;
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.customize-btn:hover {
  background: rgba(0, 0, 0, 0.25);
}

.customize-btn img {
  width: 15px;
  height: 15px;
  filter: brightness(0) saturate(100%);
}

/* Header Navigation */
.header-nav {
  position: absolute;
  top: 0;
  right: 0;
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 10px 12px;
  z-index: 100;
}

.nav-links {
  display: none;
}

.nav-icons {
  display: flex;
  align-items: center;
  gap: 4px;
}

.nav-icon-btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 6px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s;
  width: 34px;
  height: 34px;
}

.nav-icon-btn:hover {
  background: rgba(0, 0, 0, 0.06);
}

.lab-btn img {
  width: 15px;
  height: 15px;
  filter: brightness(0) saturate(100%) opacity(0.7);
}

.app-btn img {
  width: 22px;
  height: 22px;
  filter: brightness(0) saturate(100%) opacity(0.7);
}

.profile-btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  width: 28px;
  height: 28px;
  border-radius: 50%;
  overflow: visible;
  transition: all 0.2s;
  position: relative;
}

.profile-btn::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 28px;
  height: 28px;
  background: rgba(0, 0, 0, 0.08);
  border-radius: 50%;
  opacity: 0;
  transition: all 0.3s ease;
  z-index: 0;
}

.profile-btn:hover::before,
.profile-btn.active::before {
  width: 36px;
  height: 36px;
  opacity: 1;
}

.profile-btn img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  position: relative;
  z-index: 1;
  border-radius: 50%;
}

/* Modal */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 1000;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: 55px 15px 15px;
}

.modal-content {
  background: rgb(235, 238, 241);
  border-radius: 16px;
  width: 100%;
  max-width: 340px;
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.3);
  overflow: hidden;
}

.modal-header {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px 16px;
  border-bottom: none;
}

.modal-email {
  font-size: 12px;
  font-weight: 500;
  color: #202124;
  text-align: center;
}

.close-btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 6px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s;
  margin: -6px;
  position: absolute;
  right: 12px;
}

.close-btn:hover {
  background: rgba(0, 0, 0, 0.08);
}

.close-btn img {
  width: 18px;
  height: 18px;
  filter: brightness(0) saturate(100%) opacity(0.6);
}

.modal-body {
  padding: 20px 16px 24px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.profile-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 16px;
}

.profile-photo {
  position: relative;
  width: 70px;
  height: 70px;
  margin-bottom: 14px;
}

.profile-photo img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  object-fit: cover;
}

.photo-edit-btn {
  position: absolute;
  bottom: 0;
  right: 0;
  background: white;
  border: 2px solid white;
  border-radius: 50%;
  width: 26px;
  height: 26px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
  transition: background 0.2s;
}

.photo-edit-btn:hover {
  background: rgba(60, 64, 67, 0.08);
}

.photo-edit-btn img {
  width: 14px;
  height: 14px;
  filter: brightness(0) saturate(100%);
}

.profile-name {
  font-size: 18px;
  font-weight: 400;
  line-height: 0.5rem;
  color: #202124;
  text-align: center;
  font-family: 'Google Sans', 'Roboto', sans-serif;
}

.manage-account-btn {
  background: rgb(235, 238, 241);
  border: 1px solid #797979;
  border-radius: 24px;
  padding: 8px 20px;
  font-size: 12px;
  color: #055bcc;
  font-weight: 500;
  cursor: pointer;
  margin-top: 1px;
  font-family: 'Segoe UI', Tahoma, sans-serif;
  transition: background 0.2s, box-shadow 0.2s;
}

.manage-account-btn:hover {
  background: #c8dbef;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}

/* Projects Modal */
.projects-modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: 2000;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  overflow-y: auto;
}

.folder {
  margin: 0 auto;
  width: 100%;
  max-width: 100%;
  max-height: calc(100vh - 30px);
  overflow-y: auto;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.folder::-webkit-scrollbar {
  display: none;
}

.tabs {
  padding: 1.5rem 0 0 0;
  width: calc(100% - 2rem);
  margin: 0 1rem;
  overflow-x: auto;
  white-space: nowrap;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.tabs::-webkit-scrollbar {
  display: none;
  height: 0;
}

.tab {
  font-family: 'Segoe UI', Tahoma, sans-serif;
  line-height: 0.8;
  display: inline-block;
  margin-left: -25px;
  filter: drop-shadow(0px -2px 2px rgba(0, 0, 0, 0.05));
  border: none;
  border-radius: 6px 6px 0 0;
  position: relative;
  margin-right: 2rem;
  background: linear-gradient(to bottom, #fee9a5, #f9d877);
  white-space: nowrap;
  cursor: pointer;
  font-size: 90%;
}

.tab:focus { outline: none; }

.tab:focus span {
  border-bottom: 2px solid;
  border-radius: 0;
}

.tab:first-of-type { margin-left: 20px; }

.tab div {
  background: linear-gradient(to bottom, #fee9a5, #f9d877);
  padding: 5px 0;
  position: relative;
  z-index: 10;
}

.tab span {
  display: inline-block;
  border: 2px solid transparent;
  padding: 5px 12px 5px;
  border-radius: 5px;
  z-index: 5;
  position: relative;
  font-size: 120%;
  color: black;
  min-width: 5rem;
}

.tab:before,
.tab:after {
  content: "";
  height: 100%;
  position: absolute;
  background: linear-gradient(to bottom, #fee9a5, #f9d877);
  border-radius: 6px 6px 0 0;
  width: 25px;
  top: 0;
}

.tab:before {
  right: -14px;
  transform: skew(25deg);
  border-radius: 0 6px 0 0;
}

.tab:after {
  transform: skew(-25deg);
  left: -14px;
  border-radius: 6px 0 0 0;
}

.tab.active {
  z-index: 50;
  position: relative;
}

.tab:nth-child(2) div,
.tab:nth-child(2):before,
.tab:nth-child(2):after {
  background: linear-gradient(to bottom, #a8e6a1, #76c776);
}

.tab:nth-child(3) div,
.tab:nth-child(3):before,
.tab:nth-child(3):after {
  background: linear-gradient(to bottom, #a1c9e6, #6ba5d6);
}

.content-wrapper {
  border-radius: 10px;
  position: relative;
  width: 100%;
}

.content__inner {
  display: none;
  background: linear-gradient(to bottom, #f9d877, #fee9a5);
  border-radius: 10px;
  padding: 0.8rem;
  filter: drop-shadow(0px -2px 2px rgba(0, 0, 0, 0.1));
  z-index: 5;
}

.content__inner.active { display: block; }

.content__inner:nth-child(2).active {
  background: linear-gradient(to bottom, #76c776, #a8e6a1) !important;
}

.content__inner:nth-child(3).active {
  background: linear-gradient(to bottom, #6ba5d6, #a1c9e6) !important;
}

.page {
  padding: 1rem;
  border-radius: 2px;
  min-height: 15rem;
  line-height: 150%;
  background-color: #f9f9f9;
  filter: drop-shadow(0px 1px 3px rgba(0, 0, 0, 0.15));
  font-family: 'Segoe UI', Tahoma, sans-serif;
}

.project-gallery {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 1rem;
}

.gallery-main {
  cursor: pointer;
  border-radius: 6px;
  overflow: hidden;
  background: #e0e0e0;
  width: 100%;
  height: 140px;
  transition: transform 0.2s;
  border: 2px solid #66676a;
}

.gallery-main img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.gallery-more {
  width: 100%;
  height: 100px;
  background: #e0e0e0;
  border-radius: 6px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.2s;
  position: relative;
  overflow: hidden;
  border: 2px solid #66676a;
}

.gallery-more:hover { background: #d0d0d0; }

.more-bg {
  position: absolute;
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: brightness(0.6);
  z-index: 1;
}

.more-text {
  font-size: 1.4rem;
  font-weight: 500;
  color: rgb(243, 240, 240);
  font-family: 'Segoe UI', Tahoma, sans-serif;
  position: relative;
  z-index: 2;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.237);
}

.project-overview {
  margin-bottom: 1rem;
  padding: 12px;
  background: white;
  border-radius: 6px;
  border: 1px solid #e0e0e0;
}

.overview-header {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.role-badge {
  display: flex;
  align-items: center;
  gap: 6px;
  background: #dadadb;
  color: #3a3939;
  padding: 5px 10px;
  border-radius: 14px;
  font-size: 11px;
  font-weight: 500;
  position: relative;
  overflow: hidden;
  cursor: default;
  transition: background 0.4s, color 0.4s;
}

.role-badge::before {
  content: '';
  display: block;
  position: absolute;
  background: rgba(255,255,255,0.9);
  width: 80px;
  height: 100%;
  left: 0;
  top: 0;
  opacity: 0.5;
  filter: blur(15px);
  transform: translateX(-100px) skewX(-15deg);
}

.role-badge::after {
  content: '';
  display: block;
  position: absolute;
  background: rgba(255,255,255,0.7);
  width: 30px;
  height: 100%;
  left: 30px;
  top: 0;
  opacity: 0;
  filter: blur(3px);
  transform: translateX(-100px) skewX(-15deg);
}

.role-badge:hover::before {
  transform: translateX(300px) skewX(-15deg);
  opacity: 0.6;
  transition: 0.7s;
}

.role-badge:hover::after {
  transform: translateX(300px) skewX(-15deg);
  opacity: 1;
  transition: 0.7s;
}

.overview-text {
  margin: 0;
  color: #5f6368;
  font-size: 12px;
  line-height: 1.5;
  text-align: justify;
}

.project-skills {
  margin-bottom: 1rem;
  padding: 12px;
  background: white;
  border-radius: 6px;
  border: 1px solid #e0e0e0;
}

.skills-header {
  font-size: 12px;
  font-weight: 600;
  color: #202124;
  margin-bottom: 10px;
  padding-bottom: 6px;
  border-bottom: 2px solid #e8f0fe;
}

.skills-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 8px;
}

.skill-item {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 6px;
  border-radius: 4px;
  transition: background 0.2s;
}

.skill-item:hover,
.skill-item.skill-active {
  background: #f0faf0;
}

.skill-item:hover .skill-check,
.skill-item.skill-active .skill-check {
  filter: brightness(0) saturate(100%) invert(55%) sepia(60%) saturate(400%) hue-rotate(95deg) brightness(95%);
}

.skill-item:hover span,
.skill-item.skill-active span {
  color: #34a853;
}

.skill-check {
  width: 16px;
  height: 16px;
  flex-shrink: 0;
  filter: brightness(0) saturate(100%);
}

.skill-item span {
  font-size: 11px;
  font-weight: 500;
  color: #3c4043;
  line-height: 1.3;
}

.project-tools { margin-top: 1rem; }

.project-tools strong {
  display: block;
  margin-bottom: 0.5rem;
  color: #333;
  font-size: 12px;
}

.tools-list {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
}

.tool-badge {
  background: white;
  color: #333;
  padding: 5px 10px;
  border-radius: 14px;
  font-size: 11px;
  font-weight: 500;
  font-family: 'Segoe UI', Tahoma, sans-serif;
  border: 1px solid #ddd;
  display: flex;
  align-items: center;
  gap: 5px;
  position: relative;
  overflow: hidden;
  cursor: default;
  transition: background 0.3s, color 0.3s, border-color 0.3s;
}

.tool-badge::before {
  content: '';
  position: absolute;
  background: rgba(255,255,255,0.95);
  width: 50px;
  height: 100%;
  left: 0;
  top: 0;
  opacity: 0;
  filter: blur(8px);
  transform: translateX(-100px) skewX(-15deg);
}

.tool-badge:hover::before {
  transform: translateX(200px) skewX(-15deg);
  opacity: 0.7;
  transition: 0.6s;
}

.tool-dot {
  width: 7px;
  height: 7px;
  border-radius: 50%;
  display: inline-block;
  flex-shrink: 0;
}

.project-repos {
  margin-top: 1rem;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.repo-card {
  background: white;
  border: 1px solid #d0d7de;
  border-radius: 6px;
  padding: 12px;
  text-decoration: none;
  color: inherit;
  transition: all 0.2s;
  width: 100%;
}

.repo-card[href="#"] {
  cursor: default;
  opacity: 0.85;
}

.repo-card[href="#"]:hover {
  border-color: #d0d7de;
  box-shadow: none;
}

.repo-card:hover {
  border-color: #0969da;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.repo-header {
  display: flex;
  align-items: center;
  gap: 6px;
  margin-bottom: 10px;
}

.repo-icon {
  width: 14px;
  height: 14px;
  filter: brightness(0) saturate(100%) opacity(1);
  flex-shrink: 0;
}

.repo-name {
  font-size: 12px;
  font-weight: 600;
  flex: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

a.repo-name {
  text-decoration: none;
  color: inherit;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: fit-content;
}

a.repo-name:hover {
  text-decoration: underline;
  color: #0969da;
}

.repo-badge {
  font-size: 10px;
  color: #3b3d41;
  border: 1px solid #c6c8cb;
  border-radius: 15px;
  padding: 0px 8px;
  font-weight: 500;
  flex-shrink: 0;
}

.repo-language {
  display: flex;
  align-items: center;
  gap: 5px;
  font-size: 11px;
  color: #57606a;
}

.language-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  display: inline-block;
  flex-shrink: 0;
}

.language-name { font-weight: 400; }

/* Sleeping Cat */
.repo-card.has-cat {
  position: relative;
  overflow: visible;
}

.sleeping-cat {
  position: absolute;
  top: -44px;
  right: 20px;
  z-index: 10;
  pointer-events: none;
  font-family: 'Sour Gummy', sans-serif;
}

.sleep-symbol {
  margin-left: -10px;
  margin-bottom: 8px;
  font-weight: 600;
  font-size: 16px;
  color: #666;
}

.sleep-symbol span {
  position: relative;
  display: inline-block;
  opacity: 1;
  transform: scale(1);
  animation: sleep 4s ease-in-out infinite;
}

.sleep-symbol span:nth-child(1) { animation-delay: 0s; }
.sleep-symbol span:nth-child(2) { animation-delay: 1s; margin-left: -4px; }
.sleep-symbol span:nth-child(3) { animation-delay: 2s; margin-left: -4px; }

.cat-svg {
  transform: scale(2);
  filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.15));
}

.repo-card.has-cat:hover .sleep-symbol {
  opacity: 0;
  transition: opacity 0.3s;
}

.repo-card.has-cat:hover .cat-svg #lefteyelid,
.repo-card.has-cat:hover .cat-svg #righteyelid {
  visibility: hidden;
}

.repo-card.has-cat:hover .cat-svg #eyesdown {
  visibility: visible;
}

.cat-svg #eyesdown { visibility: hidden; }

@keyframes sleep {
  0% { opacity: 1; transform: translateY(0) scale(1); }
  50% { opacity: 0.5; transform: translate(-2px, -16px) scale(1.2); }
  100% { opacity: 0; transform: translateY(-28px) scale(1.5); }
}

/* Lightbox */
.lightbox-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.95);
  z-index: 3000;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
}

.lightbox-content {
  max-width: 100%;
  max-height: 100%;
  position: relative;
}

.lightbox-content img {
  max-width: 100%;
  max-height: calc(100vh - 100px);
  object-fit: contain;
  border-radius: 6px;
}

.lightbox-close {
  position: absolute;
  top: 15px;
  right: 15px;
  background: rgba(255, 255, 255, 0.1);
  border: none;
  border-radius: 50%;
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 3001;
  transition: background 0.2s;
}

.lightbox-close:hover { background: rgba(241, 243, 244, 0.2); }

.lightbox-close img {
  width: 18px;
  height: 18px;
  filter: brightness(0) saturate(100%) invert(1);
}

.lightbox-nav {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(255, 255, 255, 0.1);
  border: none;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  cursor: pointer;
  z-index: 3001;
  transition: background 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 6px;
}

.lightbox-nav:hover { background: rgba(241, 243, 244, 0.2); }

.lightbox-nav img {
  width: 20px;
  height: 20px;
  filter: brightness(0) saturate(100%) invert(1);
}

.lightbox-nav.prev { left: 15px; }
.lightbox-nav.next { right: 15px; }

.lightbox-counter {
  position: absolute;
  bottom: -25px;
  left: 50%;
  transform: translateX(-50%);
  font-size: 12px;
  color: white;
  font-family: 'Segoe UI', Tahoma, sans-serif;
}

/* Footer */
.footer {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 8px 15px;
  text-align: center;
  z-index: 5;
  pointer-events: none;
}

.footer-text {
  font-size: 9px;
  color: rgba(0, 0, 0, 0.5);
  font-family: 'Segoe UI', Tahoma, sans-serif;
  pointer-events: auto;
  line-height: 1.4;
}

.footer-text a {
  color: #1a73e8;
  text-decoration: none;
  transition: color 0.2s;
}

.footer-text a:hover {
  color: #0d47a1;
  text-decoration: underline;
}

/* Tablet Styles */
@media (min-width: 768px) {
  .content { padding: 80px 20px 40px; }

  .logo {
    height: 220px;
    max-width: 220px;
    margin-top: -40px;
    margin-bottom: -50px;
  }

  .search-container {
    max-width: 620px;
    margin-bottom: 30px;
  }

  .search-bar { padding: 6px 10px; }
  .search-input { font-size: 15px; }
  .search-input::placeholder { font-size: 14px; }
  .ai-mode-btn { padding: 7px 11px 7px 7px; font-size: 11px; }
  .ai-text { display: inline; }
  .shortcuts { gap: 45px; }
  .shortcut-icon { width: 42px; height: 42px; }
  .shortcut-icon img { width: 22px; height: 22px; }
  .shortcut-label { font-size: 13px; }

  .header-nav { gap: 12px; padding: 12px 16px; }

  .nav-links {
    display: flex;
    align-items: center;
    gap: 15px;
  }

  .nav-link {
    color: #3c4043;
    text-decoration: none;
    font-size: 13px;
    font-family: arial, sans-serif;
    transition: opacity 0.2s;
    position: relative;
  }

  .nav-link::after {
    content: '';
    position: absolute;
    bottom: -2px;
    left: 0;
    width: 100%;
    height: 1px;
    background: #3c4043;
    transform: scaleX(0);
    transition: transform 0.2s ease;
  }

  .nav-link:hover::after { transform: scaleX(1); }

  .nav-icons { gap: 6px; }
  .nav-icon-btn { padding: 7px; width: 38px; height: 38px; }
  .lab-btn img { width: 16px; height: 16px; }
  .app-btn img { width: 24px; height: 24px; }
  .profile-btn { width: 30px; height: 30px; }
  .modal-content { max-width: 355px; }
  .modal-email { font-size: 13px; }
  .profile-name { font-size: 19px; }
  .manage-account-btn { padding: 8px 22px; font-size: 13px; }

  .folder { max-width: 45rem; }
  .tabs { margin: 0 1.5rem; width: calc(100% - 3rem); }
  .tab { margin-right: 3rem; font-size: 95%; }
  .tab span { font-size: 130%; min-width: 5.5rem; }
  .content__inner { padding: 1rem; }
  .page { padding: 1.2rem; min-height: 18rem; }
  .project-gallery { flex-direction: row; gap: 12px; }
  .gallery-main { width: 180px; height: 135px; }
  .gallery-more { width: 180px; height: 135px; }
  .more-text { font-size: 1.5rem; }
  .project-overview, .project-skills { padding: 14px; }
  .overview-text { font-size: 13px; }
  .skills-grid { grid-template-columns: repeat(2, 1fr); gap: 10px; }
  .skill-item span { font-size: 12px; }
  .project-repos { flex-direction: row; flex-wrap: wrap; gap: 12px; }
  .repo-card { flex: 1; min-width: calc(50% - 6px); max-width: calc(50% - 6px); padding: 14px; }
  .repo-name { font-size: 13px; }
  .repo-language { font-size: 12px; }
  .sleeping-cat { top: -48px; right: 28px; }
  .sleep-symbol { font-size: 17px; }
  .cat-svg { transform: scale(2.2); }

  .notification-popup { left: 20px; transform: none; width: 380px; }
  .slide-enter-from { transform: translateY(100px); }
  .slide-leave-to { transform: translateY(100px); }
  .footer-text { font-size: 10px; }
}

/* Desktop Styles */
@media (min-width: 1024px) {
  .content { padding: 80px 20px 40px; }

  .logo {
    height: 265px;
    max-width: 265px;
    margin-top: -50px;
    margin-bottom: -60px;
  }

  .search-container { max-width: 672px; }
  .search-bar { padding: 6.5px 10px; }
  .search-icon { margin-right: 12px; margin-left: -4px; padding: 8px; }
  .search-icon img { width: 22px; height: 22px; }
  .search-input { font-size: 16px; margin-left: -6px; margin-bottom: 2px; }
  .search-input::placeholder { font-size: 16px; }
  .search-actions { gap: 12px; }
  .action-btn { padding: 1px; }
  .action-btn img { width: 18px; height: 18px; }
  .ai-mode-btn { padding: 8px 12px 8px 8px; font-size: 12px; }
  .ai-icon { width: 16px; height: 16px; }
  .shortcuts { gap: 55px; margin-bottom: 25px; }
  .shortcut-item::before { width: 80px; height: 80px; }
  .shortcut-icon { width: 44px; height: 44px; }
  .shortcut-icon img { width: 23px; height: 23px; }
  .customize-btn { bottom: 16px; right: 16px; width: 32px; height: 32px; }
  .customize-btn img { width: 17px; height: 17px; }

  .header-nav { gap: 15px; padding: 15px 20px; }
  .nav-icon-btn { padding: 8px; width: 40px; height: 40px; }
  .lab-btn img { width: 17px; height: 17px; }
  .app-btn img { width: 25px; height: 25px; }
  .profile-btn { width: 32px; height: 32px; }
  .profile-btn::before { width: 32px; height: 32px; }
  .profile-btn:hover::before, .profile-btn.active::before { width: 40px; height: 40px; }

  .modal-overlay { padding: 60px 20px 20px; justify-content: flex-end; }
  .modal-content { max-width: 365px; }
  .modal-header { padding: 12px 20px; }
  .close-btn { padding: 8px; margin: -8px; right: 16px; }
  .close-btn img { width: 20px; height: 20px; }
  .modal-body { padding: 24px 20px 28px; }
  .profile-photo { width: 75px; height: 75px; margin-bottom: 16px; }
  .photo-edit-btn { width: 28px; height: 28px; }
  .photo-edit-btn img { width: 16px; height: 16px; }
  .profile-name { font-size: 20px; }
  .manage-account-btn { padding: 9px 24px; }

  .projects-modal-overlay { padding: 20px; }
  .folder { max-width: 50rem; }
  .tabs { padding: 2rem 0 0 0; margin: 0 2rem; width: calc(100% - 4rem); }
  .tab { margin-left: -35px; margin-right: 4rem; font-size: 100%; }
  .tab:first-of-type { margin-left: 30px; }
  .tab div { padding: 6px 0; }
  .tab span { padding: 6px 15px 6px; font-size: 140%; min-width: 6rem; }
  .tab:before, .tab:after { width: 30px; }
  .tab:before { right: -16px; }
  .tab:after { left: -16px; }
  .content__inner { padding: 1rem; }
  .page { padding: 1.5rem; min-height: 20rem; line-height: 160%; }
  .project-gallery { margin-bottom: 1.5rem; }
  .gallery-main { width: 200px; height: 150px; }
  .gallery-more { width: 200px; height: 150px; }
  .more-text { font-size: 1.6rem; }
  .project-overview { margin-bottom: 1.5rem; padding: 16px; }
  .role-badge { padding: 6px 12px; font-size: 13px; }
  .overview-text { font-size: 14px; }
  .project-skills { margin-bottom: 1.5rem; padding: 16px; }
  .skills-header { font-size: 14px; margin-bottom: 12px; }
  .skills-grid { gap: 12px; }
  .skill-item { padding: 8px; }
  .skill-check { width: 18px; height: 18px; }
  .skill-item span { font-size: 13px; }
  .project-tools strong { font-size: 14px; }
  .tools-list { gap: 8px; }
  .tool-badge { padding: 6px 12px; font-size: 12px; }
  .tool-dot { width: 8px; height: 8px; }
  .project-repos { margin-top: 1.5rem; }
  .repo-card { padding: 16px; }
  .repo-header { gap: 8px; margin-bottom: 12px; }
  .repo-icon { width: 16px; height: 16px; }
  .repo-name { font-size: 14px; }
  .repo-badge { font-size: 12px; }
  .repo-language { gap: 6px; font-size: 12px; }
  .language-dot { width: 12px; height: 12px; }
  .sleeping-cat { top: -54px; right: 33px; }
  .sleep-symbol { margin-left: -12px; margin-bottom: 10px; font-size: 19px; }
  .sleep-symbol span:nth-child(2) { margin-left: -5px; }
  .sleep-symbol span:nth-child(3) { margin-left: -5px; }
  .cat-svg { transform: scale(2.5); }

  @keyframes sleep {
    0% { opacity: 1; transform: translateY(0) scale(1); }
    50% { opacity: 0.5; transform: translate(-3px, -20px) scale(1.2); }
    100% { opacity: 0; transform: translateY(-35px) scale(1.5); }
  }

  .lightbox-overlay { padding: 20px; }
  .lightbox-content img { max-height: 90vh; }
  .lightbox-close { top: 20px; right: 20px; width: 40px; height: 40px; }
  .lightbox-close img { width: 20px; height: 20px; }
  .lightbox-nav { width: 48px; height: 48px; padding: 8px; }
  .lightbox-nav img { width: 24px; height: 24px; }
  .lightbox-nav.prev { left: 20px; }
  .lightbox-nav.next { right: 20px; }
  .lightbox-counter { bottom: -30px; font-size: 14px; }
  .notification-popup { max-width: 400px; padding: 16px 20px; }
  .notification-icon { width: 24px; height: 24px; }
  .notification-title { font-size: 14px; }
  .notification-message { font-size: 13px; }
  .notification-close img { width: 20px; height: 20px; }
  .footer { padding: 12px 20px; }
  .footer-text { font-size: 11px; }
}

/* Large Desktop */
@media (min-width: 1440px) {
  .shortcuts { gap: 65px; }
}
</style>