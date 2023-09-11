<template>
    <button id="menu" @click="$emit('showmenu')">Menu</button>
</template>        
<script>
export default 
{
    name:'GameMenu'
}
</script>
<style>
button#menu
{
    position:relative;
    width:70px;
    height: 40px;
    border:none;
    border-radius: 10px;
    background-color: #4fb14f;
    font-size: 1.5em;
    color:#ffffff;
    font-family: arial;
}
button#menu:hover
{
    cursor:pointer;
}
</style>