<script>

import { onMounted, onBeforeUnmount, ref } from 'vue';
import { Pie } from 'vue-chartjs';

export default {
  name: 'CombinedComponent',
  setup() {
    // Task 1: Carousel Component
    const carouselImages = ref([
      'https://example.com/image1.jpg',
      'https://example.com/image2.jpg',
      'https://example.com/image3.jpg',
    ]);
    const currentImageIndex = ref(0);

    const nextImage = () => {
      currentImageIndex.value = (currentImageIndex.value + 1) % carouselImages.value.length;
    };

    let carouselInterval;

    onMounted(() => {
      startCarousel();
    });

    onBeforeUnmount(() => {
      clearInterval(carouselInterval);
    });

    const startCarousel = () => {
      nextImage();
      carouselInterval = setInterval(nextImage, 3000);
    };

    // Task 2: Pie Chart Component
    const chartCanvas = ref(null);
    let chart;

    const chartData = ref({
      labels: ['Red', 'Blue', 'Yellow'],
      datasets: [
        {
          data: [300, 50, 100],
          backgroundColor: ['#FF6384', '#36A2EB', '#FFCE56'],
        },
      ],
    });

    onMounted(() => {
      if (chartCanvas.value) {
        chart = new Pie(chartCanvas.value, {
          data: chartData.value,
        });
      }
    });

    onBeforeUnmount(() => {
      if (chart) {
        chart.destroy();
      }
    });

    return {
      currentImageIndex,
      currentImage: computed(() => carouselImages.value[currentImageIndex.value]),
      chartCanvas,
      chartData,
    };
  },
};




</script>

<template>

<div class="container">
    
    <div class="carousel">
      <transition name="slide-fade" mode="out-in">
        <img :key="currentImageIndex" :src="currentImage" alt="Carousel Image" />
      </transition>
    </div>

   
    <div class="pie-chart">
      <canvas ref="chartCanvas"></canvas>
    </div>
  </div>
  




</template>

<style scoped>

.container {
  display: flex;
  justify-content: space-around;
  align-items: center;
  padding: 20px;
}

.carousel,
.pie-chart {
  width: 300px;
  height: 300px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #ccc;
  margin: 10px;
}

.slide-fade-enter-active,
.slide-fade-leave-active {
  transition: opacity 1s;
}
.slide-fade-enter,
.slide-fade-leave-to {
  opacity: 0;
}




</style>
