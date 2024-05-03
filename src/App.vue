<script setup>
import { onMounted, ref } from 'vue';
import axios from 'axios';

const images = ref([]);
const fullscreenImage = ref(null);

const fetchImages = async () => {
    const response = await axios.get('https://pixabay.com/api/', {
        params: {
            key: '43695687-61cb3f2b52fad28060de3e596',
            image_type: 'photo',
            per_page: 20,
        },
    });
    images.value = response.data.hits;
};

const openImage = (image) => {
    fullscreenImage.value = image;
};

const closeImage = () => {
    fullscreenImage.value = null;
};

onMounted(fetchImages);
</script>

<template>
    <h1 class="title">Vue Gallery</h1>
    <div class="gallery">
        
        <div
            v-for="image in images"
            :key="image.id"
            class="image-container"
            @click="openImage(image)">
            <img :src="image.largeImageURL" :alt="image.tags" class="image" />
            <div class="image-info">
                <p class="image-likes">Likes: {{ image.likes }}</p>
                <p class="image-views">Views: {{ image.views }}</p>
            </div>
        </div>
        <div v-if="fullscreenImage" class="image-fullscreen" @click="closeImage">
            <img :src="fullscreenImage.largeImageURL" :alt="fullscreenImage.tags" />
        </div>
    </div>
</template>

<style scoped>

.title {
    text-align: center;
}
.gallery {
    margin: 0 auto;
    max-width: 1440px;
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 10px;
}

.image-container {
    position: relative;
    height: 150px;
}

.image {
    width: 100%;
    height: 150px;
    object-fit: cover;
    cursor: pointer;
}

.image-likes, .image-views {
  margin: 5px;
}

.image-info {
    position: absolute;
    bottom: 0;
    background: rgba(0, 0, 0, 0.3);
    color: white;
    display: flex;
    justify-content: space-around;
    width: 100%;
}

.image-fullscreen {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.7);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 10;
}

.image-fullscreen img {
    max-width: 90%;
    max-height: 90%;
}
</style>
