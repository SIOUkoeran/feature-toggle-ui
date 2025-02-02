<template>
  <div class="p-4 max-w-4xl mx-auto">
    <h1 class="text-2xl font-bold mb-4">Feature Toggle Management</h1>
    
    <div class="grid gap-4">
      <div v-for="toggle in toggles" 
           :key="toggle.feature" 
           class="p-4 border rounded-lg">
        <div class="flex justify-between items-center">
          <h2 class="text-xl font-semibold">{{ toggle.feature }}</h2>
          <label class="switch">
            <input type="checkbox" 
                   v-model="toggle.isEnabled" 
                   @change="toggleFeature(toggle.feature, toggle.isEnabled)">
            <span class="slider round"></span>
          </label>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
/* Toggle Switch styles */
.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  transition: .4s;
}

input:checked + .slider {
  background-color: #2196F3;
}

input:checked + .slider:before {
  transform: translateX(26px);
}

.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
</style>

<script>
import axios from 'axios'

axios.defaults.baseURL = 'http://localhost:8080'

export default {
  name: 'App',
  data() {
    return {
      toggles: []
    }
  },
  methods: {
    async fetchFeatureToggles() {
      try {
        const response = await axios.get('/api/v1/feature/toggle')
        this.toggles = response.data.toggles
        console.log('Fetched toggles:', this.toggles) // Debug log
      } catch (error) {
        console.error('Error fetching feature toggles:', error)
      }
    },
    async toggleFeature(feature, isEnabled) {
      try {
        await axios.put(`/api/v1/feature/toggle?toggleId=${feature}&enabled=${isEnabled}`)
        await this.fetchFeatureToggles()
      } catch (error) {
        console.error('Error toggling feature:', error)
      }
    }
  },
  mounted() {
    this.fetchFeatureToggles()
  }
}
</script>

<style>
.toggle-checkbox {
  width: 20px;
  height: 20px;
  cursor: pointer;
}
</style>