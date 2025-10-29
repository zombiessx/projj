if _G.primas then return end
_G.primas=true

repeat task.wait()until game:IsLoaded()

cloneref=cloneref or function(a)return a end

local a=cloneref(game:GetService'ReplicatedStorage')
local b=cloneref(game:GetService'UserInputService')
local c=cloneref(game:GetService'HttpService')
local d=cloneref(game:GetService'VirtualUser')
local e=cloneref(game:GetService'RunService')
local f=cloneref(game:GetService'Players')
local g=cloneref(game:GetService'Debris')
local h=cloneref(game:GetService'Stats')

local i=f.LocalPlayer

_G.config={
parry_arrucacy_division=nil,
accuracy=nil,
spam_threshold=nil,
curve_keybind=nil,
manual_notify=nil,
curve_notify=nil,
curve_method='',
skin_changer=nil,
manual_spam=nil,
ability_esp=nil,
auto_parry=nil,
ball_debug=nil,
auto_spam=nil,
ping_fix=nil,
no_slow=nil,
names='',
}

if not isfolder'Primas/cfg'then
makefolder'Primas/cfg'
end

local j='Primas/cfg/blade_ball.json'

local function save_cfg()
writefile(j,c:JSONEncode(_G.config))
end

local function load_cfg()
if isfile(j)then
_G.config=c:JSONDecode(readfile(j))
end
end

load_cfg()

task.spawn(function()
repeat
save_cfg()task.wait(1)
until not _G.primas
end)

local k=loadstring(game:HttpGet"https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua")()

local l=k:CreateWindow{
Title='prypiat',
SubTitle='',
TabWidth=100,
Size=UDim2.fromOffset(440,315),
Acrylic=false,
Theme='Darker',
MinimizeKey=Enum.KeyCode.LeftControl
}

local m=l:AddTab{Title='blatant'}
local n=l:AddTab{Title='player'}
local o=l:AddTab{Title='visual'}
local p=l:AddTab{Title='misc'}

local q={}

local r={}

r.ball={
properties={
aerodynamic_time=tick(),
last_warping=tick(),
lerp_radians=0,
curving=tick(),
}
}

local s

local function linear_predict(t,u,v)
return t+(u-t)*v
end

task.spawn(function()
i.Idled:connect(function()
pcall(function()
d:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
task.wait()
d:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
end)
end)
end)

local t={}
local u={}
local v

function isValidRemoteArgs(w)
return#w==7 and
type(w[2])=="string"and
type(w[3])=="number"and
typeof(w[4])=="CFrame"and
type(w[5])=="table"and
type(w[6])=="table"and
type(w[7])=="boolean"
end

function hookRemote(w)
if not t[w]then
if not u[getrawmetatable(w)]then
u[getrawmetatable(w)]=true
local x=getrawmetatable(w)
setreadonly(x,false)

local y=x.__index
x.__index=function(z,A)
if(A=="FireServer"and z:IsA"RemoteEvent")or
(A=="InvokeServer"and z:IsA"RemoteFunction")then
return function(B,...)
local C={...}
if isValidRemoteArgs(C)and not t[z]then
t[z]=C
v=C[2]
end
return y(z,A)(B,unpack(C))
end
end
return y(z,A)
end
setreadonly(x,true)
end
end
end

for w,x in pairs(a:GetChildren())do
if x:IsA"RemoteEvent"or x:IsA"RemoteFunction"then
hookRemote(x)
end
end

a.ChildAdded:Connect(function(w)
if w:IsA"RemoteEvent"or w:IsA"RemoteFunction"then
hookRemote(w)
end
end)

function r.get_ball()
for w,x in workspace.Balls:GetChildren()do
if x:GetAttribute'realBall'then
return x
end
end
end

function r.get_balls()
local w={}

for x,y in workspace.Balls:GetChildren()do
if y:GetAttribute'realBall'then
table.insert(w,y)
end
end

return w
end

local w

function r.closest_player()
local x=math.huge
local y

for z,A in workspace.Alive:GetChildren()do
if A.Name~=i.Name then
if A.PrimaryPart then
local B=i:DistanceFromCharacter(A.PrimaryPart.Position)
if B<x then
x=B
y=A
end
end
end
end


