<script setup>
import { ref } from "vue";

const tampilkanOverlay = ref(false);
const newNote = ref("");
const notes = ref([]);
const pesanError = ref("");

function simpanNote() {
  if (newNote.value === "") {
    pesanError.value = "Maaf bro form gak boleh kosong ya hehehe";
    return;
  } else {
    pesanError.value = "";
  }
  notes.value.push({ 
    id: Date.now(),
    note: newNote.value,
    date: new Date().toLocaleDateString("en-US"),
    backgroundColor: gantiWarna(),
  });

  newNote.value = "";
  tampilkanOverlay.value = false;
};

function hapusNote(id) {
  notes.value = notes.value.filter((note) => note.id !== id);
};

function gantiWarna() {
  return `#${Math.floor(Math.random() * 16777215).toString(16)}`
};
</script>

<template>
  <main>
    <div class="container">
      <header>
        <h1 class="header-title">Simple Note</h1>
        <button @click="tampilkanOverlay = true" class="header-button">
          +
        </button>
      </header>
      <div class="card-container">
        <div v-for="note in notes" class="card" :key="note.id" :style="{ backgroundColor: note.backgroundColor }">
          <p class="card-content">
            {{ note.note }}
          </p>
          <div class="card-footer">
            <p class="card-date">{{ note.date }}</p>
            <button @click="hapusNote(note.id)" class="card-delete-btn">
            &times;
          </button>
          </div>
        </div>
      </div>
      <div v-if="tampilkanOverlay" class="form-overlay">
        <div class="form-modal">
          <button @click="tampilkanOverlay = false" class="form-close-btn">
            &times;
          </button>
          <p class="form-error">{{ pesanError }}</p>
          <textarea v-model="newNote" name="note" id="note" cols="30" rows="10"></textarea>
          <button @click="simpanNote" class="form-save">Simpan</button>
        </div>
      </div>
    </div>
  </main>
</template>

<style>
main {
  height: 100vh;
  width: 100vw;
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

.header-title {
  font-size: 48px;
  font-weight: bold;
  margin-bottom: 25px;
  color: #075a67;
}

.header-button {
  border: none;
  background-color: #075a67;
  color: #fff;
  font-size: 18px;
  border-radius: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.header-button:hover {
  background-color: #00334d;
  color: #fff;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.card {
  width: 255px;
  height: 255px;
  padding: 10px;
  background-color: #ffa6c1;
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.card-footer {
  display: flex;
  justify-content: space-between;
  align-items: center
  ;
}

.card-delete-btn {
  align-self: flex-end;
  background-color: #075a67;
  color: #fff;
  font-size: 20px;
  border: none;
  border-radius: 20%;
  cursor: pointer;
}

.form-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  justify-content: center;
  align-items: center;
}

.form-modal {
  width: 420px;
  background-color: #fff;
  padding: 30px;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.form-save {
  margin-top: 20px;
  width: 100%;
  padding: 10px 20px;
  background-color: #075a67;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.form-save:hover {
  background-color: #00334d;
  color: #fff;
}

.form-close-btn {
  position: absolute;
  top: 5px;
  right: 5px;
  background-color: #075a67;
  color: #fff;
  font-size: 20px;
  border: none;
  border-radius: 20%;
  cursor: pointer;
}

.form-error {
  color: red;
  margin-bottom: 10px;
}
</style>
