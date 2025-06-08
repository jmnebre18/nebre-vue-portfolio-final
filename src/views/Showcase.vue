<template>
  <div class="showcase-page">
    <div class="container">
      <!-- Dynamic Header -->
      <header class="page-header">
        <div class="header-content">
          <h1 class="animated-title">Project Showcase</h1>
          <div class="title-underline"></div>
          <p>Engineering solutions through code and creativity</p>
        </div>
        <div class="stats-bar">
          <div class="stat-item">
            <span class="stat-number">{{ projects.length }}</span>
            <span class="stat-label">Projects</span>
          </div>
          <div class="stat-item">
            <span class="stat-number">8</span>
            <span class="stat-label">Technologies</span>
          </div>
          <div class="stat-item">
            <span class="stat-number">2</span>
            <span class="stat-label">Years Learning</span>
          </div>
        </div>
      </header>

      <!-- Filter Tabs -->a
      <div class="filter-section">
        <div class="filter-tabs">
          <button 
            v-for="category in categories" 
            :key="category"
            @click="activeFilter = category"
            :class="['filter-tab', { active: activeFilter === category }]"
          >
            {{ category }}
          </button>
        </div>
      </div>

      <!-- Projects Grid -->
      <div class="projects-container">
        <div class="projects-grid">
          <div 
            v-for="project in filteredProjects" 
            :key="project.id" 
            class="project-card card glow-effect"
            @click="selectProject(project)"
          >
            <div class="project-image">
              <img :src="project.image" :alt="project.title" />
              <div class="project-overlay">
                <div class="overlay-content">
                  <button class="btn btn-primary">View Details</button>
                  <div class="project-links">
                    <a href="#" class="project-link">
                      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path d="M9 19C4 20.5 4 16.5 2 16M22 16V22C22 22.5304 21.7893 23.0391 21.4142 23.4142C21.0391 23.7893 20.5304 24 20 24H16C15.4696 24 14.9609 23.7893 14.5858 23.4142C14.2107 23.0391 14 22.5304 14 22V18.13C14.05 17.32 13.78 16.53 13.26 15.9C15.99 15.58 18.84 14.55 18.84 9.5C18.8381 8.25044 18.3362 7.04767 17.46 6.17C17.8769 5.1128 17.8769 3.9372 17.46 2.88C17.46 2.88 16.42 2.58 14 4.17C11.7573 3.64118 9.40271 3.64118 7.16 4.17C4.74 2.58 3.7 2.88 3.7 2.88C3.28312 3.9372 3.28312 5.1128 3.7 6.17C2.82381 7.04767 2.32193 8.25044 2.32 9.5C2.32 14.55 5.17 15.58 7.9 15.9C7.38 16.53 7.11 17.32 7.16 18.13V22" stroke="currentColor" stroke-width="2"/>
                      </svg>
                    </a>
                    <a href="#" class="project-link">
                      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path d="M18 13V6C18 5.46957 17.7893 4.96086 17.4142 4.58579C17.0391 4.21071 16.5304 4 16 4H8C7.46957 4 6.96086 4.21071 6.58579 4.58579C6.21071 4.96086 6 5.46957 6 6V13" stroke="currentColor" stroke-width="2"/>
                        <path d="M18 13L22 17L18 21" stroke="currentColor" stroke-width="2"/>
                        <path d="M6 13L2 17L6 21" stroke="currentColor" stroke-width="2"/>
                      </svg>
                    </a>
                  </div>
                </div>
              </div>
              <div class="project-status">
                <span :class="['status-badge', project.status.toLowerCase()]">
                  {{ project.status }}
                </span>
              </div>
            </div>
            <div class="project-content">
              <div class="project-header">
                <h3>{{ project.title }}</h3>
                <div class="project-category">{{ project.category }}</div>
              </div>
              <p>{{ project.description }}</p>
              <div class="project-tech">
                <span v-for="tech in project.technologies" :key="tech" class="tech-tag">
                  {{ tech }}
                </span>
              </div>
              <div class="project-footer">
                <div class="project-date">{{ formatDate(project.date) }}</div>
                <div class="project-difficulty">
                  <span class="difficulty-label">Complexity:</span>
                  <div class="difficulty-bars">
                    <div 
                      v-for="i in 5" 
                      :key="i" 
                      :class="['difficulty-bar', { active: i <= project.difficulty }]"
                    ></div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Featured Project Modal -->
      <div v-if="selectedProject" class="modal-overlay" @click="closeModal">
        <div class="modal-content" @click.stop>
          <button class="modal-close" @click="closeModal">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <line x1="18" y1="6" x2="6" y2="18" stroke="currentColor" stroke-width="2"/>
              <line x1="6" y1="6" x2="18" y2="18" stroke="currentColor" stroke-width="2"/>
            </svg>
          </button>
          <div class="modal-header">
            <h2>{{ selectedProject.title }}</h2>
            <div class="modal-meta">
              <span class="modal-category">{{ selectedProject.category }}</span>
              <span class="modal-date">{{ formatDate(selectedProject.date) }}</span>
            </div>
          </div>
          <div class="modal-body">
            <img :src="selectedProject.image" :alt="selectedProject.title" />
            <div class="modal-details">
              <p>{{ selectedProject.fullDescription }}</p>
              <div class="modal-tech">
                <h4>Technologies Used:</h4>
                <div class="tech-list">
                  <span v-for="tech in selectedProject.technologies" :key="tech" class="tech-tag">
                    {{ tech }}
                  </span>
                </div>
              </div>
              <div class="modal-features">
                <h4>Key Features:</h4>
                <ul>
                  <li v-for="feature in selectedProject.features" :key="feature">{{ feature }}</li>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

