<template>
  <div class="calculator">
    <div class="input-group">
      <label>
        <span class="label-text">총인원</span>
        <div class="input-wrapper">
          <input 
            type="number" 
            v-model="totalPeople" 
            min="1" 
            placeholder="총인원을 입력"
            inputmode="numeric"
          >
          <span class="unit">명</span>
        </div>
      </label>
    </div>

    <div class="input-group">
      <label>
        <span class="label-text">봉지 수</span>
        <div class="input-wrapper">
          <input 
            type="number" 
            v-model="bags" 
            min="1" 
            placeholder="봉지 수를 입력"
            inputmode="numeric"
          >
          <span class="unit">개</span>
        </div>
      </label>
    </div>

    <div class="result" v-if="showDivisionResult">
      <div class="result-item">
        <span class="result-label">나누기 값</span>
        <span class="result-value">{{ divisionValue.toFixed(1) }}</span>
      </div>
    </div>

    <div class="multiply-section" v-if="showDivisionResult">
      <h3 class="section-title">학교별 계산</h3>
      <div class="input-group">
        <label>
          <span class="label-text">고등학교 인원</span>
          <div class="input-wrapper">
            <input 
              type="number" 
              v-model="highSchool" 
              min="0" 
              :max="totalPeople"
              placeholder="고등학교 인원 입력"
              inputmode="numeric"
            >
            <span class="unit">명</span>
          </div>
        </label>
      </div>

      <div class="input-group">
        <label>
          <span class="label-text">중학교 인원</span>
          <div class="input-wrapper">
            <input 
              type="number" 
              v-model="middleSchool" 
              disabled
              inputmode="numeric"
            >
            <span class="unit">명</span>
          </div>
        </label>
      </div>

      <div class="result">
        <div class="result-item">
          <span class="result-label">고등학교 배분</span>
          <span class="result-value">{{ highSchoolDivision.toFixed(1) }}</span>
        </div>
        <div class="result-item">
          <span class="result-label">중학교 배분</span>
          <span class="result-value">{{ middleSchoolDivision.toFixed(1) }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const totalPeople = ref('')
const bags = ref('')
const highSchool = ref('')

const showDivisionResult = computed(() => {
  return totalPeople.value && bags.value && 
         Number(totalPeople.value) > 0 && 
         Number(bags.value) > 0
})

const divisionValue = computed(() => {
  if (!showDivisionResult.value) return 0
  return Number(totalPeople.value) / Number(bags.value)
})

const middleSchool = computed(() => {
  if (!highSchool.value || !totalPeople.value) return ''
  const middle = Number(totalPeople.value) - Number(highSchool.value)
  return middle >= 0 ? middle : 0
})

const highSchoolDivision = computed(() => {
  if (!highSchool.value || !divisionValue.value) return 0
  return Number(highSchool.value) / divisionValue.value
})

const middleSchoolDivision = computed(() => {
  if (!middleSchool.value || !divisionValue.value) return 0
  return Number(middleSchool.value) / divisionValue.value
})
</script>

<style lang="scss" scoped>
.calculator {
  width: 100%;
  max-width: 500px;
  margin: 0 auto;
  padding: 1rem;
  background: #ffffff;
}

.input-group {
  margin-bottom: 1.5rem;

  label {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .label-text {
    font-size: 0.9rem;
    color: #666;
  }

  .input-wrapper {
    position: relative;
    display: flex;
    align-items: center;
  }

  input {
    width: 100%;
    padding: 1rem;
    font-size: 1.2rem;
    border: none;
    border-bottom: 2px solid #ddd;
    background: transparent;
    text-align: right;
    padding-right: 3rem;

    &:focus {
      outline: none;
      border-bottom-color: #007AFF;
    }

    &:disabled {
      background: #f5f5f5;
      color: #666;
    }

    &::placeholder {
      color: #999;
    }
  }

  .unit {
    position: absolute;
    right: 1rem;
    color: #666;
    font-size: 1rem;
  }
}

.result {
  background: #007AFF;
  padding: 1.5rem;
  border-radius: 12px;
  margin: 2rem 0;

  .result-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    
    &:not(:last-child) {
      margin-bottom: 1rem;
      padding-bottom: 1rem;
      border-bottom: 1px solid rgba(255, 255, 255, 0.2);
    }
  }

  .result-label {
    color: rgba(255, 255, 255, 0.8);
    font-size: 1rem;
  }

  .result-value {
    color: white;
    font-size: 1.5rem;
    font-weight: 600;
  }
}

.multiply-section {
  margin-top: 2rem;
  padding-top: 2rem;
  border-top: 1px solid #eee;

  .section-title {
    font-size: 1.1rem;
    color: #333;
    margin-bottom: 1.5rem;
    text-align: center;
  }
}

// 모바일 최적화
@media (max-height: 500px) {
  .calculator {
    padding: 0.5rem;
  }
  
  .input-group {
    margin-bottom: 1rem;
  }
  
  .result {
    margin: 1rem 0;
    padding: 1rem;
  }
}
</style>