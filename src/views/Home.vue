<template>
<div class="home">
<header class="header">
<h1 class="title">食谱收藏夹</h1>
<p class="subtitle">发现美食，收藏美味</p>
</header>
<div class="search-container">
<input
v-model="searchQuery"
type="text"
placeholder="搜索食材，例如：鸡蛋、生菜..."
class="search-input"
/>
<div class="search-icon">🔍</div>
</div>
<div class="content">
<div class="nav-bar">
<router-link to="/" class="nav-link" :class="{ active: activeTab === 'all' }">
所有菜谱
</router-link>
<router-link to="/favorites" class="nav-link" :class="{ active: activeTab === 'favorites' }">
我的收藏
</router-link>
</div>
<div v-if="filteredRecipes.length > 0" class="recipes-grid">
<RecipeCard
v-for="recipe in filteredRecipes"
:key="recipe.id"
:recipe="recipe"
@favorite-changed="handleFavoriteChanged"
/>
</div>
<div v-else class="empty-state">
<div class="empty-icon">🍳</div>
<h3>没有找到相关菜谱</h3>
<p>尝试搜索其他食材，或浏览所有菜谱</p>
<button @click="clearSearch" class="clear-btn">清除搜索</button>
</div>
<div class="stats">
共 {{ filteredRecipes.length }} 个菜谱
<span v-if="searchQuery">（搜索"{{ searchQuery }}"）</span>
</div>
</div>
</div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { useRoute } from 'vue-router'
import RecipeCard from '@/components/RecipeCard.vue'
import recipesData from '@/assets/recipes.json'

const route = useRoute()
const searchQuery = ref('')
const recipes = ref([])

// 加载菜谱数据
onMounted(() => {
recipes.value = recipesData.recipes
})

// 处理收藏状态变化
const handleFavoriteChanged = () => {
// 重新触发计算属性更新
}

// 清除搜索
const clearSearch = () => {
searchQuery.value = ''
}

// 当前激活的标签页
const activeTab = computed(() => {
return route.path === '/favorites' ? 'favorites' : 'all'
})

// 过滤菜谱
const filteredRecipes = computed(() => {
let result = recipes.value

// 如果在收藏页面，只显示收藏的菜谱
if (activeTab.value === 'favorites') {
const favorites = JSON.parse(localStorage.getItem('recipeFavorites') || '[]')
result = result.filter(recipe => favorites.includes(recipe.id))
}

// 搜索过滤
if (searchQuery.value.trim()) {
const query = searchQuery.value.trim().toLowerCase()
result = result.filter(recipe => {
return recipe.ingredients.some(ingredient =>
ingredient.toLowerCase().includes(query)
)
})
}

return result
})
</script>

<style scoped>
.home {
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
background: linear-gradient(135deg, #4caf50 0%, #2e7d32 100%);
-webkit-background-clip: text;
-webkit-text-fill-color: transparent;
}

.subtitle {
font-size: 1.2rem;
color: #666;
}

.search-container {
position: relative;
max-width: 600px;
margin: 0 auto 40px;
}

.search-input {
width: 100%;
padding: 15px 20px 15px 50px;
font-size: 1.1rem;
border: 2px solid #e0e0e0;
border-radius: 50px;
outline: none;
transition: all 0.3s ease;
}

.search-input:focus {
border-color: #4caf50;
box-shadow: 0 4px 15px rgba(76, 175, 80, 0.2);
}

.search-icon {
position: absolute;
left: 20px;
top: 50%;
transform: translateY(-50%);
font-size: 1.2rem;
color: #888;
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
background: #4caf50;
color: white;
}

.recipes-grid {
display: grid;
grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
gap: 25px;
margin-bottom: 40px;
}

.empty-state {
text-align: center;
padding: 60px 20px;
}

.empty-icon {
font-size: 4rem;
margin-bottom: 20px;
}

.empty-state h3 {
font-size: 1.5rem;
color: #333;
margin-bottom: 10px;
}

.empty-state p {
color: #666;
margin-bottom: 25px;
font-size: 1.1rem;
}

.clear-btn {
background: #4caf50;
color: white;
border: none;
padding: 12px 30px;
border-radius: 8px;
font-size: 1rem;
font-weight: 600;
cursor: pointer;
transition: all 0.3s ease;
}

.clear-btn:hover {
background: #388e3c;
transform: translateY(-2px);
box-shadow: 0 4px 12px rgba(76, 175, 80, 0.3);
}

.stats {
text-align: center;
padding: 20px;
color: #666;
font-size: 1.1rem;
border-top: 1px solid #f0f0f0;
}

@media (max-width: 768px) {
.title {
font-size: 2.2rem;
}

.recipes-grid {
grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
gap: 20px;
}
}

@media (max-width: 480px) {
.home {
padding: 10px;
}

.content {
padding: 20px;
}

.recipes-grid {
grid-template-columns: 1fr;
}
}
</style>