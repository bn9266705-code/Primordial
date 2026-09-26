local Player=game:GetService("Players").LocalPlayer
local RS=game:GetService("ReplicatedStorage")
local Survs=workspace:FindFirstChild("Players") and workspace.Players:FindFirstChild("Survivors")
local VOL=5

local function import(link,name)
if not isfile(name) then
writefile(name,game:HttpGet(link))
end
return getcustomasset(name)
end

local LMS_SHED=import("https://github.com/bn9266705-code/Primordial/raw/refs/heads/main/lms%20com%20shed.mp3","Primordial_LMS_Shed.mp3")
local CH1=import("https://github.com/bn9266705-code/Primordial/raw/refs/heads/main/Chase%20Theme%20(1).mp3","Primordial_Chase1.mp3")
local CH2=import("https://github.com/bn9266705-code/Primordial/raw/refs/heads/main/Chase%20Theme%20(2).mp3","Primordial_Chase2.mp3")
local CH3=import("https://github.com/bn9266705-code/Primordial/raw/refs/heads/main/Chase%20Theme%20(3).mp3","Primordial_Chase3.mp3")
local CH4=import("https://github.com/bn9266705-code/Primordial/raw/refs/heads/main/Chase%20Theme%20(4).mp3","Primordial_Chase4.mp3")

local OLD={
UnstableEyeActivate="rbxassetid://128439277996285",
EntanglementActivate="rbxassetid://128711903717226",
RejuvenateTheRottenActivate="rbxassetid://128226726836653",
Hit="rbxassetid://89828271975845",
Swing="rbxassetid://101199185291628",
UnstableEyeMode="rbxassetid://134037097890097",
Stunned="rbxassetid://102398799721605",
Chains="rbxassetid://84279430446111",
MassInfectionActivate="rbxassetid://79782181585087",
Introduction="rbxassetid://97851287933867",
Footsteps={"rbxassetid://75457135715759","rbxassetid://95246424997069"},
}

pcall(function()
local cfg=require(RS.Assets.Skins.Killers["1x1x1x1"].Primordial1x1x1x1.Config)
cfg.Sounds=cfg.Sounds or {}
for k,v in pairs(OLD) do
cfg.Sounds[k]=v
end
end)

pcall(function()
local base=require(RS.Assets.Killers["1x1x1x1"].Config)
base.Sounds=base.Sounds or {}
for k,v in pairs(OLD) do
base.Sounds[k]=v
end
end)

local CREATION="rbxassetid://115884097233860"
local ORIG_CHASE={
["rbxassetid://111134311711719"]=CH1,
["rbxassetid://83321609403833"]=CH2,
["rbxassetid://116381899127536"]=CH3,
["rbxassetid://112319866608633"]=CH4,
}

local C3,C4
local function clearC()
if C3 then C3:Disconnect() C3=nil end
if C4 then C4:Disconnect() C4=nil end
end

local function isPrimordial(P)
if not P or P.Name~="1x1x1x1" then return false end
local skin=P:GetAttribute("SkinName") or ""
return skin=="Primordial1x1x1x1" or skin=="Primordial"
end

local function boost(v)
v.Volume=VOL
end

local function DoMorph(P)
if not isPrimordial(P) then return end
clearC()
if not Survs then
Survs=workspace:FindFirstChild("Players") and workspace.Players:FindFirstChild("Survivors")
end

C3=workspace.Themes.ChildAdded:Connect(function(v)
if not v:IsA("Sound") then return end
task.wait(0.05)
if v.SoundId==CREATION or v.Name=="LastSurvivor" then
if Survs and Survs:FindFirstChild("Shedletsky") and LMS_SHED then
v.SoundId=LMS_SHED
boost(v)
v:Play()
return
end
end
if v.Name=="LastSurvivor" then
if Survs and Survs:FindFirstChild("Shedletsky") and LMS_SHED then
v.SoundId=LMS_SHED
boost(v)
v:Play()
end
return
end
local rep=ORIG_CHASE[v.SoundId]
if rep then
v.SoundId=rep
boost(v)
v:Play()
end
end)

local prim=P.PrimaryPart or P:FindFirstChild("HumanoidRootPart")
if prim then
C4=prim.ChildAdded:Connect(function(v)
if not v:IsA("Sound") then return end
boost(v)
end)
end

local hum=P:FindFirstChildOfClass("Humanoid")
if hum then hum.Died:Once(clearC) end
Player.CharacterAdded:Once(clearC)
end

Player.CharacterAdded:Connect(function(Rig) task.wait(0.5) DoMorph(Rig) end)
if Player.Character then task.spawn(function() task.wait(0.5) DoMorph(Player.Character) end) end

task.spawn(function()
local k=workspace:FindFirstChild("Players") and workspace.Players:FindFirstChild("Killers")
if not k then
local pf=workspace:WaitForChild("Players",30)
if pf then k=pf:WaitForChild("Killers",30) end
end
if not k then return end
local function try(c) task.wait(0.6) DoMorph(c) end
for _,c in ipairs(k:GetChildren()) do try(c) end
k.ChildAdded:Connect(try)
end)

print("[Primordial] old SFX + LMS Shed + Chase")
pcall(function()
game.StarterGui:SetCore("SendNotification",{Title="Primordial",Text="SFX antigo + Themes",Duration=3})
end)
