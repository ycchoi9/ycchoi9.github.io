<script setup>
import { onBeforeUnmount, onMounted, ref, watch } from 'vue'

const projects = [
  {
    title: '전자정부프레임워크 기반 웹사이트 운영 및 유지보수',
    period: '26.01.01 - 현재',
    summary: [
      '한국어촌어항공단 웹사이트 운영 및 유지보수',
      'SMS 대량 발송 성능 개선',
      '웹 접근성 개선 경험',
    ],
    detailSummary: [
      {
        title: '로직 최적화 경험',
        description:
          '기존 단건 발송 방식(23명 발송 시 23회 호출)에서 최대 20명 단위 묶음 발송 방식으로 전환해 API 호출 횟수를 약 90% 줄였습니다. (23회 → 2회)',
      },
      {
        title: '웹 접근성 개선',
        description:
          '웹 접근성 표준을 준수하도록 사이트를 개선하여 다양한 사용자가 서비스를 이용할 수 있도록 개선했습니다.',
      },
    ],
    stack: 'JSP · Spring · PostgreSQL',
    details:
      '한국어촌어항공단 웹 사이트와 관리자 페이지를 운영 및 유지보수를 담당했습니다. ',
  },
  {
    title: 'Nuxt.js 기반 웹사이트 개발',
    period: '25.11.01 - 25.12.31',
    summary: [
      '한국어촌어항공단 연근해어장 업무시스템 개발',
      '나라장터 Open API 데이터 연동',
      '엑셀 데이터 업로드 기능 개발',
      '2명의 개발자와 협업',
    ],
    detailSummary: [
      {
        title: '업무 시스템 구축 경험',
        description: '실무 요구사항 기반 기능을 구현하며 업무 시스템을 개발했습니다.',
      },
      {
        title: '외부 데이터 연동 경험',
        description: '나라장터 Open API 연동을 하여 batch를 통해 주기적으로 데이터를 업데이트하며 데이터 처리 자동화를 지원했습니다.',
      },
      {
        title: '엑셀 데이터 업로드 기능 개발',
        description: '엑셀의 파일을 업로드하면 컬럼을 매칭시켜 데이터베이스에 저장하는 기능을 개발하였습니다.',
      },
    ],
    stack: 'Vue 3 · Spring · PostgreSQL',
    details:
      'API 연동, 파일 및 엑셀 업로드 기능을 구현하였으며 로직을 구현했습니다. 2명의 개발자와 Git으로 협업하여 진행하였습니다.',
  },
  {
    title: 'Vue3 기반 홈페이지 개편 및 CMS 운영',
    period: '25.04.14 - 25.06.30',
    summary: [
      '차세대정보보안인증원(NISC) 홈페이지 개편',
      '통합 콘텐츠 관리 시스템(CMS) 개발',
      'Vue 기반 게시판 기능 구현',
    ],
    detailSummary: [
      {
        title: 'CMS 개발 경험',
        description: '계정별 권한을 부여하고, 권한별 접근 가능한 페이지를 나누었습니다.',
      },
      {
        title: '게시판 기능 구현 및 CMS 연동',
        description: 'Vue 기반 게시판 기능을 구현해 CRUD 및 페이지네이션 기능을 구현했습니다.',
      },
    ],
    stack: 'Vue 3 · Spring · PostgreSQL',
    details:
      'vue.js이용하여 처음으로 유지보수가 아닌 개발로 시작한 프로젝트입니다. \n emit과 props를 활용하여 컴포넌트 간 데이터 흐름을 관리하며 게시판 기능을 구현했습니다. CMS 개발을 통해 관리자 관점의 운영 편의성과 유지보수성을 강화했습니다.',
  },
  {
    title: '전자정부프레임워크 기반 웹사이트 운영 및 유지보수',
    period: '25.01.01 - 25.12.31',
    summary: [
      '한국수목원정원관리원(한수정) 정보시스템 통합 유지관리',
      '예약 및 결제 시스템 운영',
      '통합 콘텐츠 관리 시스템(CMS) 운영',
    ],
    detailSummary: [
      {
        title: '웹취약점 개선 경험',
        description: '문제 확인\n- SCI 본인인증 시 클라이언트에서 SCI로 인증 요청을 보낼 때 전달 정보 탈취·조작을 통한 악용 가능성을 확인했습니다.\n개선 사항\n- 본인인증 후 SCI에 요청을 보내기 전 WAS에서 유효성검사를 거친 후 부적합할경우 리다이렉트하도록 변경',
      },
    ],
    stack: 'Vue 2 · Spring · PostgreSQL',
    details:
      '서비스 운영 관점에서 장애 대응과 기능 개선을 동시에 수행했습니다. 예약/결제 흐름의 안정성을 높이고 관리 시스템의 운영 효율을 개선했습니다.',
  },
]

