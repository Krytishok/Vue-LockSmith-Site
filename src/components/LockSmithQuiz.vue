<template>
    <div class="quiz-container" v-if="showQuiz">
      <div class="quiz-window">
        <div class="quiz-header">
          <h3>Locksmith Service Quiz</h3>
          <button @click="closeQuiz" class="close-btn">×</button>
        </div>
        
        <div class="quiz-content">
          <div class="question" v-if="currentQuestion < questions.length">
            <p class="question-text">{{ questions[currentQuestion].text }}</p>
            <div class="options">
              <button 
                v-for="(option, index) in questions[currentQuestion].options" 
                :key="index"
                @click="selectOption(index)"
                :class="{ 'selected': selectedOption === index }"
              >
                {{ option.text }}
              </button>
            </div>
          </div>
          
          <div class="quiz-results" v-else>
            <h4>Your Locksmith Knowledge Score</h4>
            <p class="score">{{ calculateScore() }}/10</p>
            <p class="result-message">{{ getResultMessage() }}</p>
            <button @click="restartQuiz" class="restart-btn">Take Quiz Again</button>
          </div>
          
          <div class="quiz-controls">
            <button 
              @click="prevQuestion" 
              :disabled="currentQuestion === 0"
              class="nav-btn"
            >
              Previous
            </button>
            <span class="progress">{{ currentQuestion }}/{{ questions.length }}</span>
            <button 
              @click="nextQuestion" 
              :disabled="selectedOption === null && currentQuestion < questions.length"
              class="nav-btn"
            >
              {{ currentQuestion === questions.length - 1 ? 'Finish' : 'Next' }}
            </button>
          </div>
        </div>
      </div>
    </div>
    
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const showQuiz = ref(false);
  const currentQuestion = ref(0);
  const selectedOption = ref(null);
  const answers = ref([]);
  
  const questions = [
    {
      text: "What should you do if you're locked out of your house?",
      options: [
        { text: "Try to pick the lock yourself", correct: false },
        { text: "Call a professional locksmith", correct: true },
        { text: "Break a window to get in", correct: false },
        { text: "Wait outside until someone comes", correct: false }
      ]
    },
    {
      text: "How often should you rekey your home locks?",
      options: [
        { text: "Every 6 months", correct: false },
        { text: "When you move into a new home", correct: true },
        { text: "Never, unless they break", correct: false },
        { text: "Every year", correct: false }
      ]
    },
    {
      text: "Which lock type is most secure for exterior doors?",
      options: [
        { text: "Deadbolt", correct: true },
        { text: "Knob lock", correct: false },
        { text: "Padlock", correct: false },
        { text: "Latch lock", correct: false }
      ]
    },
    {
      text: "What's the best way to prevent car lockouts?",
      options: [
        { text: "Hide a spare key in the wheel well", correct: false },
        { text: "Always take keys when exiting", correct: true },
        { text: "Leave window slightly open", correct: false },
        { text: "Use keyless entry system", correct: true }
      ]
    },
    {
      text: "When should you consider a smart lock?",
      options: [
        { text: "When you want keyless entry", correct: true },
        { text: "When you lose your keys often", correct: true },
        { text: "For rental properties", correct: true },
        { text: "All of the above", correct: true }
      ]
    },
    {
      text: "What's the first thing to check if your key won't turn?",
      options: [
        { text: "If the key is bent", correct: false },
        { text: "If the lock is frozen", correct: false },
        { text: "If you're using the right key", correct: true },
        { text: "If the door is warped", correct: false }
      ]
    },
    {
      text: "Why should you avoid DIY lock picking?",
      options: [
        { text: "It can damage the lock", correct: true },
        { text: "It's often illegal", correct: true },
        { text: "Professional tools are required", correct: true },
        { text: "All of the above", correct: true }
      ]
    },
    {
      text: "What's the benefit of master key systems?",
      options: [
        { text: "Single key for multiple locks", correct: true },
        { text: "Different access levels", correct: true },
        { text: "Easy to rekey", correct: true },
        { text: "All of the above", correct: true }
      ]
    },
    {
      text: "How can you make your home more secure?",
      options: [
        { text: "Install high-security locks", correct: true },
        { text: "Use door reinforcement", correct: true },
        { text: "Add security cameras", correct: true },
        { text: "All of the above", correct: true }
      ]
    },
    {
      text: "What should you do after losing your keys?",
      options: [
        { text: "Wait to see if they turn up", correct: false },
        { text: "Change all affected locks", correct: true },
        { text: "Make copies from memory", correct: false },
        { text: "Post on social media", correct: false }
      ]
    }
  ];
  
  const selectOption = (index) => {
    selectedOption.value = index;
  };
  
  const nextQuestion = () => {
    if (currentQuestion.value < questions.length) {
      answers.value[currentQuestion.value] = selectedOption.value;
      currentQuestion.value++;
      selectedOption.value = null;
    }
  };
  
  const prevQuestion = () => {
    if (currentQuestion.value > 0) {
      currentQuestion.value--;
      selectedOption.value = answers.value[currentQuestion.value];
    }
  };
  
  const calculateScore = () => {
    return answers.value.reduce((score, answer, index) => {
      return score + (questions[index].options[answer]?.correct ? 1 : 0);
    }, 0);
  };
  
  const getResultMessage = () => {
    const score = calculateScore();
    if (score >= 9) return "Excellent! You're a locksmith expert!";
    if (score >= 6) return "Good job! You know your locksmith basics.";
    if (score >= 3) return "Not bad, but you could learn more about security.";
    return "Consider learning more about home security and locksmith services.";
  };
  
  const restartQuiz = () => {
    currentQuestion.value = 0;
    selectedOption.value = null;
    answers.value = [];
  };
  
  const closeQuiz = () => {
    showQuiz.value = false;
    // Reset only if quiz wasn't completed
    if (currentQuestion.value < questions.length) {
      restartQuiz();
    }
  };


    defineExpose({
        showQuiz
    });
  </script>
  
  <style scoped>
  .quiz-container {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
  }
  
  .quiz-window {
    background-color: white;
    border-radius: 10px;
    width: 90%;
    max-width: 500px;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
    overflow: hidden;
  }
  
  .quiz-header {
    background-color: #3b82f6;
    color: white;
    padding: 15px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  .quiz-header h3 {
    margin: 0;
    font-size: 1.2rem;
  }
  
  .close-btn {
    background: none;
    border: none;
    color: white;
    font-size: 1.5rem;
    cursor: pointer;
    padding: 0 10px;
  }
  
  .quiz-content {
    padding: 20px;
  }
  
  .question-text {
    font-size: 1.1rem;
    margin-bottom: 20px;
    font-weight: 500;
  }
  
  .options {
    display: grid;
    grid-template-columns: 1fr;
    gap: 10px;
    margin-bottom: 25px;
  }
  
  .options button {
    padding: 12px;
    border: 1px solid #ddd;
    border-radius: 5px;
    background-color: #f8fafc;
    cursor: pointer;
    text-align: left;
    transition: all 0.2s;
  }
  
  .options button:hover {
    background-color: #eef2ff;
  }
  
  .options button.selected {
    background-color: #3b82f6;
    color: white;
    border-color: #3b82f6;
  }
  
  .quiz-controls {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 20px;
  }
  
  .nav-btn {
    padding: 8px 15px;
    border: none;
    border-radius: 5px;
    background-color: #3b82f6;
    color: white;
    cursor: pointer;
  }
  
  .nav-btn:disabled {
    background-color: #94a3b8;
    cursor: not-allowed;
  }
  
  .progress {
    font-size: 0.9rem;
    color: #64748b;
  }
  
  .quiz-results {
    text-align: center;
  }
  
  .score {
    font-size: 2rem;
    color: #3b82f6;
    margin: 15px 0;
    font-weight: bold;
  }
  
  .result-message {
    margin-bottom: 20px;
    font-size: 1.1rem;
  }
  
  .restart-btn {
    padding: 10px 20px;
    background-color: #3b82f6;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 1rem;
    margin-top: 15px;
  }
  
  .quiz-toggle-btn {
    position: fixed;
    bottom: 20px;
    right: 20px;
    padding: 12px 20px;
    background-color: #3b82f6;
    color: white;
    border: none;
    border-radius: 30px;
    cursor: pointer;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
    z-index: 999;
  }
  
  @media (max-width: 768px) {
    .quiz-window {
      width: 95%;
    }
    
    .quiz-toggle-btn {
      bottom: 10px;
      right: 10px;
      padding: 10px 15px;
      font-size: 0.9rem;
    }
  }
  </style>