w=y
return y
end

local x=b.TouchEnabled and not b.MouseEnabled

function r.getCurveCFrame()
local y=workspace.CurrentCamera
local z=i.Character and i.Character:FindFirstChild"HumanoidRootPart"
if not z then return y.CFrame end

local A

if not A then
local B=r.closest_player()
if B and B:FindFirstChild"HumanoidRootPart"then
A=B.HumanoidRootPart
end
end

local B=_G.config.curve_method

local C=A and A.Position or(z.Position+workspace.CurrentCamera.CFrame.LookVector*100)

if B=="dot"then
return CFrame.new(z.Position,C)
elseif B=="backwards"then
local D=(z.Position-C).Unit
local E=z.Position+D*10000+Vector3.new(0,1000,0)
return CFrame.new(workspace.CurrentCamera.CFrame.Position,E)
elseif B=="random"then
local D=(C-z.Position).Unit
local E
repeat
E=Vector3.new(
math.random(-4000,4000),
math.random(-4000,4000),
math.random(-4000,4000)
)
local F=(C+E-z.Position).Unit
local G=D:Dot(F)
until G<0.95
return CFrame.new(z.Position,C+E)
elseif B=="accelerated"then
return CFrame.new(z.Position,C+Vector3.new(0,5,0))
elseif B=="slow"then
return CFrame.new(z.Position,C+Vector3.new(0,-9000000000000000000,0))
elseif B=="high"then
return CFrame.new(z.Position,C+Vector3.new(0,9000000000000000000,0))
else
return workspace.CurrentCamera.CFrame
end
end

local y=false
local z=0

local A
local B

if a:FindFirstChild"Controllers"then
for C,D in a.Controllers:GetChildren()do
if D.Name:match"^SwordsController%s*$"then
B=D
end
end
end

if i.PlayerGui:FindFirstChild"Hotbar"and i.PlayerGui.Hotbar:FindFirstChild"Block"then
for C,D in next,getconnections(i.PlayerGui.Hotbar.Block.Activated)do
if B and getfenv(D.Function).script==B then
A=D.Function
break
end
end
end

local C=false

local D=b.TouchEnabled and not b.MouseEnabled

local E=cloneref(game:GetService"VirtualInputManager")

function r.parry()
local F=r.getCurveCFrame()

local G=b:GetMouseLocation()

local H={G.X,G.Y}

local I={}
for J,K in workspace.Alive:GetChildren()do
if K.PrimaryPart then
local L,M=pcall(function()
return workspace.CurrentCamera:WorldToScreenPoint(K.PrimaryPart.Position)
end)
if L then
I[K.Name]=M
end
end
end

local J

local K
if J then
K=J
elseif D then
local L=workspace.CurrentCamera.ViewportSize
K={L.X/2,L.Y/2}
else
K=H
end

if not C then
E:SendKeyEvent(true,Enum.KeyCode.F,false,game)
E:SendKeyEvent(false,Enum.KeyCode.F,false,game)
C=true
return
end

for L,M in t do

local N={
M[1],
M[2],
M[3],
F,
I,
K,
M[7]
}

if L:IsA"RemoteEvent"then
L:FireServer(unpack(N))
elseif L:IsA"RemoteFunction"then
L:InvokeServer(unpack(N))
end
end

if z>7 then
return false
end

z+=1

task.delay(0.5,function()
if z>0 then
z-=1
end
end)
end

function r.ball_curved()
local F=r.ball.properties

local G=r.get_ball()

if not G then
return false
end

local H=G:FindFirstChild'zoomies'

if not H then
return false
end

local I=H.VectorVelocity
local J=I.Unit

local K=(i.Character.PrimaryPart.Position-G.Position).Unit
local L=K:Dot(J)

local M=I.Magnitude
local N=math.min(M/100,40)

local O=(J-I).Unit
local P=K:Dot(O)

local Q=L-P
local R=(i.Character.PrimaryPart.Position-G.Position).Magnitude

local S=h.Network.ServerStatsItem['Data Ping']:GetValue()

local T=0.5-(S/1000)
local U=R/M-(S/1000)

