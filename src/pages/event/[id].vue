<template>
  <div class="container">
    <button class="back-button" @click="navigateTo('/')">Назад к списку</button>
    <div v-if="pending" class="loading">Загрузка...</div>
    <div v-else-if="error" class="error">Произошла ошибка при загрузке данных</div>
    <div v-else-if="event" class="event-detail">
      <img 
        :src="event.activity?.image?.uploadURL" 
        :alt="event.activity.title" 
        class="event-image"
      />
      <div class="event-content">
        <h1>{{ event.activity.title }}</h1>
        <div class="event-meta">
          <span class="price" v-if="event.minPrice">От {{ event.minPrice }} ₽</span>
          <span class="date">{{ formatDate(event.scheduleInfo.sessionDate) }}</span>
        </div>
        <div class="event-description" v-html="event.activity.description"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
const route = useRoute()

const formatDate = (dateString) => {
  const options = { 
    year: 'numeric', 
    month: 'long', 
    day: 'numeric',
    hour: '2-digit',
    minute: '2-digit'
  }
  return new Date(dateString).toLocaleDateString('ru-RU', options)
}

const { data: event, pending, error } = await useFetch(`https://api.ticketme.biz/api/v1/event/${route.params.id}`)
</script>

<style scoped>
.container {
    max-width: 1000px;
    margin: 0 auto;
    padding: 20px;
}

.back-button {
    padding: 12px 24px;
    background: linear-gradient(90deg, #00d2ff 0%, #3a7bd5 100%);
    color: white;
    border: none;
    border-radius: 25px;
    cursor: pointer;
    font-weight: 500;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    margin-bottom: 20px;
}

.back-button:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
}

.event-detail {
    background: white;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.event-image {
    width: 100%;
    height: 400px;
    object-fit: cover;
    border-bottom: 4px solid;
    border-image: linear-gradient(90deg, #00d2ff 0%, #3a7bd5 100%) 1;
}

.event-content {
    padding: 30px;
}

.event-content h1 {
    margin: 0 0 20px 0;
    color: #2d3436;
    font-size: 2rem;
    line-height: 1.3;
}

.event-meta {
    display: flex;
    gap: 20px;
    margin-bottom: 25px;
    padding-bottom: 15px;
    border-bottom: 1px solid #eee;
}

.price {
    font-size: 1.2rem;
    font-weight: bold;
    color: #00d2ff;
}

.date {
    font-size: 1.2rem;
    color: #636e72;
}

.event-description {
    color: #636e72;
    line-height: 1.6;
    font-size: 1.1rem;
}

/* Style for HTML content from API */
.event-description :deep(p) {
    margin-bottom: 1rem;
}

.event-description :deep(h2) {
    color: #2d3436;
    margin: 1.5rem 0 1rem;
    font-size: 1.4rem;
}

.event-description :deep(ul),
.event-description :deep(ol) {
    margin: 1rem 0;
    padding-left: 2rem;
}

.event-description :deep(li) {
    margin-bottom: 0.5rem;
}

.event-description :deep(a) {
    color: #3a7bd5;
    text-decoration: none;
}

.event-description :deep(a:hover) {
    text-decoration: underline;
}

/* Responsive adjustments */
@media (max-width: 768px) {
    .event-image {
        height: 300px;
    }

    .event-content {
        padding: 20px;
    }

    .event-content h1 {
        font-size: 1.6rem;
    }

    .event-meta {
        flex-direction: column;
        gap: 10px;
    }

    .price, .date {
        font-size: 1rem;
    }
}

.loading, .error {
    text-align: center;
    padding: 2rem;
    color: #636e72;
    font-size: 1.2rem;
}

.error {
    color: #d63031;
}
</style>
