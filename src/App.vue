<template>
  <main class="app">
    <!-- 🌌 배경 -->
    <div class="sky-gradient"></div>
    <div class="stars"></div>
    <div class="cloud cloud-1" aria-hidden="true"></div>
    <div class="cloud cloud-2" aria-hidden="true"></div>

    <!-- 🌕 달 & 토끼 -->
    <section class="hero">
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

    <!-- 🏮 연등 켜면 밑에서 위로 떠오름 -->
    <div v-if="lanternOn" class="lantern-wrap">
      <div
          v-for="n in 8"
          :key="n"
          class="lantern"
          :style="lanternStyle(n)"
          aria-hidden="true"
      >
        <div class="lantern-body"></div>
        <div class="lantern-light"></div>
        <div class="lantern-tail"></div>
      </div>
    </div>

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

const lanternOn = ref(false)
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
  const delay = `${n * 1.2}s`
  const left = `${10 + n * 10}%`
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
   전체 배경
=========================== */
.app {
  min-height: 100vh;
  background: #0a0b18;
  color: #f8f8fb;
}

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
      radial-gradient(1px 1px at 10% 20%, #ffffff 90%, transparent),
      radial-gradient(1px 1px at 80% 40%, #fff6cc 90%, transparent),
      radial-gradient(1px 1px at 50% 70%, #ffffff 90%, transparent);
  background-size: 600px 600px;
  animation: twinkle 8s linear infinite;
  z-index: -2;
}
@keyframes twinkle {
  0%, 100% { opacity: 0.9; }
  50% { opacity: 0.6; }
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
.rabbit {
  position: absolute;
  width: 60%;
  left: 52%;
  top: 48%;
  transform: translate(-50%, -50%) rotate(-8deg);
  color: rgba(150, 100, 40, 0.4);
}

/* ===========================
   버튼 스타일 (금빛 테마)
=========================== */
.actions {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 3rem;
}

.btn {
  background: linear-gradient(90deg, #ffcc66, #ffb84d);
  color: #1e1400;
  border: none;
  border-radius: 999px;
  padding: 0.8rem 1.8rem;
  font-weight: 700;
  font-size: 1rem;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(255, 204, 102, 0.4);
  transition: all 0.25s ease;
}
.btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 16px rgba(255, 204, 102, 0.6);
  filter: brightness(1.05);
}
.btn.small {
  padding: 0.6rem 1.4rem;
  font-size: 0.9rem;
}
.btn.outline {
  background: transparent;
  color: #ffcc66;
  border: 2px solid #ffcc66;
  box-shadow: none;
}
.btn.outline:hover {
  background: #ffcc66;
  color: #1e1400;
  box-shadow: 0 6px 14px rgba(255, 204, 102, 0.4);
}

/* ===========================
   등불 (10초 고정 속도)
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
  animation: rise 10s linear infinite; /* ✅ 속도 10초로 변경됨 */
  opacity: 0;
}
@keyframes rise {
  0% { transform: translateY(100vh) scale(1); opacity: 0; }
  10% { opacity: 1; }
  100% { transform: translateY(-130vh) scale(1.2); opacity: 0; }
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
  font-size: 1.8rem;
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
  border-radius: 12px;
  border: 1px solid #ffcc6633;
  background: #12121f;
  color: #fff;
  transition: 0.2s;
}
.input:focus {
  outline: none;
  border-color: #ffcc66;
  box-shadow: 0 0 8px rgba(255, 204, 102, 0.3);
}
.input::placeholder { color: #aaa; opacity: 0.7; }

.wish-list {
  display: grid;
  gap: 0.5rem;
}
.wish-item {
  display: grid;
  grid-template-columns: auto 1fr auto;
  align-items: center;
  gap: 0.6rem;
  background: #101020;
  border-radius: 10px;
  padding: 0.8rem 1rem;
  border: 1px solid #ffffff22;
}
.wish-item .text { opacity: 0.95; }
.remove {
  background: transparent;
  border: none;
  color: #ffcc66;
  cursor: pointer;
  font-size: 1rem;
}

/* ===========================
   푸터
=========================== */
.footer {
  text-align: center;
  padding: 1rem;
  opacity: 0.7;
  font-size: 0.85rem;
}
</style>