local V=15-math.min(R/1000,15)+N

local W=math.clamp(L,-1,1)
local X=math.rad(math.asin(W))

F.lerp_radians=linear_predict(F.lerp_radians,X,0.8)

if M>100 and U>S/10 then
V=math.max(V-15,15)
end

if R<V then
return false
end

if Q<T then
return true
end

if F.lerp_radians<0.018 then
F.last_warping=tick()
end

if(tick()-F.last_warping)<(U/1.5)then
return true
end

if(tick()-F.curving)<(U/1.5)then
return true
end

return L<T
end

function r.get_ball_properties(F)
local G=r.get_ball()

local H=Vector3.zero
local I=G

local J=(i.Character.PrimaryPart.Position-I.Position).Unit
local K=(i.Character.PrimaryPart.Position-G.Position).Magnitude
local L=J:Dot(H.Unit)

return{Velocity=H,Direction=J,Distance=K,Dot=L}
end

function r.players_properties(F)
r.closest_player()

if not w then
return false
end

local G=w.PrimaryPart.Velocity
local H=(i.Character.PrimaryPart.Position-w.PrimaryPart.Position).Unit
local I=(i.Character.PrimaryPart.Position-w.PrimaryPart.Position).Magnitude

return{velocity=G,direction=H,distance=I}
end

local F=0

function r.perfom_spam(G)local H=
r.ball.properties

local I=r.get_ball()

local J=r.closest_player()

if not I then
return false
end

if not J or not J.PrimaryPart then
return false
end

local K=I.AssemblyLinearVelocity
local L=K.Magnitude

local M=(i.Character.PrimaryPart.Position-I.Position).Unit
local N=M:Dot(K.Unit)

local O=J.PrimaryPart.Position
local P=i:DistanceFromCharacter(O)

local Q=G.Ping+math.min(L/6,95)

if G.Entity_Properties.distance>Q then
return F
end

if G.Ball_Properties.Distance>Q then
return F
end

if P>Q then
return F
end

local R=5-math.min(L/5,5)
local S=math.clamp(N,-1,0)*R

F=Q-S

return F
end

local G={}

function qolPlayerNameVisibility()
local function createBillboardGui(H)
local I=H.Character

while(not I)or(not I.Parent)do
task.wait()
I=H.Character
end

local J=I:WaitForChild"Head"

local K=Instance.new"BillboardGui"
K.Adornee=J
K.Size=UDim2.new(0,200,0,50)
K.StudsOffset=Vector3.new(0,3,0)
K.AlwaysOnTop=true
K.Parent=J

local L=Instance.new"TextLabel"
L.Size=UDim2.new(1,0,1,0)
L.TextColor3=Color3.fromRGB(255,255,255)
L.TextSize=10
L.TextWrapped=false
L.BackgroundTransparency=1
L.TextXAlignment=Enum.TextXAlignment.Center
L.TextYAlignment=Enum.TextYAlignment.Center
L.Parent=K

G[H]=L

local M=I:FindFirstChild"Humanoid"
if M then
M.DisplayDistanceType=Enum.HumanoidDisplayDistanceType.None
end

local N
N=e.Heartbeat:Connect(function()
if not(I and I.Parent)then
N:Disconnect()
K:Destroy()
G[H]=nil
return
end

if _G.config.ability_esp then
L.Visible=true
local O=H:GetAttribute"EquippedAbility"
if O then
L.Text=H.DisplayName.." ["..O.."]"
else
L.Text=H.DisplayName
end
else
L.Visible=false
end
end)
end

for H,I in f:GetPlayers()do
if I~=i then
I.CharacterAdded:Connect(function()
createBillboardGui(I)
end)
createBillboardGui(I)
end
end

f.PlayerAdded:Connect(function(H)
H.CharacterAdded:Connect(function()
createBillboardGui(H)
end)
end)
end

qolPlayerNameVisibility()

getgenv().swordModel=_G.config.names
getgenv().swordAnimations=_G.config.names
getgenv().swordFX=_G.config.names

if getgenv().updateSword and getgenv().skin_changer then
getgenv().updateSword()
return
end

