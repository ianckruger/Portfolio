<template>
    <section id="projects">
        <div v-if="data">
            <div data-aos="fade-down">
               <Title class="title" :title="data.title" :description="data.description" :whiteBar="false"/>
            </div>

        <div class="container-fluid">
            <div class="projects-grid">
              <div 
                v-for="project in data.projects" 
                :key="project.title" 
                class="project-card"
              >
                <img 
                  v-if="project.image"
                  :src="getImgUrl(project.image)" 
                  class="project-image"
                />

                <h3 v-html="project.title"></h3>
                <p v-html="project.content"></p>

                <router-link 
                  :to="`/projects/${project.slug}`"
                  class="button-link"
                >
                  <button class="view-btn">View Project</button>
                </router-link>
              </div>
            </div>
        </div>
    </div>
    <div v-else>
    Loading...
    </div>
    </section>
</template>

<script setup>
import { ref, onMounted } from "vue";
import Title from '../components/Title.vue';

const data = ref({ title: "", description: "", items: [] });

onMounted(async () => {
    const module = await import('../data/projects.json')
    data.value = module.default
    console.log('experience data loaded:', data.value)
});



function getImgUrl(img) {
    return new URL(`../assets/${img}`, import.meta.url).href;
};
    

</script>

<style>
#projects {
  width: 100%;
  min-height: 80vh;
  padding: 80px 20px;
  background: linear-gradient(to bottom, #f8f9fa, #eef1f5);
}

.title {
  text-align: center;
  margin-bottom: 60px;
}

.container-fluid {
  max-width: 1200px;
  margin: 0 auto;
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 40px;
}

.project-card {
  background: white;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.08);
  transition: all 0.25s ease;
  display: flex;
  flex-direction: column;
  height: 100%;
}


.project-card:hover {
  transform: translateY(-8px);
}

.project-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.project-card:hover .project-image {
  transform: scale(1.05);
}

.project-card h3 {
  font-size: 1.3rem;
  margin: 20px 20px 10px;
  color: #1f2d3d;
}

.project-card p {
  font-size: 0.95rem;
  color: #5f6b7a;
  line-height: 1.6;
  margin: 0 20px 20px;
  flex-grow: 1;
}

.button-link {
  text-decoration: none;
}

.view-btn {
  margin: 0 20px 20px;
  padding: 10px 16px;
  border: none;
  background: #111827;
  color: white;
  border-radius: 8px;
  font-size: 0.9rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
}

.view-btn:hover {
  background: #2563eb;
}


@media (max-width: 768px) {
  #projects {
    padding: 60px 15px;
  }

  .projects-grid {
    gap: 25px;
  }
}

</style>