<template>
  <div class="candidate-card">
    <div class="avatar"></div>
    <h3>{{ candidate.name }}</h3>
    <p class="votes-count">票数: {{ votes }}</p>
    <button 
      class="vote-btn"
      @click="handleVote"
      :disabled="isVoted"
    >
      {{ isVoted ? '已投票' : '投票' }}
    </button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const props = defineProps({
  candidate: {
    type: Object,
    required: true
  }
});

const emit = defineEmits(['vote']);

// 使用本地响应式状态保证数据隔离
const votes = ref(props.candidate.votes);
const isVoted = ref(false);

const handleVote = () => {
  if (!isVoted.value) {
    votes.value++;
    isVoted.value = true;
    emit('vote', props.candidate.id);
  }
};
</script>

<style scoped>
.candidate-card {
  background: white;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  transition: transform 0.2s;
  width: 240px;
  text-align: center;
}

.candidate-card:hover {
  transform: translateY(-5px);
}

.avatar {
  width: 80px;
  height: 80px;
  background: #eee;
  border-radius: 50%;
  margin: 0 auto 15px;
}

.votes-count {
  color: #666;
  font-size: 1.1em;
  margin: 10px 0;
}

.vote-btn {
  background: #4CAF50;
  color: white;
  padding: 8px 20px;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  transition: background 0.3s;
}

.vote-btn:disabled {
  background: #cccccc;
  cursor: not-allowed;
}

.vote-btn:not(:disabled):hover {
  background: #45a049;
}
</style>