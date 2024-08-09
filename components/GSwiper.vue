<template>
  <swiper
    @slide-change="removeClasses"
    @init="addClasses"
    @slideChangeTransitionEnd="addClasses"
    :centered-slides="true"
    :slides-per-group="1"
    :slides-per-view="5.5"
    :allow-touch-move="false"
    :loop="false"
    :navigation="true"
    :modules="modules"
    :pagination="{ type: 'fraction'}"
    >
    <swiper-slide
    v-for="(slideContent, index) in slides" :key="index">
      <img :src="images[index]"/>
    </swiper-slide>
  </swiper>
</template>
<script setup>
  import { filename } from 'pathe/utils'
  import 'swiper/css';
  import 'swiper/css/navigation';
  import 'swiper/css/pagination';
  import { Swiper, SwiperSlide } from 'swiper/vue';
  import { Navigation, Pagination, Scrollbar, A11y } from 'swiper/modules';

  const modules = [Navigation, Pagination, Scrollbar, A11y];
  const glob = import.meta.glob('@/assets/pics/*.png', { eager: true })
  const images = Object.fromEntries(
    Object.entries(glob).map(([key, value]) => [filename(key), value.default])
  )
  const photos = [
    "0.png",
    "1.png",
    "2.png",
    "3.png",
    "4.png",
    "5.png",
    "6.png",
    "61.png",
    "62.png",
    "63.png",
    "64.png",
  ];
  const slides = photos.map(photo => photo);

  const removeClasses = (swiper) => {
    console.log(swiper.slides)
    swiper.slides.map(e => {
      console.log(e, e.classList.contains("slide-last"))
      if (e.classList.contains("slide-last")) {
        e.classList.remove("slide-last");
      }
      if (e.classList.contains("slide-first")) {
        e.classList.remove("slide-first");
      }
    })
  }

  const addClasses= (swiper) => {
    console.log(swiper.slides)
    if (swiper.realIndex >= 2) {
      swiper.slides[swiper.realIndex - 2].classList.add("slide-before-prev");
    }
    if (swiper.realIndex >= 3) {
      swiper.slides[swiper.realIndex - 3].classList.add("slide-first");
    }
    if (swiper.realIndex < swiper.slides.length - 2) {
      swiper.slides[swiper.realIndex + 2].classList.add("slide-after-next");
    }
    if (swiper.realIndex < swiper.slides.length - 3) {
      swiper.slides[swiper.realIndex + 3].classList.add("slide-last");
    }
  }

</script>

<style scoped>
img {
  object-fit: cover;
}
.swiper {
  height: 50%;
  width: 60vw;
  display: flex;
  justify-content: center;
  
}
.swiper-slide {
  display: flex;
  justify-content: center;
  transition: all 300ms;
  align-items: center;
  transform: scale(0.56);
}
.swiper-slide-active {
  transform: scale(2.62) !important;
}
.swiper-slide-active img {
  aspect-ratio: 670 / 576 !important;
}
.swiper-slide img {
  aspect-ratio: 1 / 1;
}
.swiper-slide-prev {
  transform: scale(0.56) translateX(-125%) !important;
}
.swiper-slide-next {
  transform: scale(0.56) translateX(125%) !important;
}
.slide-before-prev {
  transform: scale(0.56) translateX(-50%) !important;  
}
.slide-after-next {
  transform: scale(0.56) translateX(50%) !important;  
}
.slide-first {
  transform: scale(0.56) translateX(25%) !important;  
}
.slide-last {
  transform: scale(0.56) translateX(-25%) !important;  
}
</style>