const activeFilter = ref('All')
const selectedProject = ref(null)

const categories = ['All', 'Web Development', 'Embedded Systems', 'Academic', 'Personal']

const projects = [
  {
    id: 1,
    title: 'Automatic Water Irrigation System',
    description: 'Arduino Uno-based system that automates plant watering based on soil moisture levels and time cycles.',
    fullDescription: 'This project uses Arduino Uno, soil moisture sensors, and a water pump to create an automatic irrigation system. When the soil is detected to be dry or a scheduled interval is reached, the system activates the pump to water the plants. It helps conserve water and ensures consistent plant care.',
    image: 'https://images.pexels.com/photos/6853296/pexels-photo-6853296.jpeg?auto=compress&cs=tinysrgb&w=600',
    technologies: ['Arduino Uno', 'C++', 'Soil Moisture Sensor', 'Relay Module', 'Water Pump'],
    category: 'Embedded Systems',
    status: 'Completed',
    date: new Date('2024-02-10'),
    difficulty: 3,
    features: [
      'Soil moisture detection',
      'Automated watering system',
      'Time-based activation',
      'Water conservation logic',
      'LCD status display'
    ]
  },
  {
    id: 2,
    title: 'LeakGuard: IoT Leak Detection System',
    description: 'An IoT-based leak detection and alert system using NodeMCU and water sensors.',
    fullDescription: 'LeakGuard is a smart leak detection system developed using NodeMCU and multiple water sensors placed in strategic locations in a house. When a leak is detected, the system instantly notifies the user via mobile alert. The system includes data logging for analysis and helps prevent water damage and property loss.',
    image: 'https://images.pexels.com/photos/5271080/pexels-photo-5271080.jpeg?auto=compress&cs=tinysrgb&w=600',
    technologies: ['NodeMCU', 'Water Sensor', 'IoT', 'Wi-Fi', 'Blynk App'],
    category: 'IoT',
    status: 'In Progress',
    date: new Date('2024-03-18'),
    difficulty: 4,
    features: [
      'Real-time leak detection',
      'Instant mobile notification',
      'Multiple sensor support',
      'Wi-Fi connectivity',
      'Data logging and history'
    ]
  },
  {
    id: 3,
    title: 'Customizable Coffee Vending Machine',
    description: 'Raspberry Pi-based coffee vending machine with adjustable levels of sugar, coffee, and creamer.',
    fullDescription: 'This smart vending machine project allows users to customize their coffee by choosing the type and level of sugar, coffee, and creamer. Built using Raspberry Pi, servo motors, and a web interface, it provides an interactive experience and precise ingredient control through software and hardware integration.',
    image: 'https://images.pexels.com/photos/302899/pexels-photo-302899.jpeg?auto=compress&cs=tinysrgb&w=600',
    technologies: ['Raspberry Pi', 'Python', 'Servo Motors', 'HTML/CSS', 'Flask'],
    category: 'Embedded Systems',
    status: 'Completed',
    date: new Date('2024-04-05'),
    difficulty: 5,
    features: [
      'User customization interface',
      'Precise ingredient dispensing',
      'Web-based control panel',
      'Servo-controlled ingredients',
      'Easy-to-maintain design'
    ]
  },
  {
    id: 4,
    title: 'Stress Detection System via Face Recognition',
    description: 'Emotion-based stress detection system using Arduino and facial emotion recognition.',
    fullDescription: 'This system detects a user’s stress level based on their facial expressions using emotion recognition algorithms. Data from the camera is processed, and Arduino triggers alerts or calming responses like ambient lights or music when signs of stress are detected.',
    image: 'https://images.pexels.com/photos/936575/pexels-photo-936575.jpeg?auto=compress&cs=tinysrgb&w=600',
    technologies: ['Arduino Uno', 'Python', 'OpenCV', 'Emotion Recognition API', 'LEDs/Speakers'],
    category: 'Human-Centered Tech',
    status: 'In Progress',
    date: new Date('2024-05-12'),
    difficulty: 5,
    features: [
      'Real-time face emotion detection',
      'Stress level estimation',
      'Arduino-controlled response',
      'Calming alerts via light/sound',
      'Live feedback system'
    ]
  }
]


