<template>
    <div class="buttoncontainer">
        <xbutton :class="{activeplayer1:isactiveplayer1}">

        </xbutton>
        <select name="player1" id="player1select">
            <option selected value="player1">
                Player 1
            </option>
        </select>
        <select name="player2" id="player2select" v-model="player2select" @change="playerselection">
            <option disabled value="choose">
                Choose...
            </option>
            <option selected value="Player 2">
                Player 2
            </option>
            <option value="CPU">
                CPU
            </option>
        </select>
        <obutton :class="{activeplayer2:isactiveplayer2}">

        </obutton>
    </div>
    <div class="menu">
        <GameMenu @showmenu="showmenu">
            
        </GameMenu>
    </div>
    <div class="labelcontainer" :class="[{original:isoriginal},{opaque:isopaque}]">
        <Score :player1score="player1score" :player2score="player2score" :player2="player2select">

        </Score>
        <table>
            <tr>
                <td id="cell0" @click="fillcell(0)"></td>
                <td id="cell1" @click="fillcell(1)"></td>
                <td id="cell2" @click="fillcell(2)"></td>
            </tr>
            <tr>
                <td id="cell3" @click="fillcell(3)"></td>
                <td id="cell4" @click="fillcell(4)"></td>
                <td id="cell5" @click="fillcell(5)"></td>

            </tr>
            <tr>
                <td id="cell6" @click="fillcell(6)"></td>
                <td id="cell7" @click="fillcell(7)"></td>
                <td id="cell8" @click="fillcell(8)"></td>
            </tr>
        </table>
    </div>
    <div class="winnercontent" v-if="winnershow">
        <Winner @continuer="continuer" @quit="quitter" @replay="replay" @quitter="quitter" 
            :winner="winnerplayer"
            :player1score="player1score"
            :player2score="player2score">
        </Winner>
    </div>   
    <div class="gameloader">
        <GameLoader :width="gameloaderwidth" v-if="isgameloading">

        </GameLoader>
    </div>
    <div class="gamedrawer">
        <GameDrawer v-if="isgamedrawing">

        </GameDrawer>
    </div>
</template>

