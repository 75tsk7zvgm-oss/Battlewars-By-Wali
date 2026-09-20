# Battlewars-By-Wali
```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport"
      content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">

<title>BattleWars</title>

<style>
* {
    box-sizing:border-box;
    -webkit-user-select:none;
    user-select:none;
    -webkit-touch-callout:none;
    touch-action:none;
}

html,
body {
    margin:0;
    width:100%;
    height:100%;
    overflow:hidden;
    background:#080b10;
    font-family:Arial,Helvetica,sans-serif;
}

canvas {
    position:fixed;
    inset:0;
    width:100%;
    height:100%;
}

#rotatePrompt {
    display:none;
    position:fixed;
    inset:0;
    z-index:500;
    background:#080b10;
    color:white;
    flex-direction:column;
    align-items:center;
    justify-content:center;
    text-align:center;
    gap:14px;
    padding:20px;
}

#rotatePrompt .icon {
    font-size:52px;
    animation:spin 1.8s ease-in-out infinite;
}

@keyframes spin {
    0%   { transform:rotate(0deg); }
    50%  { transform:rotate(90deg); }
    100% { transform:rotate(90deg); }
}

#rotatePrompt h2 { margin:0; font-size:20px; }
#rotatePrompt p { margin:0; color:#9eacbd; font-size:13px; max-width:280px; }

@media(orientation:portrait) and (max-width:900px){
    #rotatePrompt.enabled { display:flex; }
}

#auth {
    position:fixed;
    inset:0;
    z-index:200;
    color:white;
    display:flex;
    align-items:center;
    justify-content:center;
    background:
        radial-gradient(circle at 72% 35%, rgba(54,130,255,.35), transparent 32%),
        linear-gradient(135deg, #07101d, #142338 55%, #080b12);
}

.authBox {
    width:min(360px,88vw);
    max-height:92vh;
    overflow-y:auto;
    -webkit-overflow-scrolling:touch;
    touch-action:pan-y;
    padding:26px 26px 22px;
    background:rgba(4,8,15,.86);
    border:1px solid rgba(255,255,255,.12);
    border-radius:14px;
}

.authLogo { font-size:30px; font-weight:1000; letter-spacing:2px; text-align:center; margin-bottom:4px; }
.authLogo span { color:#55a8ff; }
.authSub { text-align:center; color:#9eacbd; font-size:12px; margin-bottom:18px; }

.authTabs {
    display:flex;
    gap:6px;
    margin-bottom:16px;
    background:rgba(255,255,255,.06);
    padding:4px;
    border-radius:8px;
}

.authTab {
    flex:1;
    padding:9px 0;
    text-align:center;
    border-radius:6px;
    font-size:13px;
    font-weight:900;
    color:#9eacbd;
    pointer-events:auto;
}

.authTab.active { background:#378ff3; color:white; }

.authField { margin-bottom:12px; }
.authField label { display:block; font-size:11px; color:#9eacbd; margin-bottom:5px; font-weight:700; }

.authField input {
    width:100%;
    padding:11px 12px;
    border-radius:7px;
    border:1px solid rgba(255,255,255,.16);
    background:rgba(255,255,255,.06);
    color:white;
    font-size:14px;
    pointer-events:auto;
}

.authError { display:none; color:#ff7a7a; font-size:11px; margin-bottom:10px; }

.authSubmit {
    width:100%;
    height:46px;
    border:0;
    border-radius:7px;
    background:#ffbd32;
    color:#111;
    font-weight:1000;
    font-size:15px;
    margin-top:4px;
    pointer-events:auto;
}

.authGuest {
    width:100%;
    height:40px;
    margin-top:10px;
    border:1px solid rgba(255,255,255,.18);
    border-radius:7px;
    background:transparent;
    color:#c5cfdb;
    font-size:12px;
    font-weight:800;
    pointer-events:auto;
}

#menu {
    display:none;
    position:fixed;
    inset:0;
    z-index:100;
    color:white;
    overflow-y:auto;
    -webkit-overflow-scrolling:touch;
    touch-action:pan-y;
    background:
        radial-gradient(circle at 72% 35%, rgba(54,130,255,.35), transparent 32%),
        linear-gradient(135deg, #07101d, #142338 55%, #080b12);
}

.topBar {
    display:flex;
    align-items:center;
    justify-content:space-between;
    padding:calc(env(safe-area-inset-top) + 14px) 22px 10px;
}

.logo { font-size:26px; font-weight:1000; letter-spacing:1.5px; text-shadow:2px 3px 0 #000; }
.logo span { color:#55a8ff; }

.profile {
    display:flex;
    align-items:center;
    gap:9px;
    background:rgba(0,0,0,.42);
    border:1px solid rgba(255,255,255,.1);
    padding:7px 11px;
    border-radius:10px;
}

.avatar { width:32px; height:32px; border-radius:50%; background:linear-gradient(135deg,#55a8ff,#824cff); }
.playerName { font-size:12px; font-weight:900; }
.level { color:#9eafc4; font-size:9px; margin-top:1px; }

.modeGrid {
    display:grid;
    grid-template-columns:repeat(auto-fit,minmax(230px,1fr));
    gap:14px;
    padding:6px 22px 26px;
    max-width:900px;
    margin:0 auto;
    touch-action:pan-y;
}

.modeCardBig {
    padding:16px;
    background:rgba(4,8,15,.82);
    border:1px solid rgba(255,255,255,.12);
    border-radius:12px;
}

.modeTag {
    display:inline-block;
    padding:3px 8px;
    border-radius:5px;
    background:#2f6fdb;
    font-size:10px;
    font-weight:900;
    margin-bottom:8px;
}

.modeTag.bed { background:#c0392b; }
.modeTitle { font-size:18px; font-weight:1000; }
.modeDescription { margin-top:4px; color:#9eacbd; font-size:12px; min-height:32px; }

.playButton {
    width:100%;
    height:44px;
    margin-top:12px;
    border:0;
    border-radius:6px;
    background:#ffbd32;
    color:#111;
    font-weight:1000;
    font-size:14px;
    pointer-events:auto;
}

.logoutRow { text-align:center; padding-bottom:20px; }
.logoutButton { color:#8a97a8; font-size:11px; background:none; border:0; pointer-events:auto; }

#hud { display:none; position:fixed; inset:0; z-index:20; pointer-events:none; }

#matchInfo {
    position:absolute;
    left:calc(env(safe-area-inset-left) + 14px);
    top:calc(env(safe-area-inset-top) + 10px);
    color:white;
    text-shadow:2px 2px 4px #000;
}

.matchTitle { font-size:13px; font-weight:1000; }
.matchSub { margin-top:3px; color:#c5cfdb; font-size:10px; }

#status {
    position:absolute;
    right:calc(env(safe-area-inset-right) + 14px);
    top:calc(env(safe-area-inset-top) + 8px);
    width:160px;
    padding:9px;
    border-radius:10px;
    background:rgba(5,9,15,.68);
    border:1px solid rgba(255,255,255,.14);
}

.statusLine { display:flex; justify-content:space-between; align-items:center; color:white; font-size:10px; font-weight:900; }
.statBar { width:100%; height:8px; margin-top:4px; border-radius:4px; background:#202630; overflow:hidden; }
#healthBar { width:100%; height:100%; background:#31d56c; }
#shieldBar { width:65%; height:100%; background:#3caaff; }

#bedStatus {
    position:absolute;
    right:calc(env(safe-area-inset-right) + 14px);
    top:calc(env(safe-area-inset-top) + 86px);
    width:160px;
    padding:9px;
    border-radius:10px;
    background:rgba(5,9,15,.68);
    border:1px solid rgba(255,255,255,.14);
    display:none;
    color:white;
    font-size:11px;
    font-weight:900;
}

#bedStatus .row { display:flex; justify-content:space-between; margin-top:3px; }
#bedStatus .yours { color:#4fb2ff; }
#bedStatus .theirs { color:#ff5a5a; }

#crosshair { position:absolute; left:50%; top:50%; width:20px; height:20px; transform:translate(-50%,-50%); }
#crosshair::before, #crosshair::after { content:""; position:absolute; background:white; box-shadow:0 0 3px black; }
#crosshair::before { left:9px; width:2px; height:20px; }
#crosshair::after  { top:9px;  width:20px; height:2px; }

#buildIndicator {
    display:none;
    position:absolute;
    left:50%;
    top:60%;
    transform:translateX(-50%);
    padding:6px 12px;
    border-radius:5px;
    background:rgba(47,126,224,.8);
    color:white;
    font-size:11px;
    font-weight:900;
}

#hitMarker {
    position:absolute;
    left:50%;
    top:50%;
    width:30px;
    height:30px;
    transform:translate(-50%,-50%);
    opacity:0;
    color:white;
    font-size:22px;
    text-align:center;
}

.bedHealthTag {
    position:absolute;
    display:none;
    transform:translate(-50%,-100%);
    padding:5px 9px;
    border-radius:6px;
    background:rgba(5,9,15,.78);
    border:1px solid rgba(255,255,255,.14);
    color:white;
    font-size:10px;
    font-weight:900;
    white-space:nowrap;
    text-align:center;
}

.bedHealthTag .barTrack {
    width:64px;
    height:6px;
    margin-top:4px;
    border-radius:3px;
    background:#202630;
    overflow:hidden;
}

.bedHealthTag .barFill {
    height:100%;
}

#respawnScreen {
    display:none;
    position:absolute;
    inset:0;
    z-index:40;
    align-items:center;
    justify-content:center;
    flex-direction:column;
    gap:10px;
    background:rgba(3,6,11,.82);
    color:white;
    pointer-events:auto;
}

#respawnScreen h1 {
    margin:0;
    font-size:30px;
    letter-spacing:1px;
    color:#ff5a5a;
}

#respawnScreen p {
    margin:0;
    color:#9eacbd;
    font-size:13px;
}

#respawnTimer {
    font-size:46px;
    font-weight:1000;
    margin:4px 0;
}

#respawnNowButton {
    width:200px;
    height:42px;
    border:0;
    border-radius:7px;
    background:#ffbd32;
    color:#111;
    font-weight:1000;
    font-size:13px;
    margin-top:6px;
}

#roundEnd {
    display:none;
    position:absolute;
    inset:0;
    z-index:40;
    align-items:center;
    justify-content:center;
    flex-direction:column;
    gap:14px;
    background:rgba(3,6,11,.82);
    color:white;
    pointer-events:auto;
}

#roundEnd h1 { margin:0; font-size:34px; letter-spacing:1px; }

#roundEnd button {
    width:220px;
    height:44px;
    border:0;
    border-radius:7px;
    background:#ffbd32;
    color:#111;
    font-weight:1000;
    font-size:14px;
}

#inventory {
    position:absolute;
    left:50%;
    bottom:calc(env(safe-area-inset-bottom) + 10px);
    transform:translateX(-50%);
    display:flex;
    gap:5px;
    pointer-events:auto;
}

.slot {
    position:relative;
    width:44px;
    height:44px;
    border-radius:7px;
    border:2px solid rgba(255,255,255,.25);
    background:rgba(7,11,18,.76);
    display:flex;
    align-items:center;
    justify-content:center;
    color:white;
}

.slot.active { border-color:#ffd63c; background:rgba(40,37,18,.82); }
.slotNumber { position:absolute; left:3px; top:2px; color:#9da8b6; font-size:7px; }
.item { font-size:18px; }
.itemCount { position:absolute; right:3px; bottom:2px; font-size:7px; }

#weaponInfo {
    position:absolute;
    right:calc(env(safe-area-inset-right) + 14px);
    bottom:calc(env(safe-area-inset-bottom) + 68px);
    width:150px;
    padding:7px 9px;
    border-radius:7px;
    background:rgba(5,9,15,.72);
    color:white;
}

.weaponName { font-size:12px; font-weight:1000; }
.weaponType { color:#9ca9ba; font-size:8px; margin-top:2px; }
.ammo { margin-top:2px; font-size:17px; font-weight:1000; }

#joystickArea {
    position:absolute;
    left:calc(env(safe-area-inset-left) + 18px);
    bottom:calc(env(safe-area-inset-bottom) + 18px);
    width:125px;
    height:125px;
    pointer-events:auto;
}

#joystick { position:absolute; inset:0; border-radius:50%; border:2px solid rgba(255,255,255,.24); background:rgba(255,255,255,.075); }

#stick {
    position:absolute;
    width:54px;
    height:54px;
    left:35.5px;
    top:35.5px;
    border-radius:50%;
    border:2px solid rgba(255,255,255,.5);
    background:rgba(255,255,255,.45);
}

#sprint {
    position:absolute;
    left:calc(env(safe-area-inset-left) + 158px);
    bottom:calc(env(safe-area-inset-bottom) + 32px);
    width:50px;
    height:50px;
}

.control {
    position:absolute;
    pointer-events:auto;
    display:flex;
    align-items:center;
    justify-content:center;
    color:white;
    font-size:10px;
    font-weight:1000;
    border-radius:50%;
    border:2px solid rgba(255,255,255,.4);
    background:rgba(7,11,18,.58);
}

#fire {
    right:calc(env(safe-area-inset-right) + 20px);
    bottom:calc(env(safe-area-inset-bottom) + 34px);
    width:76px;
    height:76px;
    background:rgba(191,46,46,.56);
    font-size:11px;
}

#jump {
    right:calc(env(safe-area-inset-right) + 102px);
    bottom:calc(env(safe-area-inset-bottom) + 116px);
    width:60px;
    height:60px;
}

#build {
    right:calc(env(safe-area-inset-right) + 20px);
    bottom:calc(env(safe-area-inset-bottom) + 118px);
    width:54px;
    height:54px;
    background:rgba(55,105,190,.58);
}

#view {
    right:calc(env(safe-area-inset-right) + 100px);
    bottom:calc(env(safe-area-inset-bottom) + 34px);
    width:46px;
    height:46px;
}

#reload {
    right:calc(env(safe-area-inset-right) + 162px);
    bottom:calc(env(safe-area-inset-bottom) + 36px);
    width:46px;
    height:46px;
}
</style>
</head>


<body>

<div id="rotatePrompt">
    <div class="icon">📱</div>
    <h2>Turn your device sideways</h2>
    <p>BattleWars plays best in landscape. Rotate your phone to jump in.</p>
</div>


<div id="auth">
    <div class="authBox">

        <div class="authLogo">BATTLE<span>WARS</span></div>
        <div class="authSub">Build. Defend. Destroy.</div>

        <div class="authTabs">
            <div class="authTab active" id="tabSignIn">Sign In</div>
            <div class="authTab" id="tabSignUp">Sign Up</div>
        </div>

        <div class="authError" id="authError"></div>

        <div class="authField">
            <label>Username</label>
            <input id="authUsername" type="text" maxlength="16" placeholder="Choose a username" autocomplete="off">
        </div>

        <div class="authField">
            <label>Password</label>
            <input id="authPassword" type="password" maxlength="32" placeholder="Enter a password" autocomplete="off">
        </div>

        <button class="authSubmit" id="authSubmit">SIGN IN</button>
        <button class="authGuest" id="authGuest">PLAY AS GUEST</button>

    </div>
</div>


<div id="menu">

    <div class="topBar">
        <div class="logo">BATTLE<span>WARS</span></div>
        <div class="profile">
            <div class="avatar"></div>
            <div>
                <div class="playerName" id="menuPlayerName">PLAYER</div>
                <div class="level">LEVEL 12</div>
            </div>
        </div>
    </div>

    <div class="modeGrid">

        <div class="modeCardBig">
            <span class="modeTag">Battle Royale</span>
            <div class="modeTitle">BATTLE ROYALE</div>
            <div class="modeDescription">Drop in solo. Loot up. Eliminate every enemy and be the last one standing.</div>
            <button class="playButton" data-mode="royale">PLAY NOW</button>
        </div>

        <div class="modeCardBig">
            <span class="modeTag bed">Bed Wars • 1v1</span>
            <div class="modeTitle">BED DEFENSE</div>
            <div class="modeDescription">Protect your bed, break your bot rival's bed, and build your way to victory.</div>
            <button class="playButton" data-mode="bedwars">PLAY NOW</button>
        </div>

    </div>

    <div class="logoutRow">
        <button class="logoutButton" id="logoutButton">Sign out</button>
    </div>

</div>


<div id="hud">

    <div id="matchInfo">
        <div class="matchTitle" id="matchTitle">BATTLE ROYALE</div>
        <div class="matchSub" id="matchSub">SOLO • STORM IN 4:52</div>
    </div>

    <div id="status">
        <div class="statusLine"><span>HEALTH</span><span id="healthNumber">100</span></div>
        <div class="statBar"><div id="healthBar"></div></div>
        <div class="statusLine" style="margin-top:6px"><span>SHIELD</span><span id="shieldNumber">65</span></div>
        <div class="statBar"><div id="shieldBar"></div></div>
    </div>

    <div id="bedStatus">
        <div class="row"><span class="yours">YOUR BED</span><span id="yourBedHP">200</span></div>
        <div class="row"><span class="theirs">BOT BED</span><span id="theirBedHP">200</span></div>
    </div>

    <div id="crosshair"></div>
    <div id="hitMarker">×</div>
    <div id="buildIndicator">BUILD MODE</div>

    <div class="bedHealthTag" id="playerBedTag">
        YOUR BED
        <div class="barTrack"><div class="barFill" id="playerBedTagFill" style="width:100%;background:#3caaff"></div></div>
    </div>

    <div class="bedHealthTag" id="botBedTag">
        BOT BED
        <div class="barTrack"><div class="barFill" id="botBedTagFill" style="width:100%;background:#ff5a5a"></div></div>
    </div>

    <div id="respawnScreen">
        <h1>ELIMINATED</h1>
        <div id="respawnTimer">3</div>
        <p>Respawning at your bed...</p>
        <button id="respawnNowButton">RESPAWN NOW</button>
    </div>

    <div id="weaponInfo">
        <div class="weaponName">ASSAULT RIFLE</div>
        <div class="weaponType">COMMON • RIFLE</div>
        <div class="ammo"><span id="ammo">30</span><span style="color:#8c98a8;font-size:11px"> / 120</span></div>
    </div>

    <div id="inventory">
        <div class="slot active"><span class="slotNumber">1</span><span class="item">🔫</span></div>
        <div class="slot"><span class="slotNumber">2</span><span class="item">🗡️</span></div>
        <div class="slot"><span class="slotNumber">3</span><span class="item">🧪</span><span class="itemCount">2</span></div>
        <div class="slot"><span class="slotNumber">4</span><span class="item">🧱</span><span class="itemCount">99</span></div>
        <div class="slot"><span class="slotNumber">5</span><span class="item">💊</span><span class="itemCount">3</span></div>
    </div>

    <div id="joystickArea">
        <div id="joystick"></div>
        <div id="stick"></div>
    </div>

    <div id="sprint" class="control">RUN</div>

    <div id="fire" class="control">FIRE</div>
    <div id="jump" class="control">JUMP</div>
    <div id="build" class="control">BUILD</div>
    <div id="view" class="control">VIEW</div>
    <div id="reload" class="control">RELOAD</div>

    <div id="roundEnd">
        <h1 id="roundEndTitle">VICTORY</h1>
        <button id="roundEndButton">BACK TO MENU</button>
    </div>

</div>


<script src="https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.min.js"></script>

<script>

/* AUTH — client-side only, stored on this device */

let authMode="signin";

const tabSignIn=document.getElementById("tabSignIn");
const tabSignUp=document.getElementById("tabSignUp");
const authSubmit=document.getElementById("authSubmit");
const authError=document.getElementById("authError");
const authUsername=document.getElementById("authUsername");
const authPassword=document.getElementById("authPassword");

function setAuthMode(mode){
    authMode=mode;
    authError.style.display="none";
    tabSignIn.classList.toggle("active",mode==="signin");
    tabSignUp.classList.toggle("active",mode==="signup");
    authSubmit.textContent = mode==="signin" ? "SIGN IN" : "CREATE ACCOUNT";
}

tabSignIn.addEventListener("pointerdown",()=>setAuthMode("signin"));
tabSignUp.addEventListener("pointerdown",()=>setAuthMode("signup"));

function loadAccounts(){
    try{ return JSON.parse(localStorage.getItem("bw_accounts")||"{}"); }
    catch(e){ return {}; }
}

function saveAccounts(accounts){
    try{ localStorage.setItem("bw_accounts",JSON.stringify(accounts)); }
    catch(e){}
}

function showAuthError(message){
    authError.textContent=message;
    authError.style.display="block";
}

authSubmit.addEventListener("pointerdown",()=>{

    const username=authUsername.value.trim();
    const password=authPassword.value;

    if(username.length<3){ showAuthError("Username needs at least 3 characters."); return; }
    if(password.length<4){ showAuthError("Password needs at least 4 characters."); return; }

    const accounts=loadAccounts();

    if(authMode==="signup"){
        if(accounts[username]){ showAuthError("That username is already taken."); return; }
        accounts[username]={password:password,level:1};
        saveAccounts(accounts);
        enterMenu(username);
        return;
    }

    if(!accounts[username]){ showAuthError("No account with that username. Try signing up."); return; }
    if(accounts[username].password!==password){ showAuthError("Wrong password."); return; }

    enterMenu(username);
});

document.getElementById("authGuest").addEventListener("pointerdown",()=>{
    enterMenu("Guest"+Math.floor(Math.random()*9000+1000));
});

function enterMenu(name){
    document.getElementById("menuPlayerName").textContent=name.toUpperCase();
    document.getElementById("auth").style.display="none";
    document.getElementById("menu").style.display="block";
}

document.getElementById("logoutButton").addEventListener("pointerdown",()=>{
    document.getElementById("menu").style.display="none";
    document.getElementById("auth").style.display="flex";
    authUsername.value="";
    authPassword.value="";
});


/* ORIENTATION */

document.getElementById("rotatePrompt").classList.add("enabled");


/* ENGINE STATE */

let scene, camera, renderer;
let player, gun, body, backpack;

let enemies=[];
let bullets=[];
let builds=[];

let health=100;
let shield=65;
let ammo=30;
let reserveAmmo=120;

let yaw=0;
let pitch=-.12;

let velocityY=0;
let grounded=true;

let firstPerson=false;
let buildMode=false;
let sprinting=false;
let gameActive=false;

let gameMode="royale";

let playerBed=null;
let botBed=null;
let playerBedHP=200;
let botBedHP=200;
let botShootCooldown=0;

const keys={};


/* SCENE / CAMERA / RENDERER */

scene=new THREE.Scene();
scene.background=new THREE.Color(0x86c8ea);
scene.fog=new THREE.Fog(0x86c8ea,80,250);

camera=new THREE.PerspectiveCamera(70,innerWidth/innerHeight,.05,500);

renderer=new THREE.WebGLRenderer({antialias:true});
renderer.setSize(innerWidth,innerHeight);
renderer.setPixelRatio(Math.min(devicePixelRatio,2));
renderer.shadowMap.enabled=true;
document.body.appendChild(renderer.domElement);


/* LIGHT */

const sunlight=new THREE.DirectionalLight(0xffffff,2.5);
sunlight.position.set(80,120,50);
sunlight.castShadow=true;
sunlight.shadow.mapSize.width=2048;
sunlight.shadow.mapSize.height=2048;
scene.add(sunlight);

scene.add(new THREE.HemisphereLight(0xd8efff,0x35452f,1.4));


/* WORLD */

let worldGroup=new THREE.Group();
scene.add(worldGroup);

function clearWorld(){

    scene.remove(worldGroup);
    worldGroup=new THREE.Group();
    scene.add(worldGroup);

    for(const b of builds) scene.remove(b);
    builds=[];

    for(const e of enemies) scene.remove(e);
    enemies=[];

    for(const b of bullets) scene.remove(b);
    bullets=[];

    playerBed=null;
    botBed=null;
}

function ground(size,color){
    const g=new THREE.Mesh(
        new THREE.PlaneGeometry(size,size),
        new THREE.MeshStandardMaterial({color:color,roughness:1})
    );
    g.rotation.x=-Math.PI/2;
    g.receiveShadow=true;
    worldGroup.add(g);
}

function road(x,z,width,depth){
    const r=new THREE.Mesh(
        new THREE.BoxGeometry(width,.08,depth),
        new THREE.MeshStandardMaterial({color:0x41464c})
    );
    r.position.set(x,.04,z);
    worldGroup.add(r);
}

function building(x,z,width,height,depth,color){
    const b=new THREE.Mesh(
        new THREE.BoxGeometry(width,height,depth),
        new THREE.MeshStandardMaterial({color:color||0x6b625a,roughness:.85})
    );
    b.position.set(x,height/2,z);
    b.castShadow=true;
    b.receiveShadow=true;
    worldGroup.add(b);
}

function makeTree(x,z){

    const group=new THREE.Group();

    const trunk=new THREE.Mesh(
        new THREE.CylinderGeometry(.45,.6,4,10),
        new THREE.MeshStandardMaterial({color:0x70462d})
    );
    trunk.position.y=2;
    group.add(trunk);

    const leaves=new THREE.Mesh(
        new THREE.SphereGeometry(2.7,16,12),
        new THREE.MeshStandardMaterial({color:0x24773b})
    );
    leaves.position.y=5;
    group.add(leaves);

    group.position.set(x,0,z);
    group.traverse(o=>{ if(o.isMesh) o.castShadow=true; });
    worldGroup.add(group);
}

function makeBed(x,z,color){

    const group=new THREE.Group();

    const frame=new THREE.Mesh(
        new THREE.BoxGeometry(2.6,.5,4.2),
        new THREE.MeshStandardMaterial({color:0x6b4a2d})
    );
    frame.position.y=.45;
    frame.castShadow=true;
    group.add(frame);

    const mattress=new THREE.Mesh(
        new THREE.BoxGeometry(2.3,.35,3.9),
        new THREE.MeshStandardMaterial({color:color})
    );
    mattress.position.y=.85;
    mattress.castShadow=true;
    group.add(mattress);

    const pillow=new THREE.Mesh(
        new THREE.BoxGeometry(2.1,.3,.8),
        new THREE.MeshStandardMaterial({color:0xffffff})
    );
    pillow.position.set(0,1.05,-1.5);
    group.add(pillow);

    group.position.set(x,0,z);
    group.userData.hp=200;
    group.userData.color=color;

    worldGroup.add(group);

    return group;
}


/* PLAYER MODEL */

player=new THREE.Group();
scene.add(player);

body=new THREE.Mesh(
    new THREE.CapsuleGeometry(.55,1.25,8,16),
    new THREE.MeshStandardMaterial({color:0x286fe0})
);
body.position.y=1.7;
body.castShadow=true;
player.add(body);

const head=new THREE.Mesh(
    new THREE.SphereGeometry(.43,20,20),
    new THREE.MeshStandardMaterial({color:0xf0b789})
);
head.position.y=2.95;
head.castShadow=true;
player.add(head);

backpack=new THREE.Mesh(
    new THREE.BoxGeometry(.75,.9,.25),
    new THREE.MeshStandardMaterial({color:0x242c3b})
);
backpack.position.set(0,1.8,.43);
player.add(backpack);

gun=new THREE.Group();

const gunBody=new THREE.Mesh(
    new THREE.BoxGeometry(.2,.25,1.25),
    new THREE.MeshStandardMaterial({color:0x24282d,metalness:.65})
);
gun.add(gunBody);

const barrel=new THREE.Mesh(
    new THREE.CylinderGeometry(.055,.055,.7,10),
    new THREE.MeshStandardMaterial({color:0x101216,metalness:.8})
);
barrel.rotation.x=Math.PI/2;
barrel.position.z=-.95;
gun.add(barrel);

gun.position.set(.65,2.05,-.35);
player.add(gun);


/* ENEMY / BOT FACTORY */

function createEnemy(x,z,color){

    const enemy=new THREE.Group();
    enemy.position.set(x,0,z);

    const ebody=new THREE.Mesh(
        new THREE.CapsuleGeometry(.55,1.25,8,16),
        new THREE.MeshStandardMaterial({color:color||0xd43b43})
    );
    ebody.position.y=1.7;
    ebody.castShadow=true;
    enemy.add(ebody);

    const ehead=new THREE.Mesh(
        new THREE.SphereGeometry(.43,20,20),
        new THREE.MeshStandardMaterial({color:0xf0b68b})
    );
    ehead.position.y=2.95;
    ehead.castShadow=true;
    enemy.add(ehead);

    enemy.userData.health=100;

    scene.add(enemy);
    enemies.push(enemy);

    return enemy;
}


/* BUILD WORLDS */

function buildRoyaleWorld(){

    ground(300,0x4e9147);
    road(0,0,300,16);
    road(0,0,16,300);

    building(-35,-30,18,15,17);
    building(37,-30,20,20,18);
    building(-38,35,22,12,19);
    building(39,36,18,17,18);

    for(let i=0;i<55;i++){
        const x=(Math.random()-.5)*230;
        const z=(Math.random()-.5)*230;
        if(Math.abs(x)<25 && Math.abs(z)<25) continue;
        makeTree(x,z);
    }

    player.position.set(0,0,0);

    createEnemy(18,-12);
    createEnemy(-22,-20);
    createEnemy(27,21);
    createEnemy(-26,26);
}

function buildBedWarsWorld(){

    ground(120,0x3f7a45);
    building(0,0,10,1,10,0x9c8158);

    playerBed=makeBed(0,-34,0x2f6fdb);
    botBed=makeBed(0,34,0xc0392b);

    for(let i=0;i<14;i++){
        const x=(Math.random()-.5)*90;
        const z=(Math.random()-.5)*30;
        makeTree(x,z);
    }

    building(-22,-10,6,4,6,0x8a7a63);
    building(22,10,6,4,6,0x8a7a63);

    player.position.set(0,0,-26);

    const bot=createEnemy(0,26,0xc0392b);
    bot.userData.isBedBot=true;
    bot.userData.health=150;
}


/* SHOOT */

function shoot(){

    if(!gameActive || ammo<=0) return;

    ammo--;
    document.getElementById("ammo").textContent=ammo;

    const direction=new THREE.Vector3();
    camera.getWorldDirection(direction);

    const bullet=new THREE.Mesh(
        new THREE.SphereGeometry(.075,8,8),
        new THREE.MeshBasicMaterial({color:0xffdc55})
    );

    bullet.position.copy(camera.position);
    bullet.userData.velocity=direction.clone().multiplyScalar(1.8);
    bullet.userData.life=100;
    bullet.userData.friendly=true;

    scene.add(bullet);
    bullets.push(bullet);
}

function botShoot(bot){

    const direction=player.position.clone().sub(bot.position);
    direction.y+=1.6;
    direction.normalize();

    const bullet=new THREE.Mesh(
        new THREE.SphereGeometry(.075,8,8),
        new THREE.MeshBasicMaterial({color:0xff6a6a})
    );

    bullet.position.copy(bot.position);
    bullet.position.y+=1.7;
    bullet.userData.velocity=direction.multiplyScalar(1.2);
    bullet.userData.life=140;
    bullet.userData.friendly=false;

    scene.add(bullet);
    bullets.push(bullet);
}


/* BULLET UPDATE */

function updateBullets(){

    for(let i=bullets.length-1;i>=0;i--){

        const bullet=bullets[i];

        bullet.position.add(bullet.userData.velocity);
        bullet.userData.life--;

        let hit=false;

        if(bullet.userData.friendly){

            for(let j=enemies.length-1;j>=0;j--){

                const enemy=enemies[j];
                const target=enemy.position.clone();
                target.y+=1.7;

                if(bullet.position.distanceTo(target)<1.1){

                    enemy.userData.health-=35;
                    hitMarker();

                    scene.remove(bullet);
                    bullets.splice(i,1);
                    hit=true;

                    if(enemy.userData.health<=0){

                        scene.remove(enemy);
                        enemies.splice(j,1);

                        if(gameMode==="bedwars"){
                            setTimeout(()=>respawnBot(),3000);
                        }else if(enemies.length===0){
                            endRound(true);
                        }
                    }

                    break;
                }
            }

            if(!hit && gameMode==="bedwars" && botBed){

                const bedTarget=botBed.position.clone();
                bedTarget.y+=.85;

                if(bullet.position.distanceTo(bedTarget)<2.4){

                    botBedHP=Math.max(0,botBedHP-15);
                    document.getElementById("theirBedHP").textContent=botBedHP;

                    hitMarker();
                    scene.remove(bullet);
                    bullets.splice(i,1);
                    hit=true;

                    if(botBedHP<=0) endRound(true);
                }
            }

        }else{

            const target=player.position.clone();
            target.y+=1.7;

            if(bullet.position.distanceTo(target)<1.1){
                damagePlayer(6);
                scene.remove(bullet);
                bullets.splice(i,1);
                hit=true;
            }

            if(!hit && playerBed){

                const bedTarget=playerBed.position.clone();
                bedTarget.y+=.85;

                if(bullet.position.distanceTo(bedTarget)<2.4 && Math.random()<.4){

                    playerBedHP=Math.max(0,playerBedHP-15);
                    document.getElementById("yourBedHP").textContent=playerBedHP;

                    scene.remove(bullet);
                    bullets.splice(i,1);
                    hit=true;

                    if(playerBedHP<=0) endRound(false);
                }
            }
        }

        if(!hit && bullet.userData.life<=0){
            scene.remove(bullet);
            bullets.splice(i,1);
        }
    }
}

function respawnBot(){

    if(!gameActive || gameMode!=="bedwars") return;

    const bot=createEnemy((Math.random()-.5)*10,30,0xc0392b);
    bot.userData.isBedBot=true;
    bot.userData.health=150;
}


/* HIT MARKER */

function hitMarker(){
    const marker=document.getElementById("hitMarker");
    marker.style.opacity=1;
    setTimeout(()=>{ marker.style.opacity=0; },100);
}


/* RELOAD */

function reload(){
    const needed=30-ammo;
    const amount=Math.min(needed,reserveAmmo);
    ammo+=amount;
    reserveAmmo-=amount;
    document.getElementById("ammo").textContent=ammo;
}


/* BUILD */

function build(){

    if(!gameActive) return;

    buildMode=!buildMode;
    document.getElementById("buildIndicator").style.display = buildMode ? "block" : "none";

    if(!buildMode) return;

    const direction=new THREE.Vector3();
    camera.getWorldDirection(direction);
    direction.y=0;
    direction.normalize();

    const position=player.position.clone().add(direction.multiplyScalar(4));
    position.y=2;

    const wall=new THREE.Mesh(
        new THREE.BoxGeometry(5,4,.3),
        new THREE.MeshStandardMaterial({color:0x4c9be8,transparent:true,opacity:.8})
    );

    wall.position.copy(position);
    wall.rotation.y=yaw;
    wall.castShadow=true;

    scene.add(wall);
    builds.push(wall);
}


/* JUMP */

function jump(){
    if(!grounded || !gameActive) return;
    velocityY=10;
    grounded=false;
}


/* MOVEMENT */

function updatePlayer(){

    if(!gameActive) return;

    let forward=0;
    let side=0;

    if(keys["w"]) forward+=1;
    if(keys["s"]) forward-=1;
    if(keys["a"]) side-=1;
    if(keys["d"]) side+=1;

    forward-=joyY;
    side+=joyX;

    const length=Math.sqrt(forward*forward+side*side);
    if(length>1){ forward/=length; side/=length; }

    const speed=(sprinting||keys["shift"]) ? .27 : .145;

    const sin=Math.sin(yaw);
    const cos=Math.cos(yaw);

    player.position.x+=(side*cos+forward*sin)*speed;
    player.position.z+=(side*sin-forward*cos)*speed;

    const bound = gameMode==="bedwars" ? 56 : 148;
    player.position.x=THREE.MathUtils.clamp(player.position.x,-bound,bound);
    player.position.z=THREE.MathUtils.clamp(player.position.z,-bound,bound);

    if(Math.abs(forward)+Math.abs(side)>.1){
        player.rotation.y=yaw+Math.atan2(side,forward);
    }

    velocityY-=.45;
    player.position.y+=velocityY*.05;

    if(player.position.y<=0){
        player.position.y=0;
        velocityY=0;
        grounded=true;
    }
}


/* CAMERA */

function updateCamera(){

    const target=new THREE.Vector3(
        player.position.x,
        player.position.y+(firstPerson?2.5:2.2),
        player.position.z
    );

    if(firstPerson){
        camera.position.copy(target);
        camera.rotation.order="YXZ";
        camera.rotation.y=yaw;
        camera.rotation.x=pitch;
        return;
    }

    const distance=7;
    const offset=new THREE.Vector3(
        Math.sin(yaw)*distance,
        3.2+pitch*2,
        Math.cos(yaw)*distance
    );

    camera.position.copy(target.clone().add(offset));
    camera.lookAt(target);
}


/* BED HEALTH TAGS */

function projectToScreen(vec3){

    const v=vec3.clone();
    v.project(camera);

    return {
        x:(v.x*.5+.5)*innerWidth,
        y:(-v.y*.5+.5)*innerHeight,
        inFront:v.z<1
    };
}

function updateBedTags(){

    const playerTag=document.getElementById("playerBedTag");
    const botTag=document.getElementById("botBedTag");

    if(gameMode!=="bedwars" || !playerBed || !botBed){
        playerTag.style.display="none";
        botTag.style.display="none";
        return;
    }

    const playerPoint=playerBed.position.clone();
    playerPoint.y+=2.6;

    const botPoint=botBed.position.clone();
    botPoint.y+=2.6;

    const playerScreen=projectToScreen(playerPoint);
    const botScreen=projectToScreen(botPoint);

    if(playerScreen.inFront){
        playerTag.style.display="block";
        playerTag.style.left=playerScreen.x+"px";
        playerTag.style.top=playerScreen.y+"px";
    }else{
        playerTag.style.display="none";
    }

    if(botScreen.inFront){
        botTag.style.display="block";
        botTag.style.left=botScreen.x+"px";
        botTag.style.top=botScreen.y+"px";
    }else{
        botTag.style.display="none";
    }

    document.getElementById("playerBedTagFill").style.width=
        Math.max(0,playerBedHP/200*100)+"%";

    document.getElementById("botBedTagFill").style.width=
        Math.max(0,botBedHP/200*100)+"%";
}


/* ENEMY / BOT AI */

function updateEnemies(){

    for(const enemy of enemies){

        if(gameMode==="bedwars" && enemy.userData.isBedBot){

            let goal;
            const distToPlayer=enemy.position.distanceTo(player.position);

            if(distToPlayer<20) goal=player.position;
            else if(playerBed) goal=playerBed.position;
            else goal=player.position;

            const direction=goal.clone().sub(enemy.position);
            direction.y=0;

            if(direction.length()>2.5){
                direction.normalize();
                enemy.position.add(direction.multiplyScalar(.045));
            }

            enemy.lookAt(goal.x,1.7,goal.z);

            botShootCooldown--;

            if(botShootCooldown<=0 && enemy.position.distanceTo(player.position)<40){
                botShoot(enemy);
                botShootCooldown=55;
            }

            continue;
        }

        const distance=enemy.position.distanceTo(player.position);

        if(distance<35){

            const direction=player.position.clone().sub(enemy.position);
            direction.y=0;

            if(direction.length()>3){
                direction.normalize();
                enemy.position.add(direction.multiplyScalar(.025));
            }

            enemy.lookAt(player.position.x,1.7,player.position.z);

            if(distance<3.5 && Math.random()<.015){
                damagePlayer(5);
            }
        }
    }
}


/* PLAYER DAMAGE */

function damagePlayer(amount){

    if(shield>0){
        const shieldDamage=Math.min(shield,amount);
        shield-=shieldDamage;
        amount-=shieldDamage;
    }

    health-=amount;
    health=Math.max(0,health);

    document.getElementById("healthBar").style.width=health+"%";
    document.getElementById("shieldBar").style.width=shield+"%";
    document.getElementById("healthNumber").textContent=Math.round(health);
    document.getElementById("shieldNumber").textContent=Math.round(shield);

    if(health<=0){

        if(gameMode==="bedwars" && playerBedHP>0){
            showRespawnScreen();
        }else{
            endRound(false);
        }
    }
}


/* RESPAWN */

let respawnInterval=null;

function showRespawnScreen(){

    gameActive=false;
    cameraTouch=null;
    resetJoystick();

    document.getElementById("respawnScreen").style.display="flex";

    let seconds=3;
    document.getElementById("respawnTimer").textContent=seconds;

    clearInterval(respawnInterval);

    respawnInterval=setInterval(()=>{

        seconds--;

        if(seconds<=0){
            clearInterval(respawnInterval);
            respawnPlayer();
        }else{
            document.getElementById("respawnTimer").textContent=seconds;
        }

    },1000);
}

function respawnPlayer(){

    clearInterval(respawnInterval);
    document.getElementById("respawnScreen").style.display="none";

    health=100;
    shield=65;
    ammo=30;
    reserveAmmo=120;

    document.getElementById("healthBar").style.width="100%";
    document.getElementById("shieldBar").style.width="65%";
    document.getElementById("healthNumber").textContent="100";
    document.getElementById("shieldNumber").textContent="65";
    document.getElementById("ammo").textContent="30";

    if(playerBed){
        player.position.set(playerBed.position.x,0,playerBed.position.z+6);
    }else{
        player.position.set(0,0,0);
    }

    velocityY=0;
    grounded=true;

    gameActive=true;
}

document.getElementById("respawnNowButton").addEventListener("pointerdown",()=>{
    respawnPlayer();
});


/* ROUND END */

function endRound(won){
    gameActive=false;
    document.getElementById("roundEndTitle").textContent = won ? "VICTORY" : "ELIMINATED";
    document.getElementById("roundEnd").style.display="flex";
}

document.getElementById("roundEndButton").addEventListener("pointerdown",()=>{
    document.getElementById("roundEnd").style.display="none";
    document.getElementById("hud").style.display="none";
    document.getElementById("menu").style.display="block";
});


/* JOYSTICK */

const joystickArea=document.getElementById("joystickArea");
const stick=document.getElementById("stick");

let joystickPointer=null;
let joyX=0;
let joyY=0;

function updateJoystick(x,y){

    const rect=joystickArea.getBoundingClientRect();
    const centerX=rect.left+rect.width/2;
    const centerY=rect.top+rect.height/2;

    let dx=x-centerX;
    let dy=y-centerY;

    const max=Math.min(42,rect.width*.33);
    const distance=Math.sqrt(dx*dx+dy*dy);

    if(distance>max){
        dx=dx/distance*max;
        dy=dy/distance*max;
    }

    joyX=dx/max;
    joyY=dy/max;

    stick.style.left=(rect.width/2-27+dx)+"px";
    stick.style.top=(rect.height/2-27+dy)+"px";
}

joystickArea.addEventListener("pointerdown",e=>{
    joystickPointer=e.pointerId;
    joystickArea.setPointerCapture(e.pointerId);
    updateJoystick(e.clientX,e.clientY);
});

joystickArea.addEventListener("pointermove",e=>{
    if(e.pointerId===joystickPointer){
        updateJoystick(e.clientX,e.clientY);
    }
});

function resetJoystick(){
    joystickPointer=null;
    joyX=0;
    joyY=0;
    stick.style.left=(joystickArea.offsetWidth/2-27)+"px";
    stick.style.top=(joystickArea.offsetHeight/2-27)+"px";
}

joystickArea.addEventListener("pointerup",resetJoystick);
joystickArea.addEventListener("pointercancel",resetJoystick);


/* CAMERA TOUCH (right side drag = look) */

let cameraTouch=null;

renderer.domElement.addEventListener("pointerdown",e=>{

    if(!gameActive) return;
    if(e.clientX<innerWidth*.40) return;

    cameraTouch={id:e.pointerId,x:e.clientX,y:e.clientY};
    renderer.domElement.setPointerCapture(e.pointerId);
});

renderer.domElement.addEventListener("pointermove",e=>{

    if(!cameraTouch || e.pointerId!==cameraTouch.id) return;

    const dx=e.clientX-cameraTouch.x;
    const dy=e.clientY-cameraTouch.y;

    yaw-=dx*.007;
    pitch-=dy*.005;

    pitch=THREE.MathUtils.clamp(pitch,-1.1,.6);

    cameraTouch.x=e.clientX;
    cameraTouch.y=e.clientY;
});

renderer.domElement.addEventListener("pointerup",()=>{
    cameraTouch=null;
});


/* BUTTONS */

document.getElementById("jump").addEventListener("pointerdown",jump);
document.getElementById("fire").addEventListener("pointerdown",shoot);
document.getElementById("build").addEventListener("pointerdown",build);
document.getElementById("reload").addEventListener("pointerdown",reload);

document.getElementById("sprint").addEventListener("pointerdown",()=>{ sprinting=true; });
document.getElementById("sprint").addEventListener("pointerup",()=>{ sprinting=false; });

document.getElementById("view").addEventListener("pointerdown",()=>{
    firstPerson=!firstPerson;
    body.visible=!firstPerson;
    backpack.visible=!firstPerson;
});


/* KEYBOARD (desktop testing) */

window.addEventListener("keydown",e=>{

    keys[e.key.toLowerCase()]=true;

    if(e.key===" ") jump();
    if(e.key.toLowerCase()==="f") shoot();
    if(e.key.toLowerCase()==="r") reload();
    if(e.key.toLowerCase()==="b") build();

    if(e.key.toLowerCase()==="v"){
        firstPerson=!firstPerson;
        body.visible=!firstPerson;
    }
});

window.addEventListener("keyup",e=>{
    keys[e.key.toLowerCase()]=false;
});


/* START GAME */

function resetMatchState(){

    health=100;
    shield=65;
    ammo=30;
    reserveAmmo=120;
    playerBedHP=200;
    botBedHP=200;
    botShootCooldown=60;

    yaw=0;
    pitch=-.12;
    velocityY=0;
    grounded=true;
    buildMode=false;
    firstPerson=false;
    body.visible=true;
    backpack.visible=true;

    document.getElementById("healthBar").style.width="100%";
    document.getElementById("shieldBar").style.width="65%";
    document.getElementById("healthNumber").textContent="100";
    document.getElementById("shieldNumber").textContent="65";
    document.getElementById("ammo").textContent="30";
    document.getElementById("buildIndicator").style.display="none";
    document.getElementById("yourBedHP").textContent="200";
    document.getElementById("theirBedHP").textContent="200";

    clearInterval(respawnInterval);
    document.getElementById("respawnScreen").style.display="none";
}

function startGame(mode){

    gameMode=mode;

    clearWorld();
    resetMatchState();

    if(mode==="royale"){

        buildRoyaleWorld();
        document.getElementById("matchTitle").textContent="BATTLE ROYALE";
        document.getElementById("matchSub").textContent="SOLO • ELIMINATE ALL ENEMIES";
        document.getElementById("bedStatus").style.display="none";

    }else{

        buildBedWarsWorld();
        document.getElementById("matchTitle").textContent="BED DEFENSE";
        document.getElementById("matchSub").textContent="1v1 • DESTROY THE BOT'S BED";
        document.getElementById("bedStatus").style.display="block";
    }

    document.getElementById("menu").style.display="none";
    document.getElementById("hud").style.display="block";

    gameActive=true;
}

document.querySelectorAll(".playButton").forEach(btn=>{
    btn.addEventListener("pointerdown",()=>{
        startGame(btn.dataset.mode);
    });
});


/* RESIZE */

window.addEventListener("resize",()=>{
    camera.aspect=innerWidth/innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(innerWidth,innerHeight);
});


/* GAME LOOP */

function animate(){

    requestAnimationFrame(animate);

    if(gameActive){
        updatePlayer();
        updateCamera();
        updateBullets();
        updateEnemies();
        updateBedTags();
    }

    renderer.render(scene,camera);
}

animate();

</script>

</body>
</html>
```