local H=game:GetService"Players"
local I=H.LocalPlayer
local J=game:GetService"ReplicatedStorage"
local K=J:WaitForChild("Shared",9000000000):WaitForChild("ReplicatedInstances",9000000000):WaitForChild("Swords",9000000000)
local L=require(K)

local M

while task.wait()and(not M)do
for N,O in getconnections(J.Remotes.FireSwordInfo.OnClientEvent)do
if O.Function and islclosure(O.Function)then
local P=getupvalues(O.Function)
if#P==1 and type(P[1])=="table"then
M=P[1]
break
end
end
end
end

function getSlashName(N)
local O=L:GetSword(N)
return(O and O.SlashName)or"SlashEffect"
end

function setSword()
if not _G.config.skin_changer then return end

setupvalue(rawget(L,"EquipSwordTo"),3,false)

L:EquipSwordTo(I.Character,getgenv().swordModel)
M:SetSword(getgenv().swordAnimations)
end

local N
local O

while task.wait()and not O do
for P,Q in getconnections(J.Remotes.ParrySuccessAll.OnClientEvent)do
if Q.Function and getinfo(Q.Function).name=="parrySuccessAll"then
O=Q
N=Q.Function
Q:Disable()
end
end
end

local P
while task.wait()and not P do
for Q,R in getconnections(J.Remotes.ParrySuccessClient.Event)do
if R.Function and getinfo(R.Function).name=="parrySuccessAll"then
P=R
R:Disable()
end
end
end

getgenv().slashName=getSlashName(getgenv().swordFX)

local Q=0
local R={}

J.Remotes.ParrySuccessAll.OnClientEvent:Connect(function(...)
setthreadidentity(2)
local S={...}
if tostring(S[4])~=I.Name then
Q=tick()
elseif _G.config.skin_changer then
S[1]=getgenv().slashName
S[3]=getgenv().swordFX
end
return N(unpack(S))
end)

table.insert(R,getconnections(J.Remotes.ParrySuccessAll.OnClientEvent)[1])

getgenv().updateSword=function()
getgenv().slashName=getSlashName(getgenv().swordFX)
setSword()
end

task.spawn(function()
while task.wait()do
if _G.config.skin_changer then
local S=I.Character or I.CharacterAdded:Wait()
if I:GetAttribute"CurrentlyEquippedSword"~=getgenv().swordModel then
setSword()
end
if S and(not S:FindFirstChild(getgenv().swordModel))then
setSword()
end
for T,U in(S and S:GetChildren())or{}do
if U:IsA"Model"and U.Name~=getgenv().swordModel then
U:Destroy()
end
task.wait()
end
end
end
end)

_G.config.curve_method='camera'



local S=0.7

m:AddToggle('Toggle',{
Title='auto parry',
Default=_G.config.auto_parry or false,
Callback=function(T)
_G.config.auto_parry=T

if T then
q.auto_parry=e.PreSimulation:Connect(function()
local U=r.ball.properties

local V=r.get_ball()
local W=r.get_balls()

for X,Y in W do

if not Y then
return
end

local Z=Y:FindFirstChild'zoomies'

if not Z then
return
end

Y:GetAttributeChangedSignal'target':Once(function()
y=false
end)

if y then
return
end

local _=Y:GetAttribute'target'
local aa=V:GetAttribute'target'

local ab=Z.VectorVelocity

local ac=(i.Character.PrimaryPart.Position-Y.Position).Magnitude

local ad=h.Network.ServerStatsItem['Data Ping']:GetValue()/10

local ae=math.clamp(ad/10,5,17)

local af=ab.Magnitude

local ag=math.min(math.max(af-9.5,0),650)
local ah=2.4+ag*0.002

local ai=S

local aj=ah*S
local ak=ae+math.max(af/aj,9.5)

local al=r.ball_curved()

if Y:FindFirstChild'AeroDynamicSlashVFX'then
g:AddItem(Y.AeroDynamicSlashVFX,0)
U.aerodynamic_time=tick()
end

if workspace.Runtime:FindFirstChild'Tornado'then
if(tick()-U.aerodynamic_time)<(workspace.Runtime.Tornado:GetAttribute"TornadoTime"or 1)+0.314159 then
return
end
end

if aa==i.Name and al then
return
end

if _==i.Name and ac<=ak and ai*0.8 then
r.parry(_G.config.curve_method)
y=true
end

local am=tick()

repeat
e.PreSimulation:Wait()
until(tick()-am)>=1 or not y
y=false
end
end)
else
if q.auto_parry then
q.auto_parry:Disconnect()
q.auto_parry=nil
end
end
end
})

