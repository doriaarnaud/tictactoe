<template>
    <div class="drawer">
        <div class="drawercontent">
           <h1>Game is drawing...</h1>
        </div>
    </div>
</template>
<script>
export default 
{
   name:'GameDrawer'
}
</script>
<style>
.drawer
{
    border:1px solid black;
    width:80%;
    height:100px;
    margin-left:auto;
    margin-right:auto;
    text-align:center;
    position:relative;
    z-index:9999999999999999;
    background-color:#405840;
    border-radius:10px;
    border:none;
    padding:0;
    padding-top:20px;
    color:#ffffff;
}
@media (max-width: 575px)
{
    .drawer
    {
        border:1px solid black;
        width:80%;
        height:70px;
        margin-left:auto;
        margin-right:auto;
        text-align:center;
        position:relative;
        z-index:9999999999999999;
        background-color:#405840;
        border-radius:10px;
        border:none;
        padding:0;
        color:#ffffff;
    }
}
</style>