const filteredProjects = computed(() => {
  if (activeFilter.value === 'All') {
    return projects
  }
  return projects.filter(project => project.category === activeFilter.value)
})

const selectProject = (project) => {
  selectedProject.value = project
}

const closeModal = () => {
  selectedProject.value = null
}

const formatDate = (date: Date): string => {
  return date.toLocaleDateString('en-US', {
    year: 'numeric',
    month: 'short'
  })
}
</script>

<style scoped>
.showcase-page {
  padding: 2rem;
  max-width: 1400px;
  margin: 0 auto;
  min-height: 100vh;
}

.page-header {
  text-align: center;
  margin-bottom: 3rem;
}

.header-content {
  margin-bottom: 2rem;
}

.animated-title {
  font-size: 3rem;
  font-weight: 800;
  background: linear-gradient(135deg, var(--primary-blue), var(--accent-purple));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  margin-bottom: 1rem;
  animation: titleGlow 3s ease-in-out infinite alternate;
}

.title-underline {
  width: 120px;
  height: 4px;
  background: linear-gradient(90deg, var(--primary-blue), var(--accent-purple));
  margin: 0 auto 1rem;
  border-radius: 2px;
  animation: underlineExpand 2s ease-out;
}

.page-header p {
  color: var(--gray-600);
  font-size: 1.25rem;
  font-weight: 500;
}

.stats-bar {
  display: flex;
  justify-content: center;
  gap: 3rem;
  margin-top: 2rem;
}

.stat-item {
  text-align: center;
}

.stat-number {
  display: block;
  font-size: 2rem;
  font-weight: 700;
  color: var(--primary-blue);
  line-height: 1;
}

.stat-label {
  color: var(--gray-600);
  font-size: 0.875rem;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.filter-section {
  margin-bottom: 3rem;
}

.filter-tabs {
  display: flex;
  justify-content: center;
  gap: 0.5rem;
  flex-wrap: wrap;
}

.filter-tab {
  padding: 0.75rem 1.5rem;
  border: 2px solid var(--border-color);
  background: var(--card-bg);
  color: var(--gray-600);
  border-radius: 2rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.3s ease;
}

.filter-tab:hover {
  border-color: var(--primary-blue);
  color: var(--primary-blue);
}

.filter-tab.active {
  background: linear-gradient(135deg, var(--primary-blue), var(--accent-purple));
  color: white;
  border-color: transparent;
  transform: translateY(-2px);
}

.projects-container {
  position: relative;
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
  gap: 2rem;
}

.project-card {
  overflow: hidden;
  transition: all 0.3s ease;
  cursor: pointer;
  background: var(--card-bg);
}

.project-card:hover {
  transform: translateY(-8px);
}

.project-image {
  position: relative;
  height: 250px;
  overflow: hidden;
}

.project-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.project-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(135deg, rgba(59, 130, 246, 0.9), rgba(139, 92, 246, 0.9));
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.project-card:hover .project-overlay {
  opacity: 1;
}

.project-card:hover .project-image img {
  transform: scale(1.1);
}

.overlay-content {
  text-align: center;
}

.project-links {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-top: 1rem;
}

.project-link {
  width: 3rem;
  height: 3rem;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  text-decoration: none;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
}

.project-link:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: scale(1.1);
}

.project-status {
  position: absolute;
  top: 1rem;
  right: 1rem;
}

.status-badge {
  padding: 0.5rem 1rem;
  border-radius: 2rem;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.status-badge.completed {
  background: var(--accent-green);
  color: white;
}

.status-badge.in-progress {
  background: var(--accent-orange);
  color: white;
}

.project-content {
  padding: 2rem;
}

.project-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 1rem;
}

.project-content h3 {
  font-size: 1.5rem;
  font-weight: 600;
  color: var(--gray-900);
  line-height: 1.3;
}

