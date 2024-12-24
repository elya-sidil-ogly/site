<template>
    <div class="container">
        <h1 class="page-title">Мероприятия</h1>
        <div v-if="error" class="error">Произошла ошибка при загрузке данных</div>

        <div class="posts-grid">
            <div v-for="event in events" :key="event.id" class="post-card">
                <img :src="event.activity.image?.uploadURL" :alt="event.title" class="post-image" />
                <div class="post-content">
                    <h2 class="post-title">{{ event.activity.title }}</h2>
                    <p class="post-snippet">{{ event.activity.snippet }}</p>
                    <div class="post-meta">
                        <div class="price" v-if="event.minPrice">От {{ event.minPrice }} ₽</div>
                        <NuxtLink :to="`/event/${event.id}`" class="read-more">
                            Подробнее
                        </NuxtLink>
                    </div>
                </div>
            </div>
        </div>

        <!-- Loading indicator -->
        <div v-if="pending" class="loading">Загрузка...</div>
    </div>
</template>

<script setup>
const LIMIT = 12

// Fetch events using useAsyncData
// Define query parameters
const queryParams = {
    'scheduleInfo.saleOpening_lte': new Date().toISOString(),
    'scheduleInfo.saleEnding_gte': new Date().toISOString(),
    '_sort': 'scheduleInfo.presentationDate:asc',
    '_limit': LIMIT,
    '_skip': 0
}

const { data: events, pending, error } = await useAsyncData(
    'events',
    () => $fetch('https://api.ticketme.biz/api/v1/event/get-the-nearest-events-of-activities-available-for-online?scheduleInfo.saleOpening_lte=2024-12-24T16:50:57.546Z&scheduleInfo.saleEnding_gte=2024-12-24T16:50:57.546Z&_sort=scheduleInfo.presentationDate:asc&_limit=6&_skip=0', {
        params: queryParams
    })
)
</script>

<style scoped>
.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
}

.page-title {
    text-align: center;
    color: #2d3436;
    font-size: 2.5rem;
    margin: 40px 0;
    font-weight: 700;
    position: relative;
    padding-bottom: 15px;
}

.page-title::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 100px;
    height: 4px;
    background: linear-gradient(90deg, #00d2ff 0%, #3a7bd5 100%);
    border-radius: 2px;
}

.posts-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 25px;
    padding: 20px;
}

.post-card {
    background: white;
    border-radius: 12px;
    transition: all 0.3s ease;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    overflow: hidden;
    text-decoration: none;
    display: flex;
    flex-direction: column;
}

.post-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 15px rgba(0, 0, 0, 0.1);
}

.post-image {
    width: 100%;
    height: 200px;
    object-fit: cover;
    border-bottom: 4px solid;
    border-image: linear-gradient(90deg, #00d2ff 0%, #3a7bd5 100%) 1;
}

.post-content {
    padding: 20px;
    flex: 1;
    display: flex;
    flex-direction: column;
}

.post-card h2 {
    margin: 0 0 15px 0;
    color: #2d3436;
    font-size: 1.2rem;
    font-weight: 600;
    line-height: 1.4;
}

.post-card p {
    color: #636e72;
    margin: 0;
    flex-grow: 1;
}

.post-meta {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 15px;
}

.post-meta a {
    margin-left: auto;
}

.read-more {
    color: #3a7bd5;
    font-weight: 500;
    font-size: 0.9rem;
    transition: color 0.3s ease;
}

.post-card:hover .read-more {
    color: #00d2ff;
}

.loading,
.error {
    text-align: center;
    padding: 2rem;
    color: #636e72;
    font-size: 1.2rem;
}

.error {
    color: #d63031;
}
</style>
