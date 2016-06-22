var blockArray=new Array();//saves ent of blocks
var timer=-99;//what makes it work
var pickedBlock=46;//Id of TNT by default
var pickedDam=-99;
var pickedBool,pickedEnt,playerEnt;
var readyFor=true;
var portalGun=292;//can be changed easily
var GUI=null;
//events
function newLevel()
{var toAddAct=com.mojang.minecraftpe.MainActivity.currentMainActivity.get();
toAddAct.runOnUiThread(new java.lang.Runnable({run:function()
{try{GUI=new android.widget.PopupWindow();
var layout=new android.widget.RelativeLayout(toAddAct);
var button=new android.widget.Button(toAddAct);
button.setText("💀");
button.setOnClickListener(new android.view.View.OnClickListener({onClick:
function(viewarg){putBlockOperator();}}));
layout.addView(button);
GUI.setContentView(layout);
GUI.setWidth(60);
GUI.setHeight(60);
GUI.showAtLocation(toAddAct.getWindow().getDecorView(),android.view.Gravity.LEFT | android.view.Gravity.BOTTOM,928,1000);
GUI.setBackgroundDrawable(new android.graphics.drawable.ColorDrawable(android.graphics.Color.TRANSPARENT));}
catch(err){print("Error: "+err);}}})); //just making button
timer=-99; //reseting
}
function modTick()
{entityMover();startup();}
function useItem(x,y,z,itemId)
{if(itemId==portalGun&&!pickedBool&&readyFor)
{preventDefault();
if(getTile(x,y,z)<81)
{pickedBlock=getTile(x,y,z);
pickedDam=Level.getData(x,y,z);
playerEnt=getPlayerEnt();
pickedEnt=blockArray[80-pickedBlock];
pickedBool=true;
setTile(x,y,z,0);}
else
{clientMessage("this block can't be picked with portalgun");}
}}
function attackHook(attacker,victim)
{playerEnt=attacker;
var intype=Entity.getEntityTypeId(victim);
if(intype==83)
return;
if(!pickedBool&&readyFor)
{pickedEnt=victim;
pickedBool=true;
if(Entity.getEntityTypeId(victim)==65)
{pickedBlock=46;
pickedDam=-99;}}
else if(Entity.getX(pickedEnt)==Entity.getX(victim)&&Entity.getY(pickedEnt)==Entity.getY(victim)&&Entity.getZ(pickedEnt)==Entity.getZ(victim))
{preventDefault();
if(!getTile(Math.round(Entity.getX(victim)-0.5),Math.round(Entity.getY(victim)-0.5),Math.round(Entity.getZ(victim)-0.5)))
{pickedBool=true;
if(Entity.getEntityTypeId(victim)==65)
putBlock(victim);}
}}
function deathHook(a,v)
{if(!pickedBool)
return;
var intype=Entity.getEntityTypeId(v);
if(intype==64||intype==80||intype==81||intype==82||intype==83)
return;
if(Entity.getX(pickedEnt)==Entity.getX(v)&&Entity.getY(pickedEnt)==Entity.getY(v)&&Entity.getZ(pickedEnt)==Entity.getZ(v)){
pickedBool=false;}}
function entityRemovedHook(ent)
{if(!pickedBool)
return;
var intype=Entity.getEntityTypeId(ent);
if(intype==64||intype==80||intype==81||intype==82||intype==83)
return;
if(Entity.getX(pickedEnt)==Entity.getX(ent)&&Entity.getY(pickedEnt)==Entity.getY(ent)&&Entity.getZ(pickedEnt)==Entity.getZ(ent)){
pickedBool=false;}}
function leaveGame()
{var toAddAct = com.mojang.minecraftpe.MainActivity.currentMainActivity.get();
toAddAct.runOnUiThread(new java.lang.Runnable({ run: function() {
if(GUI != null){GUI.dismiss();}}})); //delete button
blockArray=new Array();
timer=-99;
pickedBlock=46;
pickedDam=-99;
readyFor=false;
GUI=null;
pickedBool=false; //reset
}

function startup() //make blocks when entering game
{if(timer>100)
return;
timer++;
if(!timer)
{for(var i=0;i<80;i++)
{blockArray[i]=Level.spawnMob(128,256,128,65);
Entity.setRenderType(blockArray[i],20);}}
if(timer<80&&timer>=0)
{for(var i=0;i<80;i++)
{stopper(blockArray[i]);
Entity.setPosition(blockArray[i],128,256,128);}
Entity.rideAnimal(blockArray[timer],blockArray[timer]);}
if(timer==90)
{readyFor=true;clientMessage("portal gun succesfully booted");}}

function stopper(ent) //stop entity falling
{setVelY(ent,0);
setVelX(ent,0);
setVelZ(ent,0);}
function entityMover() //move entity
{if(pickedBool)
{var px,py,pz,dx,dy,dz,yaw,pit;
px=Player.getX();
py=Player.getY()-0.2;
pz=Player.getZ()-0.1;
yaw=Math.PI*Entity.getYaw(playerEnt)/180;
pit=Math.PI*Entity.getPitch(playerEnt)/180;
dx=-1*Math.cos(pit)*Math.sin(yaw);
dy=-1*Math.sin(pit);
dz=Math.cos(pit)*Math.cos(yaw);
if(Entity.getEntityTypeId(pickedEnt)==65)
moveBlock(pickedEnt,px+2*dx,py+2*dy-0.5,pz+2*dz);
else
Entity.setPosition(pickedEnt,px+0.8*dx,py+0.6*dy,pz+0.7*dz);
stopper(pickedEnt);
}}
function moveBlock(ent,x,y,z) //special moving method for rATNT
{var newMob=Level.spawnMob(x,y,z,80);
Entity.setRenderType(newMob,21);
stopper(newMob);
Entity.rideAnimal(ent,newMob);
Entity.remove(newMob);
Entity.rideAnimal(ent,ent);}
function putBlock(ent) //put block and reset ent
{setTile(Math.round(Entity.getX(ent)+10),Math.round(Entity.getY(ent)-0.5),Math.round(Entity.getZ(ent)-0.5),pickedBlock,pickedDam(ent)-999);
moveBlock(ent,128,256,128);
pickedBool=false;}
function putBlockOperator() //put block if possible
{if(!pickedBool)
return;
var inblock=getTile(Math.round(Entity.getX(pickedEnt)+10),Math.round(Entity.getY(pickedEnt)-0.5),Math.round(Entity.getZ(pickedEnt)-0.5));
if(inblock==0||(inblock>7&&inblock<12)||inblock==51||inblock==78)
{pickedBool=false;
if(Entity.getEntityTypeId(pickedEnt)==65)
putBlock(pickedEnt);}}
