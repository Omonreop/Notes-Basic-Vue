<script setup>
import { ref } from "vue";
const showForm = ref(false);
const newMemo = ref("");
const memos = ref([]);
const errorMsg = ref("");

function addMemo() {
  if (!newMemo.value) {
    errorMsg.value = "Please Enter Memo!";
    return;
  }
  memos.value.push({
    id: Date.now(),
    memo: newMemo.value,
    date: new Date().toLocaleDateString("en-us"),
    backgroundColor: getRandomColor(),
  });
  newMemo.value = "";
  showForm.value = false;
  errorMsg.value = "";
}

function deleteMemo(id) {
  memos.value = memos.value.filter((memo) => memo.id !== id);
}

function getRandomColor() {
  return `#${Math.floor(Math.random() * 16777215).toString(16)}`;
}
</script>

<template>
  <main>
    <div class="container">
      <header>
        <h1 class="h-title">Notes</h1>
        <button @click="showForm = true" class="h-button">+</button>
      </header>
      <div class="card-container">
        <div
          v-for="(memo, index) in memos"
          class="card"
          :key="index"
          :style="{ backgroundColor: memo.backgroundColor }"
        >
          <p class="card-content">
            {{ memo.memo }}
          </p>
          <div class="card-footer">
            <p class="card-date">{{ memo.date }}</p>
            <button @click="deleteMemo(memo.id)" class="card-button">X</button>
          </div>
        </div>
      </div>
    </div>
    <div v-if="showForm" class="form-overlay">
      <div class="form-modal">
        <button @click="showForm = false" class="form-close-btn">
          &times;
        </button>
        <p v-if="errorMsg" class="form-error">{{ errorMsg }}</p>
        <textarea v-model="newMemo" name="memo" id="memo" cols="30" rows="10">
        </textarea>
        <button @click="addMemo" class="form-save-btn">Save</button>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
  background-color: bisque;
}
.container {
  max-width: 900px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.h-title {
  font-size: 48px;
  font-weight: bold;
  margin-bottom: 25px;
  color: brown;
}
.h-button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  border-radius: 100%;
  background-color: brown;
  color: white;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}
.card {
  width: 225px;
  height: 225px;
  padding: 10px;
  background-color: burlywood;
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.card-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.card-button {
  border: none;
  padding: 10px;
  width: 30px;
  height: 30px;
  cursor: pointer;
  border-radius: 10%;
}
.card-button:hover {
  background-color: red;
}
.form-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}
.form-modal {
  width: 420px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}
.form-save-btn {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: brown;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  margin-top: 15px;
  color: white;
}
.form-close-btn {
  position: absolute;
  top: 5px;
  right: 10px;
  width: 30px;
  height: 30px;
  background: transparent;
  border: none;
  font-size: 25px;
  cursor: pointer;
}
.form-error {
  color: red;
}
</style>
