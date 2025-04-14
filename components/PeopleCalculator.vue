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
            @input="calculate"
            inputmode="numeric"
          >
          <span class="unit">명</span>
        </div>
      </label>
    </div>

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

    <div class="result" v-if="showResult">
      <div class="result-item">
        <span class="result-label">1명당</span>
        <span class="result-value">{{ peoplePerGroup }}g</span>
      </div>
    </div>

    <div class="multiply-section" v-if="showResult">
      <h3 class="section-title">그람 계산</h3>
      <div class="input-group">
        <label>
          <span class="label-text">인원수</span>
          <div class="input-wrapper">
            <input 
              type="number" 
              v-model="multiplyNumber" 
              min="1" 
              placeholder="인원수 입력"
              inputmode="numeric"
            >
            <span class="unit">명</span>
          </div>
        </label>
      </div>

      <div class="result">
        <div class="result-item">
          <span class="result-label">필요한 양</span>
          <span class="result-value">{{ totalAmount }}g</span>
        </div>
      </div>
    </div>
  </div>
</template>

// ... existing code ...

<script setup>
import { ref, computed } from 'vue'

const totalPeople = ref('')
const groups = ref('')
const multiplyNumber = ref('')

const showResult = computed(() => {
  return totalPeople.value && groups.value && 
         Number(totalPeople.value) > 0 && 
         Number(groups.value) > 0
})

const peoplePerGroup = computed(() => {
  if (!showResult.value) return 0
  // Math.floor 대신 Number로 변경하고 소수점 첫째 자리까지 표시
  return (Number(groups.value) / Number(totalPeople.value)).toFixed(1)
})

const totalAmount = computed(() => {
  if (!multiplyNumber.value) return 0
  // 소수점이 포함된 계산을 위해 Number로 변환
  return (Number(peoplePerGroup.value) * Number(multiplyNumber.value)).toFixed(1)
})

const calculate = () => {
  if (Number(totalPeople.value) < 0) totalPeople.value = '1'
  if (Number(groups.value) < 0) groups.value = '1'
}
</script>

// ... existing code ...

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