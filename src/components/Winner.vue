<template>
    <div class="winner">
        <div class="winnercontent">
            <h1>{{ winner }} Won !</h1>
            <h2>Player 1: {{ player1score }} - Player 2 : {{player2score}}</h2>
        </div>
        <div class="quitreplaycontainer">
            <button id="continuer" @click="$emit('continuer')">Continue</button>
            <button id="replay" @click="$emit('replay')">Replay</button>
            <button id="quit" @click="$emit('quitter')">Quit</button>
			<a id="mainhomepage" href="https://doriaarnaud.online">Go to Homepage</a>
        </div>
    </div>
</template>
<script>
export default 
{
    name:'Winner',
    props:
    {
        'winner':String,
        'player1score':Number,
        'player2score':Number
    }
}
</script>
<style>
.winner
{
    border:1px solid black;
    width:70%;
    height:450px;
    margin-top:-450px;
    margin-left:auto;
    margin-right:auto;
    text-align:center;
    position:relative;
    z-index:9999999999999999;
    background-color:#405840;
    border-radius:10px;
    border:none;
    padding-top:30px;
    color:#ffffff;
}
.winner h1
{
    margin-top:25px;
    padding-bottom: 50px;
}
.winner h2
{
    margin-bottom:50px;
}
.quitreplaycontainer
{
    margin-left:auto;
    margin-right:auto;
    width:70%;
    align-content: center;
    justify-content:space-between;
}
button#continuer
{
    width:150px;
    height:30px;
    border:none;
    border-radius: 30px;
    font-size:1.1em;
    font-family: arial;
    margin-top:20px;
    margin:20px;
}
button#continuer:hover
{
    cursor:pointer;
}
button#replay
{
    width:150px;
    height:30px;
    border:none;
    border-radius: 30px;
    font-size:1.1em;
    font-family: arial;
    margin-top:20px;
    margin:20px;
}
button#replay:hover
{
    cursor:pointer;
}
button#quit
{
    width:150px;
    height:30px;
    border:none;
    border-radius: 30px;
    font-size:1.1em;
    font-family: arial;
    margin-top:20px;
    margin:20px;
}
button#quit:hover
{
    cursor:pointer;
}
button#stat
{
    width:150px;
    height:30px;
    border:none;
    border-radius: 30px;
    font-size:1.1em;
    font-family: arial;
    margin-top:20px;
    margin:20px;
}
button#stat:hover
{
    cursor:pointer;
}
#mainhomepage
{
	display:block;
	font-size:1.7em;
	color:#ffffff;
	text-align: center;
}
a#mainhomepage
{
	text-decoration:none;
}
</style>