a.Remotes.ParrySuccessAll.OnClientEvent:Connect(function(aa,ab)
if ab.Parent and ab.Parent~=i.Character then
if ab.Parent.Parent~=workspace.Alive then
return
end
end

r.closest_player()

local ac=r.get_ball()

if not ac then
return
end

local ad=(i.Character.PrimaryPart.Position-w.PrimaryPart.Position).Magnitude
local ae=(i.Character.PrimaryPart.Position-ac.Position).Magnitude
local af=(i.Character.PrimaryPart.Position-ac.Position).Unit
local ag=af:Dot(ac.AssemblyLinearVelocity.Unit)

local ah=r.ball_curved()

if ad<15 and ae<15 and ag>-0.25 then
if ah then
r.parry(_G.config.curve_method)
end
end

if not s then
return
end

s:Stop()
end)

a.Remotes.ParrySuccess.OnClientEvent:Connect(function()
if i.Character.Parent~=workspace.Alive then
return
end

if not s then
return
end

s:Stop()
end)

workspace.Balls.ChildAdded:Connect(function()
y=false
end)

workspace.Balls.ChildRemoved:Connect(function(aa)
z=0
y=false

if q.target_change then
q.target_change:Disconnect()
q.target_change=nil
end
end)

a.Remotes.ParrySuccessAll.OnClientEvent:Connect(function(aa,ab)
local ac=i.Character.PrimaryPart
local ad=r.get_ball()

if not ad then
return
end

local ae=ad:FindFirstChild'zoomies'

if not ae then
return
end

local af=ae.VectorVelocity.Magnitude

local ag=(i.Character.PrimaryPart.Position-ad.Position).Magnitude
local ah=ae.VectorVelocity

local ai=ah.Unit

local aj=(i.Character.PrimaryPart.Position-ad.Position).Unit
aj:Dot(ai)

local ak=h.Network.ServerStatsItem['Data Ping']:GetValue()


local al=math.min(af/100,40)
local am=ag/af-(ak/1000)

local T=af>100
local U=15-math.min(ag/1000,15)+al

if T and am>ak/10 then
U=math.max(U-15,15)
end

if ab~=ac and ag>U then
r.ball.properties.curving=tick()
end
end)

m:AddSlider('Slider',{
Title='accuracy',
Description='',
Default=_G.config.accuracy or 100,
Min=1,
Max=100,
Rounding=1,
Callback=function(aa:number)
S=0.5+(aa-1)*(0.015656566)
_G.config.accuracy=aa
end
})

local aa=m:AddDropdown('Dropdown',{
Title='curve method',
Values={
'camera',
'random',
'dot',
'backwards',
'slow',
'accelerated',
'high',
},
Multi=false,
Default=_G.config.curve_method or'camera',
Callback=function(aa)
task.spawn(function()
_G.config.curve_method=aa
end)
end
})

local ab={
[Enum.KeyCode.One]="camera",
[Enum.KeyCode.Two]="random",
[Enum.KeyCode.Three]="dot",
[Enum.KeyCode.Four]="backwards",
[Enum.KeyCode.Five]="slow",
[Enum.KeyCode.Six]="accelerated",
[Enum.KeyCode.Seven]="high",
}

local ac

b.InputBegan:Connect(function(ad,ae)
if ae then return end
if not _G.config.curve_keybind then
return
end

local af=ab[ad.KeyCode]

if not af then
return
end

if _G.config.curve_method==af then
if _G.config.curve_notify then
k:Notify{
Title='curve method',
Content='already: '..af,
Duration=3
}
end

return
end

_G.config.curve_method=af
aa:SetValue(af)
ac=af

if _G.config.curve_notify then
k:Notify{
Title='curve method',
Content=af,
Duration=3
}
end
end)

