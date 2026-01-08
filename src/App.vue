<template>
<div id="app">
<router-view />
</div>
</template>

<script>
export default {
name: 'App'
}
</script>

<style>
* {
margin: 0;
padding: 0;
box-sizing: border-box;
}

body {
font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
sans-serif;
background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
min-height: 100vh;
color: #333;
}

#app {
min-height: 100vh;
}

button {
font-family: inherit;
}

a {
text-decoration: none;
color: inherit;
}

/* 滚动条样式 */
::-webkit-scrollbar {
width: 8px;
}

::-webkit-scrollbar-track {
background: #f1f1f1;
border-radius: 4px;
}

::-webkit-scrollbar-thumb {
background: #888;
border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
background: #555;
}
</style>