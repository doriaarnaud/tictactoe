<template>
    <div class="loader" v-if="isgameloading">
        <div class="loadercontent">
            <div class="loaderline" :style="{width:width+'px'}">

            </div>
           <h1>Loading...</h1>
        </div>
    </div>
</template>
<script>
let width=0;
let timer=null;
let isgameloading=false;
export default 
{
   name:'GameLoader',
   data()
   {
    return {width,timer,isgameloading}
   },
   mounted:function()
   {
        this.isgameloading=true;
        this.startloading();
   },
   methods:
   {
    startloading:function()
    {   
        this.timer=setInterval(this.next,20);
    },
    next:function()
    {
        this.width+=10;
        if(this.width==500)
        {
            this.width=0;
        }
        setTimeout(()=>
        {
            this.isgameloading=false;
        },1000);
    }
   }
}
</script>
<style>
.loader
{
    border:1px solid black;
    width:70%;
    height:100px;
    margin-top:-400px;
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
.loaderline
{
    height:5px;
    margin-left:0px;
    background-color: aqua;
}
</style>