<script>
import xbutton from '@/components/xbutton.vue'
import obutton from '@/components/obutton.vue'
import Winner from '@/components/Winner.vue'
import GameLoader from '@/components/GameLoader.vue'
import GameDrawer from '@/components/GameDrawer.vue'
import GameMenu from '@/components/GameMenu.vue'
import Score from '@/components/Score.vue'
let player2select='';
let activeplayer='X';
let clickcounter=0;
let winnerplayer='No';
let winnershow=false;
let player1score=0;
let player2score=0;
let gameloaderwidth=0;
let isgameloading=true;
let loadtimer=null;
let isactiveplayer1=false;
let isactiveplayer2=false;
let isgamedrawing=false;
let cells=[null,null,null,null,null,null,null,null,null];
let winner=null;
let player1position=0;
let ifwin=true;
let isoriginal=false;
let isopaque=false;
let winingconditions=
[
    [0,1,2],
    [3,4,5],
    [6,7,8],
    [0,3,6],
    [1,4,7],
    [2,5,8],
    [0,4,8],
    [2,4,6]
]
export default 
{
    name:'Gameboard',
    components:
    {
        xbutton,obutton,Winner,GameLoader,GameDrawer,Score,GameMenu
    },
    data()
    {
        return {
            player2select,cells,activeplayer,winingconditions,clickcounter,winner,ifwin,
            winnerplayer,winnershow,player1score,player2score,
            isactiveplayer1,isactiveplayer2,isgamedrawing,
            gameloaderwidth,isgameloading,loadtimer,
            isoriginal,isopaque,player1position
        }
    },
    mounted:function()
    {
        this.isactiveplayer1=!this.isactiveplayer1;
        this.startloading();
        this.isopaque=true;
        this.isoriginal=false;
        if(this.player2select==='')
        {
            this.player2select='Choose the player 2!';
        }
    },
    methods:
    {
        startloading:function()
        {
            this.loadtimer=setInterval(this.nextloading,10);
        },
        nextloading:function()
        {
            this.gameloaderwidth+=10;
            if(this.width==100)
            {
                this.width=0;
            }
            setTimeout(()=>
            {
                this.isgameloading=false;
                this.isopaque=false;
                this.isoriginal=true;
            },300);
        },
        playerselection:function()
        {
            if(this.player2select==='CPU')
            {
				this.player2select='This mode is under construction';
                this.activeplayer='X';
                this.isactiveplayer1=true;
                this.isactiveplayer2=false;
                this.replay();
            }
            else if(this.player2select==='Player 2')
            {
                this.activeplayer='X';
                this.isactiveplayer1=true;
                this.isactiveplayer2=false;
                this.replay();
            }
        },
        fillcell:function(id)
        {
            if(this.player2select==='Player 2')
            {
                let cellname="cell"+id;
                let cell=document.getElementById(cellname);
                this.player1position=id;
                if(this.cells[id]==null)
                {
                    this.clickcounter+=1;
                    if(this.activeplayer==='X')
                    {
                        cell.innerHTML='X';
                    }
                    else
                    {
                        cell.innerHTML='O';
                    }
                    this.cells[id]=this.activeplayer;
                    this.activeplayer=(this.activeplayer==='X') ? 'O':'X';
                    this.isactiveplayer1=!this.isactiveplayer1;
                    this.isactiveplayer2=!this.isactiveplayer2;
                    if(this.clickcounter>=5)
                    {
                        this.checkgame();
                    }
                }
            }
            // else if(this.player2select==='CPU')
            // {
            //     this.activeplayer='X';
            //     console.log('CPU select');
            //     let cellname="cell"+id;
            //     let cell=document.getElementById(cellname);
            //     this.player1position=id;
            //     if(this.cells[id]==null)
            //     {
            //         this.clickcounter+=1;
            //         if(this.activeplayer==='X')
            //         {
            //             cell.innerHTML='X';
            //         }
            //     }
            // }
        },
        checkgame:function()
        {
            for(let i=0;i<=7;i++)
            {
                let winingcondition=this.winingconditions[i];
                let a=this.cells[winingcondition[0]];
                let b=this.cells[winingcondition[1]];
                let c=this.cells[winingcondition[2]];
                if(a==null || b==null || c==null)
                    continue;
                if(a==b && b==c)
                {
                    this.winner=a;
                    break;
                }
            }
            if(this.winner)
            {
              if(this.winner==='X')
              {
                this.player1score+=1;
                this.activeplayer='X';
                this.isactiveplayer1=true;
                this.isactiveplayer2=false;
              }
              else if(this.winner==='O')
              {
                this.player2score+=1;
                this.activeplayer='O';
                this.isactiveplayer1=false;
                this.isactiveplayer2=true;
              }
              this.winnershow=true;
              this.winnerplayer=this.winner;
              this.activeplayer=this.winner;
            }
            else
            {
                if(this.clickcounter==9)
                {
                    setTimeout(() => {
                        this.isgamedrawing=true;
                    }, 500);
                    setTimeout(() => {
                        this.isgamedrawing=false;
                        let cellule0=document.getElementById("cell0");
                        let cellule1=document.getElementById("cell1");
                        let cellule2=document.getElementById("cell2");
                        let cellule3=document.getElementById("cell3");
                        let cellule4=document.getElementById("cell4");
                        let cellule5=document.getElementById("cell5");
                        let cellule6=document.getElementById("cell6");
                        let cellule7=document.getElementById("cell7");
                        let cellule8=document.getElementById("cell8");
                        let tabcell=[cellule0,cellule1,cellule2,cellule3,cellule4,cellule5,cellule6,cellule7,cellule8];
                        for(let i=0;i<tabcell.length;i++)
                            {
                                tabcell[i].innerHTML=' ';
                                this.cells[i]=null;
                            }
                        this.winner=null;
                        this.clickcounter=0;
                    },2000);
                }
            }
        },
        continuer:function()
        {
            if(this.clickcounter>=5 && (this.winner==='X' || this.winner==='O'))
            {
                let cellule0=document.getElementById("cell0");
                let cellule1=document.getElementById("cell1");
                let cellule2=document.getElementById("cell2");
                let cellule3=document.getElementById("cell3");
                let cellule4=document.getElementById("cell4");
                let cellule5=document.getElementById("cell5");
                let cellule6=document.getElementById("cell6");
                let cellule7=document.getElementById("cell7");
                let cellule8=document.getElementById("cell8");
                let tabcell=[cellule0,cellule1,cellule2,cellule3,cellule4,cellule5,cellule6,cellule7,cellule8];
                for(let i=0;i<tabcell.length;i++)
                {
                    tabcell[i].innerHTML=' ';
                    this.cells[i]=null;
                }
                this.clickcounter=0;
                this.winner=null;
                this.activeplayer=this.winnerplayer;
                this.winnershow=false;
            }
            else
            {
                this.winnershow=false;
            }
            
        },
        replay:function()
        {
            setTimeout(() => {
                this.isgamedrawing=true;
                this.winnershow=false;
            }, 200);
            setTimeout(() => {
                this.isgamedrawing=false;
                let cellule0=document.getElementById("cell0");
                let cellule1=document.getElementById("cell1");
                let cellule2=document.getElementById("cell2");
                let cellule3=document.getElementById("cell3");
                let cellule4=document.getElementById("cell4");
                let cellule5=document.getElementById("cell5");
                let cellule6=document.getElementById("cell6");
                let cellule7=document.getElementById("cell7");
                let cellule8=document.getElementById("cell8");
                let tabcell=[cellule0,cellule1,cellule2,cellule3,cellule4,cellule5,cellule6,cellule7,cellule8];
                for(let i=0;i<tabcell.length;i++)
                {
                    tabcell[i].innerHTML=' ';
                    this.cells[i]=null;
                }
                this.clickcounter=0;
                this.winner=null;
                this.activeplayer=this.winnerplayer;
                this.winnershow=false;
                this.activeplayer='X';
                this.isactiveplayer1=true;
                this.isactiveplayer2=false;
                this.player1score=0;
                this.player2score=0;
                this.winnerplayer='No';
            }, 1500);
            
        },
        showmenu:function()
        {
            this.winnershow=!this.winnershow;
        },
        cpucommand:function(id)
        {
            let cellule0=document.getElementById("cell0");
            let cellule1=document.getElementById("cell1");
            let cellule2=document.getElementById("cell2");
            let cellule3=document.getElementById("cell3");
            let cellule4=document.getElementById("cell4");
            let cellule5=document.getElementById("cell5");
            let cellule6=document.getElementById("cell6");
            let cellule7=document.getElementById("cell7");
            let cellule8=document.getElementById("cell8");
            setTimeout(() => 
            {

            },1000);
        },    
        quitter:function()
        {

        }
    }
}
</script>
<style>
.buttoncontainer
{
    width:60%;
    margin-left:auto;
    margin-right:auto;
    padding-left:20px;
    padding-right:20px;
    background-image: linear-gradient(#4fb14f,#bcecbc);
    border-radius:10px;
    height: 100px;
    margin-top:-200px;
    display:flex;
    align-content: center;
    justify-content: space-between;
}
select#player1select
{
    width:100px;
    height: 30px;
    margin-top:30px;
    font-size:1.1em;
}
select#player2select
{
    width:100px;
    height: 30px;
    margin-top:30px;
    font-size:1.1em;
}
.activeplayer1
{
    border:none;
    border-bottom:10px solid #000000;
    color:#000000;
}
.activeplayer2
{
    border:none;
    border-bottom:10px solid #000000;
    color:#000000;
}
.menu
{
    width:100%;
    margin-left:auto;
    margin-right:auto;
    margin-top:5px;
}
.original
{
    opacity:1;
}
.opaque
{
    opacity:0.3;
}
table
{
    border-collapse: collapse;
    margin-left:auto;
    margin-right:auto;
    margin-top:100px;
}
td
{
    border:4px solid rgb(255, 255, 255);
    width:150px;
    height: 130px;
    font-size: 5em;
    font-family:arial;
    text-align:center;
    padding:0;
}
td.cell0win
{
    color:#00ff00;
}
td:hover
{
    cursor:pointer;
}
td#cell0
{
    border-top:none;
    border-left:none;
}
td#cell1
{
    border-top:none;
}
td#cell2
{
    border-top:none;
    border-right:none;
}
td#cell3
{
    border-left:none;
}
td#cell6
{
    border-left:none;
    border-bottom: none;
}
td#cell5
{
    border-right: none;
}
td#cell8
{
    border-right:none;
    border-bottom: none;
}
td#cell7
{
    border-bottom:none;
}
.winnercontent
{
    width:99%;
    margin-left:auto;
    margin-right:auto;
}
.gamedrawer
{
    margin-top:-500px;
}
@media (max-width: 575px)
{
    td
    {
        font-size:4em;
    }
}
</style>