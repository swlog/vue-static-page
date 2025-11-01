# 🌕 한가위 보름달처럼 마음도 가득 찼으면 (Vue + TypeScript + Vite)

🔗 **GitHub Pages:** [https://swlog.github.io/vue-static-page/](https://swlog.github.io/vue-static-page/)  
💾 **GitHub 저장소:** [https://github.com/swlog/vue-static-page](https://github.com/swlog/vue-static-page)


> 🍂 풍성한 달빛 아래 따뜻한 덕담 한마디, 행복한 한가위 되세요! 🍂  
> 이 프로젝트는 **Vue 3 + TypeScript + Vite**로 구현된 한가위 테마 웹페이지입니다.  
> 달과 토끼, 반짝이는 별, 구름, 그리고 연등 애니메이션이 함께 어우러진 명절 분위기를 표현했습니다.  
>
> 
> 
 💫 주요 기능

🌕 1. 달 & 토끼 배경

radial-gradient로 부드럽게 표현된 달

SVG 토끼 실루엣으로 따뜻한 분위기 연출

반짝이는 별(@keyframes twinkle) + 흐르는 구름(@keyframes float)

🏮 2. 연등 애니메이션

버튼 클릭 시 lanternOn 상태 토글

등불 8개가 10초 주기로 아래→위로 부드럽게 떠오름

각 등불은 다른 위치(left%)와 크기(scale)로 자연스럽게 배치

@keyframes rise 애니메이션으로 부드러운 상승 효과

💬 3. 덕담 남기기 기능

사용자가 직접 덕담을 입력 가능

Enter 또는 “남기기” 버튼으로 즉시 추가

✕ 버튼 클릭 시 해당 덕담 삭제

덕담은 리스트 형태로 표시되어 따뜻한 분위기 연출

🎨 4. 금빛 버튼 스타일

linear-gradient(90deg, #ffcc66, #ffb84d) 그라데이션

Hover 시 살짝 떠오르며 (translateY(-3px)) 부드러운 그림자 강조

“덕담 남기기” 버튼은 outline 버전으로 구현

📱 5. 반응형 디자인

clamp()와 vw 단위를 사용하여 크기 자동 조정

모바일에서도 레이아웃이 무너지지 않도록 구성

#프롬포트

vue + ts + vite로 한가위에 잘 어울리는 웹페이지를 하나 만들고 싶은데, app.vue 파일 깔쌈하게 하나 만들어줄래?

화면 구성은 다음과 같아:
1. 달과 토끼가 있는 메인 배경 (별, 구름 애니메이션 포함)
2. "한가위 보름달처럼 마음도 가득 찼으면" 이라는 제목과
   "풍성한 추석 보내세요. 풍요와 안녕을 빌어요 🌕" 라는 부제
3. "연등 켜기 / 끄기" 버튼을 누르면 등불이 아래에서 위로 올라오는 애니메이션
4. "덕담 남기기" 버튼 클릭 시 스크롤 이동
5. 덕담 입력창과 “남기기” 버튼, 덕담 목록(삭제 기능 포함)
6. 반응형으로 작동하도록 만들고, 버튼은 금빛 그라데이션 스타일로 해줘.

