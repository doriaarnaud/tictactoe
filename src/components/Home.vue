<template>
<div class="mainboard">
    <div class="gameboardcontainer">
        <div class="gameboardcontent">
            <Gameboard>

            </Gameboard>
        </div>
    </div>
</div>

</template>
<script>
import Gameboard from '@/components/Gameboard.vue'
export default 
{
    name:'Home',
    components:
    {
        Gameboard
    }
}
</script>
<style>
.mainboard
{
    border:1px solid rgb(240, 229, 229);
    width:99%;
    height: 900px;
    margin-left: auto;
    margin-right: auto;
    border-radius: 5px;
    box-shadow: 5px 5px 5px 5px rgb(240, 229, 229);
}
.gameboardcontainer
{
    margin-top:200px;
    padding-top:50px;
    height:600px;
    background-color:#4fb14f;
}
.gamereset
{
    width:99%;
    height: 900px;
    margin-left: auto;
    margin-right: auto;
}
</style>