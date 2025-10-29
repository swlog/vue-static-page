<template>
  <main class="app">
    <!-- 배경 -->
    <div class="sky-gradient"></div>
    <div class="stars"></div>
    <div class="cloud cloud-1" aria-hidden="true"></div>
    <div class="cloud cloud-2" aria-hidden="true"></div>

    <!-- 🌕 달 & 토끼 -->
    <section class="hero" role="img" aria-label="보름달과 토끼 실루엣">
      <div class="moon">
        <svg class="rabbit" viewBox="0 0 120 120" aria-hidden="true">
          <path
              d="M78 48c6-7 4-13-3-22-3-4-7-8-13-9-7 1-13 5-13 9-1 7 2 13 9 18-7 4-14 10-14 19s7 16 18 17c9 0 18-4 22-12 4-8 2-14-6-20z"
              fill="currentColor"
          />
          <circle cx="50" cy="70" r="6" fill="#ffecc9" opacity="0.5" />
        </svg>
      </div>

      <h1 class="title">
        한가위 보름달처럼<br />
        마음도 가득 찼으면
      </h1>
      <p class="subtitle">풍성한 추석 보내세요. 풍요와 안녕을 빌어요 🌕</p>

      <div class="actions">
        <button class="btn" @click="toggleLanterns">
          <span v-if="lanternOn">연등 끄기</span>
          <span v-else>연등 켜기</span>
        </button>
        <button class="btn outline" @click="scrollToWishes">덕담 남기기</button>
      </div>
    </section>

    <!-- 🏮 떠다니는 등불 -->
    <transition-group name="lantern" tag="div" class="lantern-wrap" v-if="lanternOn">
      <div
          v-for="n in 6"
          :key="n"
          class="lantern"
          :style="lanternStyle(n)"
          aria-hidden="true"
      >
        <div class="lantern-body"></div>
        <div class="lantern-light"></div>
        <div class="lantern-tail"></div>
      </div>
    </transition-group>

    <!-- ✨ 덕담 섹션 -->
    <section class="wishes" ref="wishesRef">
      <h2>오늘의 덕담</h2>
      <div class="wish-form">
        <input
            v-model="draft"
            class="input"
            placeholder="따뜻한 한마디를 남겨보세요 💬"
            @keyup.enter="addWish"
        />
        <button class="btn small" @click="addWish">남기기</button>
      </div>

      <div class="wish-list">
        <div class="wish-item" v-for="(w, i) in wishes" :key="w.id">
          <span class="bullet">🕯️</span>
          <span class="text">{{ w.text }}</span>
          <button class="remove" @click="removeWish(i)">✕</button>
        </div>
      </div>
    </section>

    <footer class="footer">
      © 2025 한가위 | Made with Vue + TypeScript + Vite ✨
    </footer>
  </main>
</template>

<script setup lang="ts">
import { ref } from "vue"

type Wish = { id: number; text: string }

const lanternOn = ref(true)
const wishes = ref<Wish[]>([
  { id: 1, text: "보름달처럼 넉넉한 마음 가득하시길!" },
  { id: 2, text: "멀리 있어도 마음만은 한가위에 함께해요 🌕" }
])
const draft = ref("")
const wishesRef = ref<HTMLElement | null>(null)

function toggleLanterns() {
  lanternOn.value = !lanternOn.value
}

function lanternStyle(n: number) {
  const delay = `${n * 0.6}s`
  const left = `${10 + n * 12}%`
  const scale = 0.8 + (n % 3) * 0.1
  return {
    left,
    animationDelay: delay,
    transform: `scale(${scale})`
  }
}

function scrollToWishes() {
  wishesRef.value?.scrollIntoView({ behavior: "smooth", block: "start" })
}

function addWish() {
  if (!draft.value) return
  wishes.value.unshift({ id: Date.now(), text: draft.value })
  draft.value = ""
}

function removeWish(index: number) {
  wishes.value.splice(index, 1)
}
</script>

<style scoped>
/* ===========================
    전체 레이아웃 & 배경
=========================== */
.app {
  min-height: 100vh;
  color: #f8f8fb;
  background: #0a0b18;
  display: grid;
  grid-template-rows: auto auto 1fr auto;
}

