<script setup>
import { nextTick, onBeforeUnmount, onMounted, ref } from 'vue'
import AboutSection from './components/AboutSection.vue'
import SkillsSection from './components/SkillsSection.vue'
import ProjectsSection from './components/ProjectsSection.vue'
import ExperienceSection from './components/ExperienceSection.vue'

const navWrapRef = ref(null)
const navRef = ref(null)
const isNavFixed = ref(false)
const navHeight = ref(0)
const activeSection = ref('about')

const sectionIds = ['about', 'skills', 'experience', 'projects']

let navStartTop = 0

const measureNav = () => {
  if (!navWrapRef.value || !navRef.value) {
    return
  }

  navStartTop = navWrapRef.value.getBoundingClientRect().top + window.scrollY
  navHeight.value = navRef.value.offsetHeight
}

const onScroll = () => {
  isNavFixed.value = window.scrollY >= navStartTop - 12

  const marker = window.scrollY + navHeight.value + 40
  let currentSection = sectionIds[0]

  for (const sectionId of sectionIds) {
    const sectionEl = document.getElementById(sectionId)

    if (sectionEl && marker >= sectionEl.offsetTop) {
      currentSection = sectionId
    }
  }

  activeSection.value = currentSection
}

onMounted(async () => {
  await nextTick()
  measureNav()
  onScroll()

  window.addEventListener('scroll', onScroll, { passive: true })
  window.addEventListener('resize', measureNav)
})

onBeforeUnmount(() => {
  window.removeEventListener('scroll', onScroll)
  window.removeEventListener('resize', measureNav)
})
</script>

<template>
  <div class="page">
    <main>
      <section class="intro">
        <div class="container">
          <h1>안녕하세요, <br class="mobile-break">웹 개발자 최영찬입니다</h1>
          <p class="intro-subtitle">Java와 Spring Framework 기반의 환경에서 SI 프로젝트 수행 및 안정적인 SM 운영 경험을 보유하고 있습니다.</p>
          <div ref="navWrapRef" class="intro-nav-wrap" :style="isNavFixed ? { height: `${navHeight}px` } : null">
            <nav ref="navRef" class="intro-nav" :class="{ 'is-fixed': isNavFixed }" aria-label="포트폴리오 섹션 이동">
              <span class="traffic-lights" aria-hidden="true">
                <span class="light red"></span>
                <span class="light yellow"></span>
                <span class="light green"></span>
              </span>
              <a href="#about" :class="{ 'is-active': isNavFixed && activeSection === 'about' }">소개</a>
              <a href="#skills" :class="{ 'is-active': isNavFixed && activeSection === 'skills' }">기술</a>
              <a href="#experience" :class="{ 'is-active': isNavFixed && activeSection === 'experience' }">경력</a>
              <a href="#projects" :class="{ 'is-active': isNavFixed && activeSection === 'projects' }">프로젝트</a>
            </nav>
          </div>
        </div>
      </section>

      <AboutSection />
      <SkillsSection />
      <ExperienceSection />
      <ProjectsSection />
    </main>
  </div>
</template>
