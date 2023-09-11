<template>
    <div class="score">
        <div class="scorecontent">
            <h2 id="player1">Player 1 : {{ player1score }}</h2>
            <h2 id="player2">{{player2}} : {{ player2score }}</h2>
        </div>
    </div>
</template>
<script>
export default 
{
    name:'Score',
    props:
    {
        'player1score':Number,
        'player2':String,
        'player2score':Number
    }
}
</script>
<style>
.scorecontent
{
    display:flex;
    justify-content: space-between;
    margin-top:30px;
    font-family: 'Courier New', Courier, monospace;
    color:#ffffff;
    font-size:1.1em;
}
h2#player1
{
    margin-left:10px;
}
h2#player2
{
    margin-right:10px;
}
@media (max-width: 575px)
{
    .score
    {
        text-align: center;
    }
}
</style>