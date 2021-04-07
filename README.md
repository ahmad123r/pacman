# pacman
pacman full code
<head>
    <title>ninjaman</title>
    <style type="text/css">
    *{
        margin:0;
        padding:0;
    }
    #bigpic{
        visibility: hidden;
    }
    #nub{
        height:50px;
        width:50px;
        position: absolute;
        top :600px;
        left :720px;
    }
    #ninjaman{
        height:40px;
        width:40px;
        display: inline-block;
        background-image:url('images/ninja.gif');
        background-size: contain;
        position:absolute;
        top:40px;
        left:40px

    }
    #ghost{
        height:40px;
        width:40px;
        display: inline-block;
        background-image:url('images/bluey.gif');
        background-size: contain;
        position:absolute;
        top:40px;
        left:720px 
    }
    #heart{
        height:40px;
        width:40px;
        display: inline-block;
        background-image:url('images/heart.png');
        top:720px;
        left:720px
    }
    .sushi{
        height:40px;
        width:40px;
        display: inline-block;
        background-image: url('images/sushi.png');
        background-color: black;
        background-size:contain;
    }
    .wall{
        height:40px;
        width:40px;
        display: inline-block;
        background-color: blue;
    }
    .blank{
        height:40px;
        width:40px;
        display: inline-block;
        background-color: black;
    }
    .onigiri{
        height:40px;
        width:40px;
        display: inline-block;
        background-image: url('images/onigiri.png');
        background-color: black;
        background-size:contain;
    }

    </style>
</head>
<body >
    <div id='world'>
        
    </div>
<script type="text/javascript">
var lives=3;
var score=0;
var z;
var world = [[],[]];
function getRndInteger(min, max) {
  var z=Math.floor(Math.random() * (max - min)) + min;
  for(var i=0;i<1;i++){
      if(z==1){
        z=Math.floor(Math.random() * (max - min)) + min;
      }
  }
  return z
}
for(var i=0;i<15;i++){
    world[i]=[]
    for(var j=0;j<20;j++){
        if(i==0||j==0||i==14||j==19){
            world[i][j]=1;
        }
        else
       
        world[i][j]=getRndInteger(0,4);
        
    }
}
world[1][1]=2
var worldDict={
 0:'blank',
 1:'wall',
 2:'sushi',
 3:'onigiri'
};

function drawWorld(){
    output="";
    for(var row=0;row<world.length;row++){
        output+="<div class='row'>"
        for(var x=0;x<world[row].length;x++){
            output+="<div class ='"+worldDict[world[row][x]]+"'></div>"
        }
        output+="</div>"
    }
    document.getElementById('world').innerHTML=output;
}

drawWorld();
var ninjaman={
    x:1,
    y:1
}
var ghost={
    x:19,
    y:1
}
function drawGhost(){
    document.getElementById('ghost').style.top=
    ghost.y*40 +'px'
    document.getElementById('ghost').style.left=
    ghost.x*40 +'px'

}
function drawNinjaMan(){
    document.getElementById('ninjaman').style.top=
    ninjaman.y*40 +'px'
    document.getElementById('ninjaman').style.left=
    ninjaman.x*40 +'px'

}
function move(){
document.onkeydown=function(e){

    if(e.keyCode==65){
        if(world[ninjaman.y][ninjaman.x-1]!=1){
            if(world[ninjaman.y][ninjaman.x-1]==2){
            score+=10;
            }
            else if(world[ninjaman.y][ninjaman.x-1]==3){
                score+=5;
            }
        ninjaman.x--;
        }
    }
    if(e.keyCode==68){
        if(world[ninjaman.y][ninjaman.x+1]!=1){
            if(world[ninjaman.y][ninjaman.x+1]==2){
            score+=10;
            }
            else if(world[ninjaman.y][ninjaman.x+1]==3){
                score+=5;
            }
        ninjaman.x++;
        }
    }
    if(e.keyCode==83){
        if(world[ninjaman.y+1][ninjaman.x]!=1){
            if(world[ninjaman.y+1][ninjaman.x]==2){
            score+=10;
            }
            else if(world[ninjaman.y+1][ninjaman.x]==3){
                score+=5;
            }
        ninjaman.y++;
        }
    }
    if(e.keyCode==87){
        if(world[ninjaman.y-1][ninjaman.x]!=1){
            if(world[ninjaman.y-1][ninjaman.x]==2){
            score+=10;
            }
            else if(world[ninjaman.y-1][ninjaman.x]==3){
                score+=5;
            }
        ninjaman.y--
        }
    }
  
    world[ninjaman.y][ninjaman.x]=0;
    drawWorld();
    drawNinjaMan();
   
}
}
function refresh(){
    ninjaman.x=1;
    ninjaman.y=1;
    ghost.x=19;
    ghost.y=1;
}
   

move();
function iswin(){
    var iswin=0;
    for(var row=0;row<world.length;row++){
        for(var x=0;x<world[row].length;x++){
            if(world[row][x]==2||world[row][x]==3){
                iswin++;
            }
        }
        
    }
    if(iswin==0){
        alert("you have won.")
        document.getElementById('bigpic').style.visibility='visible';
        return true;
    }
  
}
var intervalID = window.setInterval(myCallback, 750);

function myCallback() {
     if(ninjaman.x>ghost.x){
        if(world[ghost.y][ghost.x+1]!=1)
       ghost.x++
    }
    else if(ninjaman.x<ghost.x){
        if(world[ghost.y][ghost.x-1]!=1)
        ghost.x--
    }
    if(ninjaman.y>ghost.y){
        if(world[ghost.y+1][ghost.x]!=1)
        ghost.y++
    }
    else if(ninjaman.y<ghost.y){
        if(world[ghost.y-1][ghost.x]!=1)
        ghost.y--
    }
    drawGhost();
    document.getElementById("myText").innerHTML = score;
    document.getElementById("myhearts").innerHTML=lives;
    if((ghost.x==ninjaman.x)&&(ghost.y==ninjaman.y)){
        lives--;
        refresh();
    }
    
    if(lives==0){
        alert("gameover you have lost all your lives :( Refresh to restart.");
        window.clearInterval(intervalID);
    }
    if(iswin()){
        window.clearInterval(intervalID);
    }
    drawNinjaMan();
}

</script>
<div id='ninjaman'></div>
<div id='ghost'></div>
<div id='heart'></div>
<h1>The score is: <span id="myText"></span></h1>
<h1>you have <span id="myhearts"></span> left</h1>
<FORM>
    <INPUT id='nub' TYPE="button" onClick="history.go(0)" VALUE="Refresh">
    </FORM>

    <div id='bigpic' > 
        <img src="images/won.png" alt="alternatetext">
    </div>
</body>
