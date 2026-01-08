<template>
<div class="favorites">
<header class="header">
<h1 class="title">我的收藏</h1>
<p class="subtitle">收藏的美食，随时查看</p>
</header>
<div class="content">
<div class="nav-bar">
<router-link to="/" class="nav-link">
所有菜谱
</router-link>
<router-link to="/favorites" class="nav-link active">
我的收藏
</router-link>
</div>
<div v-if="favoriteRecipes.length > 0" class="favorites-container">
<div class="favorites-grid">
<RecipeCard
v-for="recipe in favoriteRecipes"
:key="recipe.id"
:recipe="recipe"
@favorite-changed="loadFavorites"
/>
</div>
<div class="favorites-stats">
已收藏 {{ favoriteRecipes.length }} 个菜谱
<button @click="clearAllFavorites" class="clear-all-btn">
清空收藏夹
</button>
</div>
</div>
<div v-else class="empty-favorites">
<div class="empty-icon">❤️</div>
<h3>收藏夹空空如也</h3>
<p>您还没有收藏任何菜谱</p>
<router-link to="/" class="browse-btn">
去发现美食 →
</router-link>
</div>
</div>
</div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import RecipeCard from '@/components/RecipeCard.vue'
import recipesData from '@/assets/recipes.json'

const favoriteRecipes = ref([])

// 加载收藏的菜谱
const loadFavorites = () => {
const favorites = JSON.parse(localStorage.getItem('recipeFavorites') || '[]')
favoriteRecipes.value = recipesData.recipes.filter(recipe =>
favorites.includes(recipe.id)
)
}

// 清空所有收藏
const clearAllFavorites = () => {
if (confirm('确定要清空所有收藏吗？')) {
localStorage.setItem('recipeFavorites', JSON.stringify([]))
loadFavorites()
}
}

onMounted(() => {
loadFavorites()
})
</script>

<style scoped>
.favorites {
max-width: 1200px;
margin: 0 auto;
padding: 20px;
}

.header {
text-align: center;
margin-bottom: 40px;
padding-top: 20px;
}

.title {
font-size: 2.8rem;
color: #333;
margin-bottom: 10px;
background: linear-gradient(135deg, #ff4081 0%, #d81b60 100%);
-webkit-background-clip: text;
-webkit-text-fill-color: transparent;
}

.subtitle {
font-size: 1.2rem;
color: #666;
}

.content {
background: white;
border-radius: 16px;
padding: 30px;
box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
}

.nav-bar {
display: flex;
gap: 10px;
margin-bottom: 30px;
padding-bottom: 20px;
border-bottom: 2px solid #f0f0f0;
}

.nav-link {
padding: 12px 24px;
text-decoration: none;
color: #666;
font-weight: 600;
border-radius: 8px;
transition: all 0.3s ease;
}

.nav-link:hover {
background: #f5f5f5;
color: #333;
}

.nav-link.active {
background: #ff4081;
color: white;
}

.favorites-container {
min-height: 300px;
}

.favorites-grid {
display: grid;
grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
gap: 25px;
margin-bottom: 40px;
}

.favorites-stats {
display: flex;
justify-content: space-between;
align-items: center;
padding: 20px;
background: #f8f9fa;
border-radius: 12px;
color: #666;
font-size: 1.1rem;
font-weight: 500;
}

.clear-all-btn {
background: #ff4081;
color: white;
border: none;
padding: 10px 20px;
border-radius: 6px;
font-size: 0.9rem;
font-weight: 600;
cursor: pointer;
transition: all 0.3s ease;
}

.clear-all-btn:hover {
background: #d81b60;
transform: translateY(-2px);
box-shadow: 0 4px 12px rgba(255, 64, 129, 0.3);
}

.empty-favorites {
text-align: center;
padding: 80px 20px;
}

.empty-icon {
font-size: 4rem;
margin-bottom: 20px;
}

.empty-favorites h3 {
font-size: 1.8rem;
color: #333;
margin-bottom: 10px;
}

.empty-favorites p {
color: #666;
margin-bottom: 30px;
font-size: 1.1rem;
}

.browse-btn {
display: inline-block;
background: #4caf50;
color: white;
text-decoration: none;
padding: 15px 30px;
border-radius: 8px;
font-size: 1.1rem;
font-weight: 600;
transition: all 0.3s ease;
}

.browse-btn:hover {
background: #388e3c;
transform: translateY(-2px);
box-shadow: 0 4px 15px rgba(76, 175, 80, 0.3);
}

@media (max-width: 768px) {
.favorites-stats {
flex-direction: column;
gap: 15px;
text-align: center;
}

.favorites-grid {
grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
}
}

@media (max-width: 480px) {
.favorites {
padding: 10px;
}

.content {
padding: 20px;
}

.favorites-grid {
grid-template-columns: 1fr;
}
}
</style>