m:AddToggle('Toggle',{
Title='hotkey curve',
Default=_G.config.curve_keybind or false,
Callback=function(ad)
task.spawn(function()
_G.config.curve_keybind=ad
end)
end
})

m:AddToggle('Toggle',{
Title='curve notify',
Default=_G.config.curve_notify or false,
Callback=function(ad)
task.spawn(function()
_G.config.curve_notify=ad
end)
end
})

local ad
local ae

if a:FindFirstChild"Controllers"then
for af,ag in ipairs(a.Controllers:GetChildren())do
if ag.Name:match"^SwordsController%s*$"then
ae=ag
end
end
end

if i.PlayerGui:FindFirstChild"Hotbar"and i.PlayerGui.Hotbar:FindFirstChild"Block"then
for af,ag in next,getconnections(i.PlayerGui.Hotbar.Block.Activated)do
if ae and getfenv(ag.Function).script==ae then
ad=ag.Function
break
end
end
end


m:AddToggle('Toggle',{
Title='auto spam',
Default=_G.config.auto_spam or false,
Callback=function(af)
_G.config.auto_spam=af

if af then
q.auto_spam=e.PreSimulation:Connect(function()

local ag=r.get_ball()

if not ag then
return
end

local ah=ag:FindFirstChild'zoomies'

if not ah then
return
end

r.closest_player()

local ai=h.Network.ServerStatsItem['Data Ping']:GetValue()

local aj=math.clamp(ai/10,1,16)

local ak=ag:GetAttribute'target'

local al=r:get_ball_properties()
local am=r:players_properties()

local T=r.perfom_spam{
Ball_Properties=al,
Entity_Properties=am,
Ping=aj
}

local U=w.PrimaryPart.Position

if not U then
return
end
local V=i:DistanceFromCharacter(U)

local W=(i.Character.PrimaryPart.Position-ag.Position).Unit
local X=ah.VectorVelocity.Unit

W:Dot(X)

local Y=i:DistanceFromCharacter(ag.Position)

if not ak then
return
end

if V>T or Y>T then
return
end

local Z=i.Character:GetAttribute'Pulsed'

if Z then
return
end

if ak==i.Name and V>30 and Y>30 then
return
end

local _=_G.config.spam_threshold

if Y<=T and z>_ then
r.parry()
end
end)
else
if q.auto_spam then
q.auto_spam:Disconnect()
q.auto_spam=nil
end
end
end
})

m:AddSlider('Slider',{
Title='threshold',
Description='',
Default=_G.config.spam_threshold or 3,
Min=1,
Max=3,
Rounding=1,
Callback=function(af:number)
task.spawn(function()
_G.config.spam_threshold=af
end)
end
})

b.InputBegan:Connect(function(af,ag)
if ag then return end
if af.KeyCode==Enum.KeyCode.E then
_G.manual_spam=not _G.manual_spam
end
end)

local af=m:AddKeybind('Keybind',{
Title='manual spam',
Mode='Toggle',
Default=_G.config.manual_spam or'E',
Callback=function(af)
if af then
q.manual_spam=e.RenderStepped:Connect(function()
r.parry()
end)
if _G.config.manual_notify then
k:Notify{
Title='manual spam',
Content='on',
Duration=4
}
end
else
if q.manual_spam then
q.manual_spam:Disconnect()
q.manual_spam=nil
end
if _G.config.manual_notify then
k:Notify{
Title='manual spam',
Content='off',
Duration=4
}
end
end
end
})

af:OnChanged(function(ag)
_G.config.manual_spam=ag
end)

m:AddToggle('Toggle',{
Title='manual notify',
Default=_G.config.manual_notify or false,
Callback=function(ag)
_G.config.manual_notify=ag
end
})

n:AddToggle('Toggle',{
Title='no slow',
Default=_G.config.no_slow,
Callback=function(ag)
_G.config.no_slow=ag

if ag then
q.no_slow=e.PostSimulation:Connect(function()
if not i.Character then
return
end

if not workspace.Alive:FindFirstChild(i.Name)then
return
end

if not i.Character:FindFirstChild'Humanoid'then
return
end

if i.Character.Humanoid.WalkSpeed<36 or i.Character.Humanoid.PlatformStand then
i.Character.Humanoid.WalkSpeed=36
end
end)
else
if q.no_slow then
q.no_slow:Disconnect()
q.no_slow=nil
end
end
end
})