/* 하늘 그라데이션 + 별 */
.sky-gradient {
  position: fixed;
  inset: 0;
  background: radial-gradient(1200px 600px at 50% -10%, #1f235a, #0a0b18 60%);
  z-index: -3;
}

.stars {
  position: fixed;
  inset: 0;
  background:
      radial-gradient(1px 1px at 10% 20%, #ffffff 90%, transparent) repeat,
      radial-gradient(1px 1px at 80% 40%, #fff6cc 90%, transparent) repeat,
      radial-gradient(1px 1px at 50% 70%, #ffffff 90%, transparent) repeat;
  background-size: 600px 600px;
  animation: twinkle 8s linear infinite;
  z-index: -2;
}

@keyframes twinkle {
  0%, 100% { opacity: 0.9; }
  50% { opacity: 0.6; }
}

/* 구름 */
.cloud {
  position: absolute;
  top: 20%;
  width: 240px;
  height: 70px;
  background: radial-gradient(closest-side, #fff, #fff2);
  border-radius: 50px;
  filter: blur(4px);
  animation: float 80s linear infinite;
  z-index: -1;
}
.cloud-1 { left: -20%; animation-delay: 0s; }
.cloud-2 { top: 40%; left: -30%; animation-delay: 20s; }

@keyframes float {
  from { transform: translateX(0); }
  to { transform: translateX(120vw); }
}

/* ===========================
    달 & 토끼
=========================== */
.moon {
  width: clamp(180px, 28vw, 320px);
  height: clamp(180px, 28vw, 320px);
  margin: auto;
  border-radius: 50%;
  background: radial-gradient(circle at 35% 35%, #fff5d6 0%, #ffe58a 50%, #ffd27d 70%, #fbc266 80%);
  box-shadow: 0 0 40px rgba(255, 207, 100, 0.6);
  position: relative;
}
.moon::after {
  content: "";
  position: absolute;
  inset: 0;
  border-radius: 50%;
  background: radial-gradient(circle at 25% 30%, #e9b86a 10%, transparent 20%);
  mix-blend-mode: multiply;
}
.rabbit {
  position: absolute;
  width: 60%;
  left: 52%;
  top: 48%;
  transform: translate(-50%, -50%) rotate(-8deg);
  color: rgba(150, 100, 40, 0.4);
}

/* ===========================
    타이틀 & 버튼
=========================== */
.title {
  text-align: center;
  font-weight: 800;
  font-size: clamp(1.8rem, 4vw, 2.8rem);
  line-height: 1.3;
  text-shadow: 0 2px 10px rgba(255, 220, 120, 0.3);
  margin-top: 2rem;
}
.subtitle {
  text-align: center;
  opacity: 0.85;
  margin-bottom: 1.5rem;
}

.actions {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 3rem;
}
.btn {
  background: #ffcc66;
  color: #231a02;
  border: none;
  border-radius: 999px;
  padding: 0.7rem 1.6rem;
  font-weight: 700;
  cursor: pointer;
  transition: 0.2s;
}
.btn:hover { transform: translateY(-2px); filter: brightness(1.05); }
.btn.outline {
  background: transparent;
  color: #ffcc66;
  border: 2px solid #ffcc66;
}
.btn.small { padding: 0.5rem 1rem; font-size: 0.9rem; }

/* ===========================
    등불 애니메이션
=========================== */
.lantern-wrap {
  position: relative;
  height: 0;
}
.lantern {
  position: absolute;
  bottom: 0;
  width: 60px;
  height: 90px;
  animation: rise 14s linear infinite;
}
@keyframes rise {
  from { transform: translateY(100vh) scale(1); opacity: 0.8; }
  to { transform: translateY(-120vh) scale(1.2); opacity: 0; }
}
.lantern-body {
  width: 100%;
  height: 70%;
  background: linear-gradient(to bottom, #ff7979, #e74c3c);
  border-radius: 20px;
}
.lantern-light {
  position: absolute;
  bottom: 10%;
  left: 50%;
  width: 10px;
  height: 10px;
  background: #ffd180;
  border-radius: 50%;
  transform: translateX(-50%);
}
.lantern-tail {
  position: absolute;
  bottom: -24px;
  left: 50%;
  width: 2px;
  height: 20px;
  background: #ffb54d;
  transform: translateX(-50%);
}

/* ===========================
    덕담 섹션
=========================== */
.wishes {
  padding: 3rem 2rem;
  max-width: 860px;
  margin: auto;
  text-align: center;
}
.wishes h2 {
  color: #ffcc66;
  font-size: 1.6rem;
  font-weight: 800;
  margin-bottom: 1rem;
}
.wish-form {
  display: grid;
  grid-template-columns: 1fr auto;
  gap: 0.6rem;
  margin-bottom: 1.5rem;
}
.input {
  padding: 0.8rem 1rem;
  border-radius: 10px;
  border: 1px solid #ffcc6633;
  background: #12121f;
  color: #fff;
}
.input::placeholder {
  color: #aaa;
  opacity: 0.7;
}
.wish-list {
  display: grid;
  gap: 0.5rem;
}
.wish-item {
  display: grid;
  grid-template-columns: auto 1fr auto;
  align-items: center;
  gap: 0.5rem;
  background: #101020;
  border-radius: 8px;
  padding: 0.7rem 1rem;
  border: 1px solid #ffffff22;
}
.wish-item .text { opacity: 0.95; }
.remove {
  background: transparent;
  border: none;
  color: #ffcc66;
  cursor: pointer;
}

/* 푸터 */
.footer {
  text-align: center;
  padding: 1rem;
  opacity: 0.7;
  font-size: 0.85rem;
}
</style>
