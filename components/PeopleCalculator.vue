<template>
  <div class="calculator">
    <div class="input-group">
      <label>
        <span class="label-text">무게</span>
        <div class="input-wrapper">
          <input 
            type="number" 
            v-model="groups" 
            min="1" 
            placeholder="g을 입력"
            @input="calculate"
            inputmode="numeric"
          >
          <span class="unit">g</span>
        </div>
      </label>
    </div>

    <div class="input-group">
      <label>
        <span class="label-text">총인원</span>
        <div class="input-wrapper">
          <input 
            type="number" 
            v-model="totalPeople" 
            min="1" 
            placeholder="총인원을 입력"
            @input="calculate"
            inputmode="numeric"
          >
          <span class="unit">명</span>
        </div>
      </label>
    </div>

    <div class="result" v-if="showResult">
      <div class="result-item">
        <span class="result-label">1명당</span>
        <span class="result-value">{{ peoplePerGroup }}g</span>
      </div>
    </div>

    <div class="multiply-section" v-if="showResult">
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
          <span class="result-label">고등학교 필요량</span>
          <span class="result-value">{{ highSchoolAmount }}g</span>
        </div>
        <div class="result-item">
          <span class="result-label">중학교 필요량</span>
          <span class="result-value">{{ middleSchoolAmount }}g</span>
        </div>
      </div>
    </div>

    <div class="reset-button-container">
      <button class="reset-button" @click="reset">초기화</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const totalPeople = ref('')
const groups = ref('')
const highSchool = ref('')

const showResult = computed(() => {
  return totalPeople.value && groups.value && 
         Number(totalPeople.value) > 0 && 
         Number(groups.value) > 0
})

const peoplePerGroup = computed(() => {
  if (!showResult.value) return 0
  return (Number(groups.value) / Number(totalPeople.value)).toFixed(1)
})

const middleSchool = computed(() => {
  if (!highSchool.value || !totalPeople.value) return ''
  const middle = Number(totalPeople.value) - Number(highSchool.value)
  return middle >= 0 ? middle : 0
})

const highSchoolAmount = computed(() => {
  if (!highSchool.value || !peoplePerGroup.value) return 0
  return (Number(highSchool.value) * Number(peoplePerGroup.value)).toFixed(1)
})

const middleSchoolAmount = computed(() => {
  if (!middleSchool.value || !peoplePerGroup.value) return 0
  return (Number(middleSchool.value) * Number(peoplePerGroup.value)).toFixed(1)
})

const calculate = () => {
  if (Number(totalPeople.value) < 0) totalPeople.value = '1'
  if (Number(groups.value) < 0) groups.value = '1'
}

const reset = () => {
  totalPeople.value = ''
  groups.value = ''
  highSchool.value = ''
}
</script>

<style lang="scss" scoped>
.calculator {
  width: 100%;
  max-width: 500px;
  margin: 0 auto;
  padding: 1.5rem;
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

.reset-button-container {
  margin-top: 2rem;
  text-align: center;
}

.reset-button {
  padding: 0.8rem 2rem;
  font-size: 1rem;
  color: #fff;
  background-color: #299d8e;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.2s;

  &:hover {
    background-color: #299d8e;
  }
}

// 모바일 최적화
@media (max-width: 768px) {
  .calculator {
    padding: 1rem;
  }
}

// 키보드가 올라왔을 때 최적화
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