const selectedProject = ref(null)
const isProjectLoading = ref(false)
const SKELETON_DELAY_MS = 180

let skeletonDelayTimer = null

const loadProjectDetail = async (project) => project

const openProjectModal = async (project) => {
  selectedProject.value = project
  isProjectLoading.value = false

  if (skeletonDelayTimer) {
    clearTimeout(skeletonDelayTimer)
  }

  skeletonDelayTimer = setTimeout(() => {
    isProjectLoading.value = true
  }, SKELETON_DELAY_MS)

  try {
    await loadProjectDetail(project)
  } finally {
    if (skeletonDelayTimer) {
      clearTimeout(skeletonDelayTimer)
      skeletonDelayTimer = null
    }

    isProjectLoading.value = false
  }
}

const closeProjectModal = () => {
  if (skeletonDelayTimer) {
    clearTimeout(skeletonDelayTimer)
    skeletonDelayTimer = null
  }

  isProjectLoading.value = false
  selectedProject.value = null
}

const onKeydown = (event) => {
  if (event.key === 'Escape' && selectedProject.value) {
    closeProjectModal()
  }
}

const originalBodyOverflow = typeof document !== 'undefined' ? document.body.style.overflow : ''

watch(selectedProject, (value) => {
  if (typeof document === 'undefined') {
    return
  }

  document.body.style.overflow = value ? 'hidden' : originalBodyOverflow
})

onMounted(() => {
  window.addEventListener('keydown', onKeydown)
})

onBeforeUnmount(() => {
  window.removeEventListener('keydown', onKeydown)

  if (skeletonDelayTimer) {
    clearTimeout(skeletonDelayTimer)
    skeletonDelayTimer = null
  }

  if (typeof document !== 'undefined') {
    document.body.style.overflow = originalBodyOverflow
  }
})
</script>

<template>
  <section id="projects" class="section">
    <div class="container">
      <h2>프로젝트</h2>
      <div class="cards">
        <article
          v-for="project in projects"
          :key="`${project.title}-${project.period}`"
          class="card project-card"
          role="button"
          tabindex="0"
          @click="openProjectModal(project)"
          @keyup.enter="openProjectModal(project)"
        >
          <h3>{{ project.title }}</h3>

          <small>{{ project.period }}</small>
          <hr>
          <ul class="summary-list">
            <li v-for="(item, index) in project.summary" :key="`${project.title}-${index}`" class="summary-item">
              {{ item }}
            </li>
          </ul>
          <small class="project-stack">{{ project.stack }}</small>
        </article>
      </div>

      <div v-if="selectedProject" class="modal-overlay" @click.self="closeProjectModal">
        <div class="project-modal" role="dialog" aria-modal="true" aria-label="프로젝트 상세 정보">
          <button class="modal-close" type="button" aria-label="모달 닫기" @click="closeProjectModal">×</button>
          <div v-if="isProjectLoading" class="modal-skeleton" aria-hidden="true">
            <div class="skeleton skeleton-title"></div>
            <div class="skeleton skeleton-label"></div>
            <div class="skeleton skeleton-line"></div>
            <div class="skeleton skeleton-label"></div>
            <div class="skeleton skeleton-block"></div>
            <div class="skeleton skeleton-label"></div>
            <div class="skeleton skeleton-line short"></div>
            <hr>
            <div class="skeleton skeleton-label"></div>
            <div class="skeleton skeleton-item"></div>
            <div class="skeleton skeleton-item"></div>
          </div>

          <template v-else>
            <h3>{{ selectedProject.title }}</h3>
            <small>기간</small>
            <p class="modal-period">{{ selectedProject.period }}</p>
            <small>프로젝트 설명</small>
            <p class="modal-detail">{{ selectedProject.details }}</p>
            <small>기술 스택</small>
            <p class="modal-stack">{{ selectedProject.stack }} · Tomcat</p>
            <hr>
            <h4>상세 내용</h4>

            <ul class="summary-list">
              <li v-for="(item, index) in selectedProject.detailSummary" :key="`detail-${index}`" class="summary-item">
                <p class="summary-title">{{ item.title }}</p>
                <p class="summary-description">{{ item.description }}</p>
              </li>
            </ul>
          </template>
          
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.project-card {
  display: flex;
  flex-direction: column;
  height: 100%;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.project-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 24px rgba(17, 24, 39, 0.08);
}

