<template>
  <div class="app-container">
    <h1 class="app-title">Daftar Kegiatan</h1>
    
    <div class="input-section">
      <input 
        v-model="newActivity" 
        @keyup.enter="addActivity" 
        placeholder="Tambahkan kegiatan baru..."
        class="activity-input"
      >
      <button @click="addActivity" class="add-button">Tambah</button>
    </div>
    
    <div class="filter-section">
      <button 
        @click="filter = 'all'" 
        :class="{ active: filter === 'all' }"
        class="filter-button"
      >
        Semua
      </button>
      <button 
        @click="filter = 'active'" 
        :class="{ active: filter === 'active' }"
        class="filter-button"
      >
        Belum Selesai
      </button>
    </div>
    
    <ul class="activity-list">
      <li 
        v-for="(activity, index) in filteredActivities" 
        :key="index" 
        class="activity-item"
      >
        <input 
          type="checkbox" 
          v-model="activity.completed" 
          @change="toggleComplete(index)"
          class="activity-checkbox"
        >
        <span 
          :class="{ completed: activity.completed }"
          class="activity-text"
        >
          {{ activity.text }}
        </span>
        <button 
          @click="removeActivity(index)" 
          class="delete-button"
        >
          Hapus
        </button>
      </li>
    </ul>
    
    <div v-if="activities.length === 0" class="empty-state">
      Tidak ada kegiatan yang ditambahkan
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newActivity: '',
      activities: [],
      filter: 'all'
    }
  },
  computed: {
    filteredActivities() {
      if (this.filter === 'active') {
        return this.activities.filter(activity => !activity.completed)
      }
      return this.activities
    }
  },
  methods: {
    addActivity() {
      if (this.newActivity.trim() === '') return
      this.activities.push({
        text: this.newActivity,
        completed: false
      })
      this.newActivity = ''
      this.saveToLocalStorage()
    },
    removeActivity(index) {
      this.activities.splice(index, 1)
      this.saveToLocalStorage()
    },
    toggleComplete(index) {
      this.activities[index].completed = !this.activities[index].completed
      this.saveToLocalStorage()
    },
    saveToLocalStorage() {
      localStorage.setItem('activities', JSON.stringify(this.activities))
    },
    loadFromLocalStorage() {
      const savedActivities = localStorage.getItem('activities')
      if (savedActivities) {
        this.activities = JSON.parse(savedActivities)
      }
    }
  },
  mounted() {
    this.loadFromLocalStorage()
  }
}
</script>

<style>
/* Reset CSS */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

/* App Styles */
.app-container {
  max-width: 600px;
  margin: 2rem auto;
  padding: 2rem;
  background-color: #f8f9fa;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.app-title {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 1.5rem;
  font-size: 2rem;
}

.input-section {
  display: flex;
  margin-bottom: 1.5rem;
  gap: 0.5rem;
}

.activity-input {
  flex: 1;
  padding: 0.75rem;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 1rem;
  transition: border-color 0.3s;
}

.activity-input:focus {
  outline: none;
  border-color: #42b983;
}

.add-button {
  padding: 0.75rem 1.5rem;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s;
}

.add-button:hover {
  background-color: #369f6e;
}

.filter-section {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}

.filter-button {
  padding: 0.5rem 1rem;
  background-color: #e9ecef;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s;
}

.filter-button.active {
  background-color: #42b983;
  color: white;
}

.filter-button:hover {
  background-color: #dee2e6;
}

.filter-button.active:hover {
  background-color: #369f6e;
}

.activity-list {
  list-style: none;
}

.activity-item {
  display: flex;
  align-items: center;
  padding: 0.75rem;
  background-color: white;
  border-radius: 5px;
  margin-bottom: 0.5rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.activity-checkbox {
  margin-right: 1rem;
  cursor: pointer;
}

.activity-text {
  flex: 1;
  color: #2c3e50;
  transition: all 0.3s;
}

.activity-text.completed {
  text-decoration: line-through;
  color: #95a5a6;
}

.delete-button {
  padding: 0.5rem 1rem;
  background-color: #e74c3c;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.delete-button:hover {
  background-color: #c0392b;
}

.empty-state {
  text-align: center;
  color: #95a5a6;
  padding: 1rem;
  background-color: white;
  border-radius: 5px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}
</style>