o:AddToggle('Toggle',{
Title='ability esp',
Default=_G.config.ability_esp or false,
Callback=function(ag)
_G.config.ability_esp=ag
for ah,ai in pairs(G)do
ai.Visible=ag
end
end
})

local ag

o:AddToggle('Toggle',{
Title='sword changer',
Default=_G.config.skin_changer or false,
Callback=function(ah)
_G.config.skin_changer=ah

task.spawn(function()
if ah then
ag=i.Character:GetAttribute'CurrentlyEquippedSword'
getgenv().updateSword()
else
if ag then
setupvalue(rawget(L,"EquipSwordTo"),2,false)
L:EquipSwordTo(I.Character,ag)
M:SetSword(ag)
end
end
end)
end
})

o:AddInput('Input',{
Title='sword name',
Default=_G.config.names or i.Character:GetAttribute'CurrentlyEquippedSword'.Name,
Placeholder='Placeholder',
Numeric=false,
Finished=true,
Callback=function(ah)
task.spawn(function()
getgenv().swordModel=ah
getgenv().swordAnimations=ah
getgenv().swordFX=ah
_G.config.names=tostring(ah)
if _G.config.skin_changer then
getgenv().updateSword()
end
end)
end
})

p:AddToggle('Toggle',{
Title='ping fix',
Default=_G.config.ping_fix or false,
Callback=function(ah)
_G.config.ping_fix=ah
task.spawn(function()
while _G.config.ping_fix do task.wait()
if _G.manual_spam or h.Network.ServerStatsItem['Data Ping']:GetValue()>270 then
setfpscap(60)
else
if x then
setfpscap(60)
else
setfpscap(999)
end
end
end
if x then
setfpscap(60)
else
setfpscap(999)
end
end)
end
})

local ah
p:AddToggle('Toggle',{
Title='ball stats',
Default=_G.config.ball_debug or false,
Callback=function(ai)
_G.config.ball_debug=ai

if ai then
ah=Instance.new("ScreenGui",i:WaitForChild"PlayerGui")
local aj=Instance.new("TextLabel",ah)
local ak=Instance.new("TextLabel",ah)

local al={}

ah.ResetOnSpawn=false

aj.Name="BallStatsLabel"
aj.Size=UDim2.new(0.2,0,0.05,0)
aj.Position=UDim2.new(0.7,0,0.1,0)
aj.TextScaled=false
aj.TextSize=26
aj.BackgroundTransparency=1
aj.TextColor3=Color3.new(1,1,1)
aj.Font=Enum.Font.Fantasy
aj.ZIndex=2

ak.Name="PeakStatsLabel"
ak.Size=UDim2.new(0.2,0,0.05,0)
ak.Position=UDim2.new(0.7,0,0.135,0)
ak.TextScaled=false
ak.TextSize=26
ak.BackgroundTransparency=1
ak.TextColor3=Color3.new(1,1,1)
ak.Font=aj.Font
ak.ZIndex=2

r.ball_stats=e.Heartbeat:Connect(function()
local am=r.get_balls()or{}

if#am==0 then
aj.Text="Waiting..."
ak.Text='Max Speed: 0'
return
end

for T,U in al do
local V=false
for W,X in am do
if X==T then
V=true
end
end
if not V then
al[T]=nil
end
end

for T,U in am do
local V=U:FindFirstChild"zoomies"
if V then
local W=V.VectorVelocity.Magnitude
al[U]=al[U]or 0
if W>al[U]then
al[U]=W
end

local X=("Ball Speed: %.2f"):format(W)
aj.Text=X

local Y=("Max Speed: %.2f"):format(al[U])
ak.Text=Y
break
end
end
end)
else
if r.ball_stats then
r.ball_stats:Disconnect()
r.ball_stats=nil
end

if ah then
ah:Destroy()
ah=nil
end
end
end
})

l:SelectTab(1)