.project-category {
  padding: 0.25rem 0.75rem;
  background: var(--blue-100);
  color: var(--primary-blue);
  border-radius: 1rem;
  font-size: 0.75rem;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.project-content p {
  color: var(--gray-600);
  line-height: 1.6;
  margin-bottom: 1.5rem;
}

.project-tech {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}

.tech-tag {
  padding: 0.375rem 0.75rem;
  background: linear-gradient(135deg, var(--gray-200), var(--gray-300));
  color: var(--gray-700);
  border-radius: 1rem;
  font-size: 0.75rem;
  font-weight: 500;
  border: 1px solid var(--border-color);
  transition: all 0.3s ease;
}

.tech-tag:hover {
  background: linear-gradient(135deg, var(--primary-blue), var(--accent-purple));
  color: white;
  transform: translateY(-1px);
}

.project-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 1rem;
  border-top: 1px solid var(--border-color);
}

.project-date {
  color: var(--gray-500);
  font-size: 0.875rem;
  font-weight: 500;
}

.project-difficulty {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.difficulty-label {
  color: var(--gray-500);
  font-size: 0.75rem;
  font-weight: 500;
}

.difficulty-bars {
  display: flex;
  gap: 2px;
}

.difficulty-bar {
  width: 4px;
  height: 16px;
  background: var(--gray-300);
  border-radius: 2px;
  transition: all 0.3s ease;
}

.difficulty-bar.active {
  background: linear-gradient(135deg, var(--primary-blue), var(--accent-purple));
}

/* Modal Styles */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 2rem;
}

.modal-content {
  background: var(--card-bg);
  border-radius: 1.5rem;
  max-width: 800px;
  width: 100%;
  max-height: 90vh;
  overflow-y: auto;
  position: relative;
  border: 1px solid var(--border-color);
}

.modal-close {
  position: absolute;
  top: 1rem;
  right: 1rem;
  width: 3rem;
  height: 3rem;
  background: var(--gray-200);
  border: none;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  color: var(--gray-600);
  transition: all 0.3s ease;
  z-index: 10;
}

.modal-close:hover {
  background: var(--accent-red);
  color: white;
}

.modal-header {
  padding: 2rem 2rem 1rem;
}

.modal-header h2 {
  font-size: 2rem;
  font-weight: 700;
  color: var(--gray-900);
  margin-bottom: 0.5rem;
}

.modal-meta {
  display: flex;
  gap: 1rem;
  align-items: center;
}

.modal-category {
  padding: 0.25rem 0.75rem;
  background: var(--blue-100);
  color: var(--primary-blue);
  border-radius: 1rem;
  font-size: 0.75rem;
  font-weight: 500;
}

.modal-date {
  color: var(--gray-500);
  font-size: 0.875rem;
}

.modal-body {
  padding: 0 2rem 2rem;
}

.modal-body img {
  width: 100%;
  height: 300px;
  object-fit: cover;
  border-radius: 1rem;
  margin-bottom: 2rem;
}

.modal-details p {
  color: var(--gray-600);
  line-height: 1.7;
  margin-bottom: 2rem;
  font-size: 1.1rem;
}

.modal-tech,
.modal-features {
  margin-bottom: 2rem;
}

.modal-tech h4,
.modal-features h4 {
  color: var(--gray-900);
  font-weight: 600;
  margin-bottom: 1rem;
}

.tech-list {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.modal-features ul {
  list-style: none;
  padding: 0;
}

.modal-features li {
  padding: 0.5rem 0;
  color: var(--gray-600);
  position: relative;
  padding-left: 1.5rem;
}

.modal-features li::before {
  content: '✓';
  position: absolute;
  left: 0;
  color: var(--accent-green);
  font-weight: bold;
}

@keyframes titleGlow {
  0% { filter: brightness(1); }
  100% { filter: brightness(1.2); }
}

@keyframes underlineExpand {
  0% { width: 0; }
  100% { width: 120px; }
}

@media (max-width: 768px) {
  .showcase-page {
    padding: 1rem;
  }
  
  .animated-title {
    font-size: 2rem;
  }
  
  .stats-bar {
    gap: 2rem;
  }
  
  .projects-grid {
    grid-template-columns: 1fr;
  }
  
  .filter-tabs {
    justify-content: flex-start;
    overflow-x: auto;
    padding-bottom: 0.5rem;
  }
  
  .modal-overlay {
    padding: 1rem;
  }
  
  .modal-content {
    max-height: 95vh;
  }
}
</style>