<template>
  <div class="calculator-page">
    <div class="tab-container">
      <button 
        :class="['tab-button', { active: activeTab === 'weight' }]"
        @click="activeTab = 'weight'"
      >
        무게 계산
      </button>
      <button 
        :class="['tab-button', { active: activeTab === 'school' }]"
        @click="activeTab = 'school'"
      >
        학교별 계산
      </button>
    </div>
    
    <PeopleCalculator v-if="activeTab === 'weight'" />
    <SchoolCalculator v-else />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import PeopleCalculator from '~/components/PeopleCalculator.vue'
import SchoolCalculator from '~/components/SchoolCalculator.vue'

const activeTab = ref('weight')
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
}

.tab-container {
  display: flex;
  gap: 1rem;
  margin-bottom: 2rem;
}

.tab-button {
  flex: 1;
  padding: 0.8rem;
  border: none;
  border-radius: 8px;
  background: #f5f5f5;
  color: #666;
  font-size: 1rem;
  cursor: pointer;

  &.active {
    background: #007AFF;
    color: white;
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