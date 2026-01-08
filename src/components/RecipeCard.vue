<template>
<div class="recipe-card" @click="goToDetail">
<div class="recipe-image">
<img :src="recipe.image" :alt="recipe.name">
<button
class="favorite-btn"
@click.stop="toggleFavorite"
:class="{ 'favorited': isFavorited }"
>
{{ isFavorited ? '取消收藏' : '加入收藏' }}
</button>
</div>
<div class="recipe-content">
<h3 class="recipe-title">{{ recipe.name }}</h3>
<p class="recipe-description">{{ recipe.description }}</p>
<div class="recipe-ingredients">
<span class="ingredients-label">食材：</span>
<span class="ingredients-list">{{ recipe.ingredients.join('、') }}</span>
</div>
</div>
</div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { useRouter } from 'vue-router'

const props = defineProps({
recipe: {
type: Object,
required: true
}
})

const router = useRouter()
const favorites = ref([])

// 加载收藏状态
const loadFavorites = () => {
const saved = localStorage.getItem('recipeFavorites')
favorites.value = saved ? JSON.parse(saved) : []
}

// 检查当前菜谱是否已收藏
const isFavorited = computed(() => {
return favorites.value.includes(props.recipe.id)
})

// 切换收藏状态
const toggleFavorite = () => {
loadFavorites()
if (isFavorited.value) {
// 取消收藏
favorites.value = favorites.value.filter(id => id !== props.recipe.id)
} else {
// 加入收藏
favorites.value.push(props.recipe.id)
}
// 保存到localStorage
localStorage.setItem('recipeFavorites', JSON.stringify(favorites.value))
// 触发自定义事件通知父组件
emit('favorite-changed')
}

// 跳转到详情页
const goToDetail = () => {
router.push(`/recipe/${props.recipe.id}`)
}

const emit = defineEmits(['favorite-changed'])

// 组件挂载时加载收藏数据
onMounted(() => {
loadFavorites()
})
</script>

<style scoped>
.recipe-card {
background: white;
border-radius: 12px;
overflow: hidden;
box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
transition: all 0.3s ease;
cursor: pointer;
height: 100%;
display: flex;
flex-direction: column;
}

.recipe-card:hover {
transform: translateY(-5px);
box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
}

.recipe-image {
position: relative;
height: 180px;
overflow: hidden;
}

.recipe-image img {
width: 100%;
height: 100%;
object-fit: cover;
transition: transform 0.3s ease;
}

.recipe-card:hover .recipe-image img {
transform: scale(1.05);
}

.favorite-btn {
position: absolute;
top: 10px;
right: 10px;
background: rgba(255, 255, 255, 0.9);
border: none;
padding: 6px 12px;
border-radius: 20px;
font-size: 12px;
font-weight: 600;
cursor: pointer;
transition: all 0.3s ease;
box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
}

.favorite-btn:hover {
background: white;
transform: scale(1.05);
}

.favorite-btn.favorited {
background: #4caf50;
color: white;
}

.recipe-content {
padding: 20px;
flex-grow: 1;
display: flex;
flex-direction: column;
}

.recipe-title {
margin: 0 0 10px 0;
font-size: 1.3rem;
color: #333;
}

.recipe-description {
color: #666;
font-size: 0.95rem;
line-height: 1.5;
margin-bottom: 15px;
flex-grow: 1;
}

.recipe-ingredients {
padding-top: 10px;
border-top: 1px solid #eee;
font-size: 0.9rem;
}

.ingredients-label {
color: #666;
font-weight: 500;
}

.ingredients-list {
color: #444;
display: -webkit-box;
-webkit-line-clamp: 2;
-webkit-box-orient: vertical;
overflow: hidden;
}
</style>