.project-card:focus-visible {
  outline: 2px solid var(--primary-blue);
  outline-offset: 2px;
}

.summary-list {
  margin: 0;
  padding-left: 1rem;
}

.summary-item {
  margin-bottom: 0.7rem;
}

.summary-item:last-child {
  margin-bottom: 0;
}

.project-stack {
  display: block;
  margin-top: auto;
  padding-top: 0.75rem;
}

.summary-title {
  margin: 0;
  font-weight: 600;
}

.summary-description {
  margin: 0.2rem 0 0;
  color: #4b5563;
  white-space: pre-line;
}

.modal-overlay {
  position: fixed;
  inset: 0;
  display: grid;
  place-items: center;
  padding: 1rem;
  background: rgba(17, 24, 39, 0.5);
  z-index: 1100;
}

.project-modal {
  position: relative;
  width: min(720px, 100%);
  max-height: 85vh;
  overflow-y: auto;
  background: #ffffff;
  border-radius: 16px;
  padding: 1.25rem;
}

.modal-close {
  position: absolute;
  top: 0.8rem;
  right: 0.8rem;
  width: 34px;
  height: 34px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #d1d5db;
  background: #ffffff;
  border-radius: 999px;
  padding: 0;
  font-weight: 600;
  font-size: 1.2rem;
  line-height: 1;
  cursor: pointer;
}

.modal-close:hover {
  background: var(--primary-blue-weak);
  border-color: var(--primary-blue-border);
  color: var(--primary-blue);
}

.modal-period {
  color: #6b7280;
  margin: 0.35rem 0 0.8rem;
}

.modal-detail {
  margin: 0.25rem 0 1rem;
}

.modal-stack {
  margin: 1rem 0 0;
  font-weight: 600;
}

.modal-skeleton {
  margin-top: 0.25rem;
}

.skeleton {
  border-radius: 10px;
  background: linear-gradient(90deg, #e5e7eb 25%, #f3f4f6 37%, #e5e7eb 63%);
  background-size: 400% 100%;
  animation: skeleton-loading 1.2s ease-in-out infinite;
}

.skeleton-title {
  width: 68%;
  height: 28px;
  margin: 0.2rem 0 1rem;
}

.skeleton-label {
  width: 72px;
  height: 13px;
  margin: 0.7rem 0 0.45rem;
}

.skeleton-line {
  width: 100%;
  height: 16px;
}

.skeleton-line.short {
  width: 45%;
}

.skeleton-block {
  width: 100%;
  height: 64px;
}

.skeleton-item {
  width: 100%;
  height: 54px;
  margin-top: 0.65rem;
}

@keyframes skeleton-loading {
  0% {
    background-position: 100% 50%;
  }

  100% {
    background-position: 0 50%;
  }
}

@media (max-width: 480px) {
  .summary-list {
    padding-left: 0.9rem;
  }

  .summary-item {
    margin-bottom: 0.55rem;
  }

  .project-stack {
    padding-top: 0.6rem;
    font-size: 0.82rem;
  }

  .modal-overlay {
    padding: 0.55rem;
  }

  .project-modal {
    width: 100%;
    max-height: 90vh;
    border-radius: 12px;
    padding: 1rem;
  }

  .modal-close {
    top: 0.55rem;
    right: 0.55rem;
    width: 30px;
    height: 30px;
    font-size: 1rem;
  }

  .summary-description {
    font-size: 0.93rem;
  }
}
</style>
