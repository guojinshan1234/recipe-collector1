<template>
<div class="recipe-detail">
<button class="back-btn" @click="goBack">
← 返回
</button>
<div v-if="recipe" class="detail-container">
<div class="detail-header">
<div class="detail-image">
<img :src="recipe.image" :alt="recipe.name">
</div>
<div class="detail-info">
<h1 class="recipe-title">{{ recipe.name }}</h1>
<p class="recipe-description">{{ recipe.description }}</p>
<button
class="detail-favorite-btn"
@click="toggleFavorite"
:class="{ 'favorited': isFavorited }"
>
{{ isFavorited ? '★ 已收藏' : '☆ 加入收藏' }}
</button>
</div>
</div>
<div class="detail-content">
<div class="ingredients-section">
<h2>食材清单</h2>
<ul class="ingredients-list">
<li v-for="(ingredient, index) in recipe.ingredients" :key="index">
{{ ingredient }}
</li>
</ul>
</div>
<div class="steps-section">
<h2>制作步骤</h2>
<ol class="steps-list">
<li v-for="(step, index) in recipe.steps" :key="index">
{{ step }}
</li>
</ol>
</div>
</div>
</div>
<div v-else class="loading">
加载中...
</div>
</div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import recipesData from '@/assets/recipes.json'

const router = useRouter()
const route = useRoute()
const recipe = ref(null)
const favorites = ref([])

// 加载收藏状态
const loadFavorites = () => {
const saved = localStorage.getItem('recipeFavorites')
favorites.value = saved ? JSON.parse(saved) : []
}

// 检查当前菜谱是否已收藏
const isFavorited = computed(() => {
return favorites.value.includes(parseInt(route.params.id))
})

// 切换收藏状态
const toggleFavorite = () => {
loadFavorites()
const recipeId = parseInt(route.params.id)

if (isFavorited.value) {
// 取消收藏
favorites.value = favorites.value.filter(id => id !== recipeId)
} else {
// 加入收藏
favorites.value.push(recipeId)
}

// 保存到localStorage
localStorage.setItem('recipeFavorites', JSON.stringify(favorites.value))
}

// 返回上一页
const goBack = () => {
router.back()
}

// 加载菜谱数据
onMounted(() => {
loadFavorites()
const recipeId = parseInt(route.params.id)
recipe.value = recipesData.recipes.find(r => r.id === recipeId)
})
</script>

<style scoped>
.recipe-detail {
max-width: 1200px;
margin: 0 auto;
padding: 20px;
}

.back-btn {
background: none;
border: 2px solid #4caf50;
color: #4caf50;
padding: 10px 20px;
border-radius: 8px;
font-size: 16px;
font-weight: 600;
cursor: pointer;
margin-bottom: 30px;
transition: all 0.3s ease;
}

.back-btn:hover {
background: #4caf50;
color: white;
}

.detail-container {
background: white;
border-radius: 16px;
overflow: hidden;
box-shadow: 0 8px 30px rgba(0, 0, 0, 0.1);
}

.detail-header {
display: flex;
flex-direction: column;
gap: 30px;
padding: 30px;
background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
}

@media (min-width: 768px) {
.detail-header {
flex-direction: row;
}
}

.detail-image {
flex: 1;
min-height: 300px;
border-radius: 12px;
overflow: hidden;
box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

.detail-image img {
width: 100%;
height: 100%;
object-fit: cover;
}

.detail-info {
flex: 1;
padding: 20px;
display: flex;
flex-direction: column;
}

.recipe-title {
font-size: 2.5rem;
color: #333;
margin-bottom: 15px;
}

.recipe-description {
font-size: 1.1rem;
color: #666;
line-height: 1.6;
margin-bottom: 30px;
}

.detail-favorite-btn {
align-self: flex-start;
background: #ff9800;
color: white;
border: none;
padding: 12px 24px;
border-radius: 8px;
font-size: 1rem;
font-weight: 600;
cursor: pointer;
transition: all 0.3s ease;
}

.detail-favorite-btn:hover {
background: #f57c00;
transform: translateY(-2px);
box-shadow: 0 4px 12px rgba(255, 152, 0, 0.3);
}

.detail-favorite-btn.favorited {
background: #4caf50;
}

.detail-favorite-btn.favorited:hover {
background: #388e3c;
box-shadow: 0 4px 12px rgba(76, 175, 80, 0.3);
}

.detail-content {
display: flex;
flex-direction: column;
gap: 40px;
padding: 40px;
}

@media (min-width: 992px) {
.detail-content {
flex-direction: row;
}
}

.ingredients-section,
.steps-section {
flex: 1;
}

h2 {
color: #333;
font-size: 1.8rem;
margin-bottom: 20px;
padding-bottom: 10px;
border-bottom: 2px solid #4caf50;
}

.ingredients-list {
list-style: none;
padding: 0;
}

.ingredients-list li {
padding: 12px 15px;
margin-bottom: 8px;
background: #f8f9fa;
border-radius: 8px;
border-left: 4px solid #4caf50;
font-size: 1.1rem;
color: #444;
}

.steps-list {
padding-left: 20px;
}

.steps-list li {
padding: 15px 0;
font-size: 1.1rem;
line-height: 1.6;
color: #444;
border-bottom: 1px solid #eee;
}

.loading {
text-align: center;
padding: 60px;
font-size: 1.2rem;
color: #666;
}
</style>