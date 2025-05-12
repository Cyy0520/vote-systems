<template>
  <div class="container">
    <h1 class="title">请为你最喜欢的人投票</h1>
    <div class="candidates-container">
      <CandidateCard
        v-for="candidate in candidates"
        :key="candidate.id"
        :candidate="candidate"
        @vote="handleVote"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import CandidateCard from './components/CandidateCard.vue';

const candidates = ref([]);

onMounted(() => {
  const savedData = localStorage.getItem('voteData');
  candidates.value = savedData ? JSON.parse(savedData) : initDefaultCandidates();
});

const initDefaultCandidates = () => [
  { id: 1, name: '张伟', votes: 221 },
  { id: 2, name: '王芳', votes: 189 },
  { id: 3, name: '李明', votes: 443 }
];
const handleVote = (candidateId) => {
  const candidate = candidates.value.find(c => c.id === candidateId);
  if (candidate) candidate.votes++;
  saveToLocalStorage();
};

const saveToLocalStorage = () => {
  try {
    localStorage.setItem('voteData', JSON.stringify(candidates.value));
  } catch (e) {
    console.error('存储失败:', e);
  }
};
</script>

<style>
body {
  background: #f0f2f5;
  margin: 0;
  font-family: 'Helvetica Neue', Arial, sans-serif;
}

.container {
  max-width: 1200px;
  margin: 2rem auto;
  padding: 0 20px;
}

.title {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 2rem;
}

.candidates-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 1.5rem;
  justify-content: center;
}
</style>