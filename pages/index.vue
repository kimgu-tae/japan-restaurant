<template>
  <div class="calculator-page">
    <PeopleCalculator />
  </div>
</template>

<script setup>
import PeopleCalculator from '~/components/PeopleCalculator.vue'
</script>

<style lang="scss" scoped>
.calculator-page {
  min-height: 100vh;
  width: 100%;
  max-width: 500px;
  margin: 0 auto;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  background: #ffffff;
  
  // 모바일 최적화
  @media (max-width: 768px) {
    padding: 0.5rem;
  }
}

// 아이폰 하단 safe area 대응
@supports (padding: max(0px)) {
  .calculator-page {
    padding-bottom: max(1rem, env(safe-area-inset-bottom));
  }
}

// 스크롤 방지
html, body {
  overflow: hidden;
  position: fixed;
  width: 100%;
  height: 100%;
}
</style>