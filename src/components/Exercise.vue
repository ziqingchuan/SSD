<script setup lang="ts">
import { ref, computed } from 'vue';
import { shortAnswer } from "../consts/shortAnswer.ts";
import Light from "../icons/Light.vue";
import Dark from "../icons/Dark.vue";
import Thinking from "../icons/Thinking.vue";

// 简答题状态管理
const shortAnswerIndex = ref(0); // 当前简答题索引
const currentShortAnswer = computed(() => shortAnswer[shortAnswerIndex.value]); // 当前简答题

// 上一简答题
const prevShortAnswer = () => {
  isFlipped.value = true; // 切换卡片翻转状态
  if (shortAnswerIndex.value > 0) {
    shortAnswerIndex.value--;
  }
};

// 下一简答题
const nextShortAnswer = () => {
  isFlipped.value = true; // 切换卡片翻转状态
  if (shortAnswerIndex.value < shortAnswer.length - 1) {
    shortAnswerIndex.value++;
  }
};

// 暗色模式切换
const isDarkMode = ref(false); // 是否为暗色模式

const toggleDarkMode = () => {
  isDarkMode.value = !isDarkMode.value;
};

// 卡片翻转状态
const isFlipped = ref(true); // 是否翻转

// 切换卡片翻转状态
const toggleFlip = () => {
  isFlipped.value = !isFlipped.value;
};
</script>

<template>
  <div :class="['container', { 'dark-mode': isDarkMode }]">
    <div class="header">
      <h1>软件系统设计</h1>
      <Light class="dark-mode-btn" @click="toggleDarkMode" v-if="!isDarkMode" />
      <Dark class="dark-mode-btn" @click="toggleDarkMode" v-else />
    </div>

    <div class="link-container">
      <a href="https://ziqingchuan.github.io/SQM/" target="_blank" class="link-btn">
        软件质量管理复习网站
      </a>
    </div>

    <!-- 简答题部分 -->
    <div class="short-answer-container">
      <div class="short-answer-question">{{ currentShortAnswer.question }}</div>
      <div class="short-answer-answer" @click="toggleFlip">
        <p v-if="!isFlipped">{{ currentShortAnswer.answer }}</p>
        <div v-else class="thinking-mode">
          <Thinking />
          <span>答案是什么来着？</span>
        </div>
      </div>
      <div class="controls">
        <button
            class="btn btn-prev"
            @click="prevShortAnswer"
            :disabled="shortAnswerIndex === 0"
        >
          <i class="fas fa-arrow-left"></i> 上一题
        </button>
        <button
            class="btn btn-next"
            @click="nextShortAnswer"
            :disabled="shortAnswerIndex === shortAnswer.length - 1"
        >
          下一题 <i class="fas fa-arrow-right"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  width: 100%;
  height: 100%;
  margin: 0 auto;
  background: white;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.1);
  overflow-y: auto;
}

.header {
  background: linear-gradient(135deg, #5767bb, #26d0ce);
  color: white;
  padding: 25px 30px;
  height: 150px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.header h1 {
  font-size: 2.2rem;
  margin-bottom: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.controls {
  display: flex;
  justify-content: space-between;
  padding: 20px 30px;
}

.btn {
  padding: 10px 20px;
  border: none;
  border-radius: 50px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  font-size: 14px;
}

.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.btn-prev {
  background: #f0f0f0;
  color: #555;
}

.btn-prev:hover:not(:disabled) {
  background: #e0e0e0;
  color: #333;
}

.btn-next {
  background: linear-gradient(135deg, #5767bb, #26d0ce);
  color: white;
}

.btn-next:hover:not(:disabled) {
  transform: translateY(-3px);
  box-shadow: 0 5px 15px rgba(26, 41, 128, 0.2);
}

.short-answer-container {
  padding: 30px;
}

.short-answer-question {
  text-align: left;
  font-size: 1.25rem;
  margin-bottom: 20px;
}

.short-answer-answer {
  text-align: left;
  font-size: 1.2rem;
  color: #333;
  background: #fdfcfc;
  border-radius: 10px;
  border: 1px solid #e0e0e0;
  padding: 20px;
  white-space: pre-line;
  box-shadow: 5px 5px 5px 3px rgba(0, 0, 0, 0.05);
}

.link-btn {
  display: inline-block;
  margin-top: 30px;
  padding: 6px 14px;
  background: linear-gradient(135deg, #5767bb, #26d0ce);
  color: white;
  text-decoration: none;
  border-radius: 20px;
  font-size: 0.9rem;
  font-weight: bold;
  transition: all 0.3s ease;
  text-align: center;
}

.link-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 5px 15px rgba(26, 41, 128, 0.2);
}

.link-container {
  padding-left: 30px;
}

/* 暗色模式 */
.dark-mode {
  background: #1e1e1e;
  color: #e0e0e0;
}

.dark-mode .header {
  background: linear-gradient(135deg, #333, #444);
  color: #fff;
}

.dark-mode .short-answer-answer {
  background: #262626;
  color: #cfcfcf;
  border-color: #333;
}

.dark-mode .btn {
  background: #333;
  color: #fff;
}

.dark-mode .btn-next {
  background: linear-gradient(135deg, #444, #555);
}

.dark-mode .btn-prev {
  background: #2c2c2c;
}

.dark-mode .btn-next:hover:not(:disabled),
.dark-mode .btn-prev:hover:not(:disabled) {
  background: #555;
  color: #fff;
}

.dark-mode .link-btn {
  background: #333;
  color: #fff;
}

.dark-mode .link-btn:hover {
  background: #444;
}

/* 切换按钮样式 */
.dark-mode-btn {
  cursor: pointer;
  position: absolute;
  top: 0;
  right: 10px;
  transition: all 0.5s ease;
}

.dark-mode-btn:hover {
  transform: translateY(10px);
}
.thinking-mode {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>