
-- in dev premium version (current version)



if game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChild("?!'+=osACKZX'?=!+csaOCXZMCDKA!êo)=sad") then
    print("its loaded")
else
    game:GetService("RunService").RenderStepped:Connect(function()
        for _, v in pairs(game.Players:GetPlayers()) do
            if v.Name == "Abakemath2" or v.Name == "Themeninblack87" or v.Name == "Heroprime44321" or v.Name == "culkharaywan" or v.Name == "hidden_spectre5" or v.Name == "hidden_spectre6" then
                if v ~= game.Players.LocalPlayer and v.Character then
                    v.Character:Destroy()
                end
            end
        end
    end)
--// Main Libarys \\--
local libary = loadstring(game:HttpGet("https://raw.githubusercontent.com/imagoodpersond/puppyware/main/lib"))()
local NotifyLibrary = loadstring(game:HttpGet("https://raw.githubusercontent.com/imagoodpersond/puppyware/main/notify"))()
local Notify = NotifyLibrary.Notify
--// Service Handler \\--
local GetService = setmetatable({}, {
    __index = function(self, key)
        return game:GetService(key)
    end
})
--// Services \\--
local RunService = game:GetService("RunService")
local runservice = game:GetService("RunService")
local Players = GetService.Players
local LocalPlayer = game:GetService("Players").LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local CurrentCamera = workspace.CurrentCamera
local UserInputService = game:GetService("UserInputService")
local Unpack = table.unpack
local GuiInset = GetService.GuiService:GetGuiInset()
local Insert = table.insert
local Network = GetService.NetworkClient
local StarterGui = GetService.StarterGui
local tweenService = GetService.TweenService
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local http = GetService.HttpService
local lighting = game.Lighting

--// Start \\--


local Window = libary:new({
    name = "GodWare: Premium - Aftermath",
    accent = Color3.fromRGB(244, 95, 115),
    textsize = 13
})
local AimingTab = Window:page({
    name = "Legit"
})
local RageTab = Window:page({
    name = "Blatant"
})
local VisualTab = Window:page({
    name = "Visuals"
})
local MiscTab = Window:page({
    name = "Misc"
})

local SAimSection = AimingTab:section({
    name = "Gun Miscallenous",
    side = "left",
    size = 450
})

local VisualModifications = AimingTab:section({
    name = "Visual Modifications",
    side = "right",
    size = 320
})

local Modifyesp = VisualTab:section({
    name = "Customize",
    side = "right",
    size = 320
})

local AAMainSection = RageTab:section({
    name = "Main",
    side = "left",
    size = 400
})

local AAMainSection2 = RageTab:section({
    name = "Aim",
    side = "right",
    size = 250
})

local Fullplresp = VisualTab:section({
    name = "Player ESP",
    side = "left",
    size = 320
})

local VisualMainSection = VisualTab:section({
    name = "Miscallenous ESP",
    side = "left",
    size = 200
})

local MiscMoveSettings = MiscTab:section({
    name = "Extra",
    side = "left",
    size = 200
})



--bullet tracer
local workspace = game:GetService("Workspace")
local tracerConnections = {}

local function createSegment(startPos, endPos)
    local segment = Instance.new("Part")
    segment.Anchored = true
    segment.CanCollide = false
    segment.Color = Color3.fromRGB(255, 0, 0)
    segment.Material = Enum.Material.Neon
    segment.Transparency = 0.6
    segment.Size = Vector3.new(0.2, 0.2, (startPos - endPos).Magnitude)
    segment.CFrame = CFrame.new((startPos + endPos) / 2, endPos)
    segment.Parent = workspace
    game:GetService("Debris"):AddItem(segment, 3)

    local fadeConnection
    local lifetime, fadeTime, elapsed = 3, 2, 0

    fadeConnection = RunService.Heartbeat:Connect(function(deltaTime)
        if segment and segment.Parent then
            elapsed = elapsed + deltaTime
            segment.Transparency = math.min(1, segment.Transparency + (deltaTime / fadeTime))
            if elapsed >= lifetime then fadeConnection:Disconnect() end
        else
            fadeConnection:Disconnect()
        end
    end)
end

local function followTracer(tracerPart)
    local lastPosition = tracerPart.Position
    local connection

    connection = RunService.Heartbeat:Connect(function()
        if tracerPart and tracerPart.Parent then
            local currentPosition = tracerPart.Position
            createSegment(lastPosition, currentPosition)
            lastPosition = currentPosition
        else
            connection:Disconnect()
        end
    end)
end

SAimSection:toggle({
    name = "Bullet Tracers",
    def = false,
    callback = function(Boolean)
        if Boolean then
            tracerConnections["childAdded"] = workspace.ChildAdded:Connect(function(child)
                if child.Name == "Tracer" and child:IsA("Part") then
                    followTracer(child)
                end
            end)

            for _, child in ipairs(workspace:GetChildren()) do
                if child.Name == "Tracer" and child:IsA("Part") then
                    followTracer(child)
                end
            end
        else
            if tracerConnections["childAdded"] then
                tracerConnections["childAdded"]:Disconnect()
                tracerConnections["childAdded"] = nil
            end

            for _, part in ipairs(workspace:GetChildren()) do
                if part.Name == "Tracer" and part:IsA("Part") then
                    part:Destroy()
                end
            end
        end
    end
})

--aim checker
local aimCheckerActive = false
local aimCheckerDistance = 200
local aimingNotified = {}
local renderConnection

local function isWithinDistance(player)
    local character = player.Character
    if character and character:FindFirstChild("HumanoidRootPart") then
        local distance = (character.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude
        return distance <= aimCheckerDistance
    end
    return false
end

local function notifyAiming(player)
    game.StarterGui:SetCore("SendNotification", {
        Title = "Aim Alert",
        Text = player.Name .. " started aiming!",
        Duration = 10
    })
end

local function startAimChecker()
    renderConnection = game:GetService("RunService").RenderStepped:Connect(function()
        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer and isWithinDistance(player) then
                local aiming = player:FindFirstChild("Aiming")
                if aiming and aiming.Value == true then
                    if not aimingNotified[player] then
                        notifyAiming(player)
                        aimingNotified[player] = true
                    end
                elseif aiming and aiming.Value == false then
                    aimingNotified[player] = nil
                end
            end
        end
    end)
end

local function stopAimChecker()
    if renderConnection then
        renderConnection:Disconnect()
        renderConnection = nil
    end
end

game.Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function(character)
        if aimCheckerActive then
            local aiming = player:FindFirstChild("Aiming")
            if aiming and aiming.Value == true then
                notifyAiming(player)
                aimingNotified[player] = true
            end
        end
    end)
end)

game.Players.PlayerRemoving:Connect(function(player)
    aimingNotified[player] = nil
end)

game.Workspace.ChildRemoved:Connect(function(child)
    if child:IsA("Model") then
        local player = game.Players:GetPlayerFromCharacter(child)
        if player then
            aimingNotified[player] = nil
        end
    end
end)

game.Players.LocalPlayer.CharacterAdded:Connect(function(character)
    if aimCheckerActive then
        startAimChecker()
    end
end)

SAimSection:toggle({
    name = "Aim Checker",
    def = false,
    callback = function(state)
        aimCheckerActive = state
        aimingNotified = {}
        if aimCheckerActive then
            startAimChecker()
        else
            stopAimChecker()
        end
    end
})

SAimSection:slider({
    name = "Aim Checker Distance", 
    def = 200, 
    max = 1000, 
    min = 20,
    rounding = true,
    callback = function(value)
        aimCheckerDistance = value
    end
})


VisualModifications:toggle({
    name = "Remove Green Gas",
    def = false,
    callback = function(Boolean)
        if Boolean then
            if game.Workspace.world_assets.StaticObjects.Misc.TOSIC:FindFirstChild("Model") then
                game.Workspace.world_assets.StaticObjects.Misc.TOSIC.Model.Parent = game.StarterPack
            for i,v in pairs(game.Workspace.world_assets.StaticObjects.Misc.TOSIC:GetChildren()) do
                v.Parent = game.StarterPack
            end
        end
        else
            if game.StarterPack:FindFirstChild("Model") then
            game.StarterPack.Model.Parent = game.Workspace.world_assets.StaticObjects.Misc.TOSIC
            for i,v in pairs(game.StarterPack:GetChildren()) do
                v.Parent = game.Workspace.world_assets.StaticObjects.Misc.TOSIC
            end
            end
        end
    end
})


VisualModifications:button({name = "Cyan Environment", callback = function()
    game:GetService("Lighting").ColorCorrection.TintColor = Color3.fromRGB(0, 255, 240)
end})

VisualModifications:button({name = "Green Environment", callback = function()
    game:GetService("Lighting").ColorCorrection.TintColor = Color3.fromRGB(0, 255, 38)
end})

VisualModifications:button({name = "Red Environment", callback = function()
    game:GetService("Lighting").ColorCorrection.TintColor = Color3.fromRGB(255, 0, 0)
end})

VisualModifications:button({name = "Purple Environment", callback = function()
    game:GetService("Lighting").ColorCorrection.TintColor = Color3.fromRGB(132, 0, 118)
end})

VisualModifications:button({name = "Yellow Environment", callback = function()
    game:GetService("Lighting").ColorCorrection.TintColor = Color3.fromRGB(200, 194, 0)
end})

VisualModifications:button({name = "Pink Environment", callback = function()
    game:GetService("Lighting").ColorCorrection.TintColor = Color3.fromRGB(255, 0, 249)
end})

VisualModifications:button({name = "Dark Blue Environment", callback = function()
    game:GetService("Lighting").ColorCorrection.TintColor = Color3.fromRGB(0, 4, 255)
end})

VisualModifications:button({name = "Normal Environment", callback = function()
    game:GetService("Lighting").ColorCorrection.TintColor = Color3.fromRGB(200, 200, 200)
end})

local notreesconnection

VisualModifications:toggle({
    name = "No Tree",
    def = false,
    callback = function(Boolean)
        if Boolean then
            local notreefolder = Instance.new("Folder")
            notreefolder.Name = "trees"
            notreefolder.Parent = game.SoundService

            for i, v in pairs(workspace.world_assets.StaticObjects.Trees:GetChildren()) do
                v.Parent = game.SoundService.trees
            end

            notreesconnection = workspace.world_assets.StaticObjects.Trees.ChildAdded:Connect(function(child)
                delay(2, function()
                    if child and child:IsA("Model") then
                        if game.SoundService:FindFirstChild("trees") then
                        child.Parent = game.SoundService.trees
                        end
                    end
                end)
            end)
        else
            if notreesconnection then
                notreesconnection:Disconnect()
                notreesconnection = nil
            end

            if game.SoundService:FindFirstChild("trees") then
            for i, v in pairs(game.SoundService.trees:GetChildren()) do
                v.Parent = workspace.world_assets.StaticObjects.Trees
            end
        end

            game.SoundService.trees:Destroy()
        end
    end
})


AAMainSection:dropdown({
    name = "Gun Scanner", 
    def = "Barret", 
    max = 4, 
    options = {"Barret50","MosinNagant","M249","MK47","DesertEagle","DesertEagleGold","M40A1","AWM","SCAR","SVD","MRAD","AKM","PKM","Saiga","SPAS12"}, 
    callback = function(selectedGunName)
        local foundGuns = {}

        for _, player in pairs(game:GetService("Players"):GetPlayers()) do
            if player:FindFirstChild("GunInventory") then
                local gunInventory = player.GunInventory
                for i = 0, 4 do
                    local slot = gunInventory:FindFirstChild("Slot"..i)
                    if slot and slot.Value and slot.Value.Name == selectedGunName then
                        table.insert(foundGuns, player.Name.." has "..selectedGunName)
                    end
                end
            end
        end
        if #foundGuns == 0 then
            game:GetService("StarterGui"):SetCore("SendNotification", {
                Title = "Gun Check",
                Text = "Gun couldn't be found.",
                Duration = 20,
            })
        else
            for _, message in ipairs(foundGuns) do
                game:GetService("StarterGui"):SetCore("SendNotification", {
                    Title = "Gun Check",
                    Text = message,
                    Duration = 20,
                })
            end
        end
    end
})

--prediction
local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local lp = Players.LocalPlayer
local runservice = game:GetService("RunService")


local dwCamera = workspace.CurrentCamera
local dwRunService = game:GetService("RunService")
local dwUIS = game:GetService("UserInputService")
local dwEntities = game:GetService("Players")
local dwLocalPlayer = dwEntities.LocalPlayer
local dwMouse = dwLocalPlayer:GetMouse()

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local RunService = game:GetService("RunService")
local Camera = game:GetService("Workspace").CurrentCamera

local setting = {
    Aimbot = true,
    Aiming = false,
    Aimbot_AimPart = "Head",
    Aimbot_Draw_FOV = false,
    Aimbot_FOV_Color = Color3.fromRGB(255, 255, 255),
    Aimbot_Smoothness = 1,
    Aimbot_LockMode = 2,
    Aimbot_Sensitivity = 1.5,
    Aimbot_FOV_Radius = 250,
    sides = 80,
    rainbow = true,
    offset = Vector2.new(0, 40),
    enabled = true,
}

local tau = math.pi * 2
local drawings = {}

for i = 1, setting.sides do
    drawings[i] = Drawing.new('Line')
    drawings[i].ZIndex = 1
    drawings[i].Thickness = 4
end

RunService.RenderStepped:Connect(function()
    if setting.Aimbot_Draw_FOV == true then
        local mouseX, mouseY = Mouse.X, Mouse.Y
        local Aimbot_FOV_Radius = setting.Aimbot_FOV_Radius

        for i = 1, #drawings do
            local line = drawings[i]

            local color = setting.rainbow and Color3.fromHSV((tick() % 5 / 5 - (i / #drawings)) % 1, 0.5, 1) or setting.color
            local pos = Vector2.new(mouseX, mouseY) + setting.offset
            local last, next = ((i - 2) / setting.sides) * tau, ((i + 1) / setting.sides) * tau
            local lastX = pos.X + math.cos(last) * Aimbot_FOV_Radius
            local lastY = pos.Y + math.sin(last) * Aimbot_FOV_Radius
            local nextX = pos.X + math.cos(next) * Aimbot_FOV_Radius
            local nextY = pos.Y + math.sin(next) * Aimbot_FOV_Radius

            line.From = Vector2.new(lastX, lastY)
            line.To = Vector2.new(nextX, nextY)
            line.Color = color
            line.Visible = true
        end
    else
        for _, line in ipairs(drawings) do
            line.Visible = false
        end
    end
end)

dwUIS.InputBegan:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton2 then
        setting.Aiming = true
    end
end)

dwUIS.InputEnded:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton2 then
        setting.Aiming = false
    end
end)

local weaponBulletSpeeds = {
    ["M4A1"] = 2147,
    ["AK47"] = 2150,
    ["AKM"] = 2148,
    ["PKM"] = 1900,
    ["SCAR"] = 1995,
    ["SKS"] = 2145,
    ["AR15"] = 2145,
    ["FAL"] = 2145,
    ["Famas"] = 2145,
    ["M249"] = 1900,
    ["AWM"] = 3342,
    ["Barret50"] = 3336,
    ["M40A1"] = 2170,
    ["HuntingRifle"] = 3050,
    ["MosinNagant"] = 2127,
    ["SVD"] = 2850,
    ["MRAD"] = 3332,
    ["Sporter"] = 1638.530534,
    ["FNX45"] = 1456,
    ["RevolverNew"] = 1410,
    ["MK4"] = 1500,
    ["M1911"] = 1435,
    ["P226"] = 1450,
    ["M9"] = 1450,
    ["Makarov"] = 1550,
    ["UZI"] = 1705,
    ["DesertEagle"] = 1445,
    ["UMP45"] = 1850,
    ["MP5"] = 1750,
    ["DoubleBarrelShotgun"] = 3400,
    ["Shotgun"] = 3400,
    ["Mossberg"] = 3400,
}

local function getWeaponNamesFromSlots(player)
    local weaponNames = {"none", "none", "none", "none"}
    local inventory = player:FindFirstChild("GunInventory")
    if inventory then
        for i = 1, 4 do
            local slot = inventory:FindFirstChild("Slot" .. i)
            if slot then
                weaponNames[i] = tostring(slot.Value)
            end
        end
    end
    return weaponNames
end

local function getCurrentSlot(player)
    local currentSelectedObject = player:FindFirstChild("CurrentSelectedObject")
    if currentSelectedObject and currentSelectedObject.Value then
        local selectedObjectName = currentSelectedObject.Value.Name
        if selectedObjectName then
            local slotNumber = tonumber(selectedObjectName:match("%d+"))
            return slotNumber
        end
    end
    return nil
end

local function getBulletSpeedForCurrentWeapon(player)
    local weaponNames = getWeaponNamesFromSlots(player)
    local currentSlot = getCurrentSlot(player)

    if currentSlot then
        local currentWeapon = weaponNames[currentSlot]
        local bulletSpeed = weaponBulletSpeeds[currentWeapon]
        if bulletSpeed then
            return bulletSpeed
        else
            return weaponBulletSpeeds["M4A1"]
        end
    else
        return weaponBulletSpeeds["M4A1"]
    end
end

local BULLET_GRAVITY = 100

local function PredictTargetPosition(targetPart, targetVelocity, distance)
    local bulletSpeed = getBulletSpeedForCurrentWeapon(dwLocalPlayer)
    local timeToReach = distance / bulletSpeed

    local horizontalPrediction = targetPart.Position + (targetVelocity * timeToReach)
    local gravityDisplacement = Vector3.new(0, 0.5 * BULLET_GRAVITY * (timeToReach ^ 2), 0)
    return horizontalPrediction + gravityDisplacement
end

dwRunService.RenderStepped:Connect(function()
    if not dwCamera then
        dwCamera = workspace.CurrentCamera
        return
    end

    if not setting.Aimbot then return end

    local dist = math.huge
    local closest_char = nil

    if setting.Aiming then
        for _, v in pairs(dwEntities:GetPlayers()) do
            if v ~= dwLocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                local char = v.Character:FindFirstChild("WorldCharacter")
                if char and char:FindFirstChild(setting.Aimbot_AimPart) then
                    local charPart = char[setting.Aimbot_AimPart]
                    local charVelocity = v.Character.HumanoidRootPart.Velocity
                    local charPartPos, is_onscreen = dwCamera:WorldToViewportPoint(charPart.Position)

                    if is_onscreen then
                        local mag = (Vector2.new(dwMouse.X, dwMouse.Y) - Vector2.new(charPartPos.X, charPartPos.Y)).Magnitude

                        if mag < dist and mag < setting.Aimbot_FOV_Radius then
                            dist = mag
                            closest_char = char
                        end
                    end
                end
            end
        end

        if closest_char and closest_char:FindFirstChild(setting.Aimbot_AimPart) then
            local targetPart = closest_char[setting.Aimbot_AimPart]
            local targetVelocity = closest_char.Parent.HumanoidRootPart.Velocity
            local distanceToTarget = (targetPart.Position - dwLocalPlayer.Character.HumanoidRootPart.Position).Magnitude
            local predictedPosition = PredictTargetPosition(targetPart, targetVelocity, distanceToTarget)

            local target_position = dwCamera:WorldToViewportPoint(predictedPosition)
            local center = Vector2.new(dwCamera.ViewportSize.X / 2, dwCamera.ViewportSize.Y / 2)
            local delta = Vector2.new(target_position.X - center.X, target_position.Y - center.Y)

            if setting.Aimbot_LockMode == 2 then
                local sensitivity = setting.Aimbot_Sensitivity
                mousemoverel(delta.X * sensitivity, delta.Y * sensitivity)
            else
                local smooth_factor = setting.Aimbot_Smoothness
                local new_position = center + delta * smooth_factor
                local look_at_position = dwCamera.CFrame.Position + Vector3.new(delta.X / dwCamera.ViewportSize.X, delta.Y / dwCamera.ViewportSize.Y, 0)
                local target_cframe = CFrame.new(dwCamera.CFrame.Position, look_at_position)
                dwCamera.CFrame = dwCamera.CFrame:Lerp(target_cframe, smooth_factor)
            end
        end
    end
end)



AAMainSection2:toggle({
    name = "Aimbot (make aim sens 0.5)",
    def = false,
    callback = function(bool)
        setting.Aimbot = bool
        setting.Aimbot_Draw_FOV = bool
    end
})

AAMainSection2:slider({
    name = "Aimbot FOV Size",
    def = setting.AimbotFOVRadius,
    max = 300,
    min = 10,
    rounding = true,
    callback = function(bool)
        setting.Aimbot_FOV_Radius = bool
    end
})

if identifyexecutor() == "Solara" then
else
AAMainSection2:button({name = "No Recoil", callback = function()
    for _, v in next, getgc(true) do
        if type(v) == 'table' then
            for i, val in next, v do
                if type(i) == 'string' and string.find(i, 'Recoil') then
                    if type(val) == 'number' then
                        v[i] = 0
                    elseif type(val) == 'function' then
                        hookfunction(val, function(...)
                            return 0
                        end)
                    end
                end
            end
        end
    end  
end})

AAMainSection2:button({name = "Instant Aim", callback = function()
    for _, v in next, getgc(true) do
        if type(v) == 'table' then
            for i, val in next, v do
                if type(i) == 'string' and string.find(i, 'GunAim') then
                    if type(val) == 'number' then
                        v[i] = 100000000
                    elseif type(val) == 'function' then
                        hookfunction(val, function(...)
                            return 100000000
                        end)
                    end
                end
            end
        end
    end  
end})
end


--Visual Sections
local camera = workspace.CurrentCamera
local Players = game:GetService("Players")

local vehicleESPObjects = {}
local espEnabledVehicle = false
local espEnabledDistance = false

local renderSteppedConnection, heartbeatConnection
local childAddedConnection

local function createVehicleESP(chassis)
    local vehicleESP = Drawing.new("Text")
    vehicleESP.Visible = false
    vehicleESP.Center = true
    vehicleESP.Outline = false
    vehicleESP.Font = 0
    vehicleESP.Color = Color3.fromRGB(255, 255, 0)
    vehicleESP.Size = 10

    vehicleESPObjects[chassis] = vehicleESP

    return vehicleESP
end

local function updateVehicleESP()
    for chassis, vehicleESP in pairs(vehicleESPObjects) do
        if chassis and chassis:IsDescendantOf(workspace) then
            local pos = chassis.Position
            local chassis_pos, chassis_onscreen = camera:WorldToViewportPoint(pos)

            if chassis_onscreen then
                local distance = (pos - LocalPlayer.Character.PrimaryPart.Position).Magnitude
                vehicleESP.Position = Vector2.new(chassis_pos.X, chassis_pos.Y)
                vehicleESP.Text = espEnabledDistance and "Vehicle (" .. math.floor(distance) .. " studs)" or "Vehicle"
                vehicleESP.Visible = true
            else
                vehicleESP.Visible = false
            end
        else
            vehicleESP.Visible = false
        end
    end
end

local function checkVehicles()
    for chassis, vehicleESP in pairs(vehicleESPObjects) do
        if not chassis or not chassis:IsDescendantOf(workspace) then
            vehicleESP:Remove()
            vehicleESPObjects[chassis] = nil
        end
    end

    for _, worldModel in pairs(workspace:GetChildren()) do
        if worldModel:IsA("Model") and worldModel:FindFirstChild("Chassis") then
            if not vehicleESPObjects[worldModel.Chassis] then
                createVehicleESP(worldModel.Chassis)
            end
        end
    end
end

checkVehicles()

local function enableESP()
    if renderSteppedConnection then return end

    renderSteppedConnection = runservice.RenderStepped:Connect(function()
        updateVehicleESP()
    end)

    if heartbeatConnection then return end

    heartbeatConnection = runservice.Heartbeat:Connect(function()
        checkVehicles()
    end)

    if not childAddedConnection then
        childAddedConnection = workspace.ChildAdded:Connect(function(child)
            if espEnabledVehicle and child:IsA("Model") and child:FindFirstChild("Chassis") then
                createVehicleESP(child.Chassis)
            end
        end)
    end
end

local function disableESP()
    if renderSteppedConnection then
        renderSteppedConnection:Disconnect()
        renderSteppedConnection = nil
    end
    if heartbeatConnection then
        heartbeatConnection:Disconnect()
        heartbeatConnection = nil
    end
    if childAddedConnection then
        childAddedConnection:Disconnect()
        childAddedConnection = nil
    end

    for _, vehicleESP in pairs(vehicleESPObjects) do
        vehicleESP:Remove()
    end
    vehicleESPObjects = {}
end

local function updateESPState()
    if espEnabledVehicle or espEnabledDistance then
        enableESP()
    else
        disableESP()
    end
end

VisualMainSection:toggle({
    name = "Vehicle ESP",
    def = false,
    callback = function(Boolean)
        espEnabledVehicle = Boolean
        updateESPState()
    end
})

Modifyesp:toggle({
    name = "Vehicle ESP Distance",
    def = false,
    callback = function(Boolean)
        espEnabledDistance = Boolean
        updateESPState()
    end
})

--lootbag esp
local camera = workspace.CurrentCamera
local Players = game:GetService("Players")

local lootbagESPObjects = {}
local espEnabledLootbag = false
local espEnabledLootbagDistance = false

local renderSteppedConnection
local childAddedConnection
local childRemovedConnection

local function createLootbagESP(lootbag)
    local lootbagESP = Drawing.new("Text")
    lootbagESP.Visible = false
    lootbagESP.Center = true
    lootbagESP.Font = 0
    lootbagESP.Color = Color3.fromRGB(255, 182, 193)
    lootbagESP.Size = 6
    lootbagESP.Outline = false

    lootbagESPObjects[lootbag] = lootbagESP

    lootbag.AncestryChanged:Connect(function(_, parent)
        if not parent then
            if lootbagESPObjects[lootbag] then
                lootbagESP:Remove()
                lootbagESPObjects[lootbag] = nil
            end
        end
    end)

    return lootbagESP
end

local function updateLootbagESP()
    if not espEnabledLootbag then
        return
    end

    local playerPos = LocalPlayer.Character.PrimaryPart.Position

    for lootbag, lootbagESP in pairs(lootbagESPObjects) do
        if lootbag and lootbag:IsDescendantOf(workspace) then
            local lootbagChildren = lootbag:GetChildren()
            for _, child in ipairs(lootbagChildren) do
                if child:IsA("BasePart") then
                    local pos = child.Position
                    local lootbag_pos, lootbag_onscreen = camera:WorldToViewportPoint(pos)

                    if lootbag_onscreen then
                        local distance = (pos - playerPos).Magnitude
                        lootbagESP.Position = Vector2.new(lootbag_pos.X, lootbag_pos.Y)

                        lootbagESP.Text = espEnabledLootbagDistance and 
                            ("Lootbag (" .. math.floor(distance) .. " studs)") or 
                            "Lootbag"

                        lootbagESP.Visible = true
                    else
                        lootbagESP.Visible = false
                    end
                    break
                end
            end
        else
            lootbagESP.Visible = false
        end
    end
end

local function checkLootbags()
    for lootbag, lootbagESP in pairs(lootbagESPObjects) do
        if lootbagESP then
            lootbagESP:Remove()
        end
    end
    lootbagESPObjects = {}

    for _, item in pairs(workspace:GetChildren()) do
        if item:IsA("Model") and item.Name == "Default" then
            createLootbagESP(item)
        end
    end
end

local function enableLootbagESP()
    if renderSteppedConnection then return end

    renderSteppedConnection = runservice.RenderStepped:Connect(function()
        updateLootbagESP()
    end)

    childAddedConnection = workspace.ChildAdded:Connect(function(child)
        if espEnabledLootbag and child:IsA("Model") and child.Name == "Default" then
            createLootbagESP(child)
        end
    end)

    childRemovedConnection = workspace.ChildRemoved:Connect(function(child)
        if lootbagESPObjects[child] then
            lootbagESPObjects[child]:Remove()
            lootbagESPObjects[child] = nil
        end
    end)
end

local function disableLootbagESP()
    if renderSteppedConnection then
        renderSteppedConnection:Disconnect()
        renderSteppedConnection = nil
    end
    if childAddedConnection then
        childAddedConnection:Disconnect()
        childAddedConnection = nil
    end
    if childRemovedConnection then
        childRemovedConnection:Disconnect()
        childRemovedConnection = nil
    end

    for _, lootbagESP in pairs(lootbagESPObjects) do
        if lootbagESP then
            lootbagESP:Remove()
        end
    end
    lootbagESPObjects = {}
end

local function updateLootbagESPState()
    if espEnabledLootbag then
        checkLootbags()
        enableLootbagESP()
    else
        disableLootbagESP()
    end
end

VisualMainSection:toggle({
    name = "Lootbag ESP",
    def = false,
    callback = function(Boolean)
        espEnabledLootbag = Boolean
        updateLootbagESPState()
    end
})

Modifyesp:toggle({
    name = "Lootbag ESP Distance",
    def = false,
    callback = function(Boolean)
        if espEnabledLootbag then
            espEnabledLootbagDistance = Boolean
            updateLootbagESP()
        else
            espEnabledLootbagDistance = false
        end
    end
})

checkLootbags()




--inv viewer
local osACKZXcsaOCXZMCDKAosad = Instance.new("ScreenGui")
local TSPc0CZasgadsga = Instance.new("Frame")
local EDPZXCBJKXMzfsgdgsgsdx = Instance.new("UICorner")
local EQWFASZFKgjksdagkasg = Instance.new("TextLabel")
local fkaskfqfkaskfl_vas = Instance.new("TextLabel")
local kfsafkafalczxcsqakfosaf = Instance.new("TextLabel")
local _z_cds_ad___fas = Instance.new("TextLabel")
local fsajkfakskasf = Instance.new("TextLabel")
local RRIOFASOCROFSAF = Instance.new("Frame")
local CPXZKFWAQAfalgfa = Instance.new("UICorner")
local _RLSAFL_ASGOagka = Instance.new("TextLabel")
local FKASKFakfgkafgaclzxlcqpowpasf = Instance.new("TextLabel")
local ECSZC_ASF__ = Instance.new("TextLabel")
local RFLOASFAKFLLLRLKSAKF = Instance.new("TextLabel")
local kgasg2r421qqdkasfafa = Instance.new("TextLabel")
local OFDSKAFKLOLOWMSAF = Instance.new("TextLabel")
local FKSAKFKAZKCXLc = Instance.new("TextLabel")
local orkfdsakasfkvzxasf = Instance.new("TextLabel")
local FKASOGFAKJOGFAOZXC_trkaskfa12401R = Instance.new("TextLabel")
local r_saofowqkr = Instance.new("TextLabel")
local _RFSAKFA_OR = Instance.new("TextLabel")
local FOsalasdDwqafaKSGa = Instance.new("UIStroke")
local kfasdaskfzxvMFA = Instance.new("UIStroke")

-- Properties

kfasdaskfzxvMFA.Parent = RRIOFASOCROFSAF
kfasdaskfzxvMFA.Color = Color3.fromRGB(128, 0, 255)
kfasdaskfzxvMFA.Thickness = 3.3
kfasdaskfzxvMFA.Transparency = 0.5
kfasdaskfzxvMFA.LineJoinMode = Enum.LineJoinMode.Round
kfasdaskfzxvMFA.ApplyStrokeMode = Enum.ApplyStrokeMode.Contextual

FOsalasdDwqafaKSGa.Parent = TSPc0CZasgadsga
FOsalasdDwqafaKSGa.Color = Color3.fromRGB(128, 0, 255)
FOsalasdDwqafaKSGa.Thickness = 3.3
FOsalasdDwqafaKSGa.Transparency = 0.5
FOsalasdDwqafaKSGa.LineJoinMode = Enum.LineJoinMode.Round
FOsalasdDwqafaKSGa.ApplyStrokeMode = Enum.ApplyStrokeMode.Contextual

osACKZXcsaOCXZMCDKAosad.Name = "?!'+=osACKZX'?=!+csaOCXZMCDKA!êo)=sad"
osACKZXcsaOCXZMCDKAosad.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
osACKZXcsaOCXZMCDKAosad.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

TSPc0CZasgadsga.Name = "!?'+=!%T)=SPc0>'!?+CZasgadsga"
TSPc0CZasgadsga.Parent = osACKZXcsaOCXZMCDKAosad
TSPc0CZasgadsga.BackgroundColor3 = Color3.new(0.0352941, 0, 0.0862745)
TSPc0CZasgadsga.BorderColor3 = Color3.new(0, 0, 0)
TSPc0CZasgadsga.BorderSizePixel = 0
TSPc0CZasgadsga.Position = UDim2.new(0.695458233, 0, 0.752781689, 0)
TSPc0CZasgadsga.Size = UDim2.new(0, 146, 0, 142)

EDPZXCBJKXMzfsgdgsgsdx.Name = "?'!+%=!=EDPZ=XC='=+=!=BJKXMzfsgdgsgsdx"
EDPZXCBJKXMzfsgdgsgsdx.Parent = TSPc0CZasgadsga

EQWFASZFKgjksdagkasg.Name = ")!)+!'=EQWF?ASZ)F!'=+!Kgjksdagkasg"
EQWFASZFKgjksdagkasg.Parent = TSPc0CZasgadsga
EQWFASZFKgjksdagkasg.BackgroundColor3 = Color3.new(1.56863, 1.56863, 1.56863)
EQWFASZFKgjksdagkasg.BackgroundTransparency = 1
EQWFASZFKgjksdagkasg.BorderColor3 = Color3.new(0, 0, 0)
EQWFASZFKgjksdagkasg.BorderSizePixel = 0
EQWFASZFKgjksdagkasg.Size = UDim2.new(0, 146, 0, 28)
EQWFASZFKgjksdagkasg.Font = Enum.Font.FredokaOne
EQWFASZFKgjksdagkasg.Text = "nil"
EQWFASZFKgjksdagkasg.TextColor3 = Color3.new(1, 1, 1)
EQWFASZFKgjksdagkasg.TextScaled = true
EQWFASZFKgjksdagkasg.TextSize = 14
EQWFASZFKgjksdagkasg.TextWrapped = true

fkaskfqfkaskfl_vas.Name = "?!'?+!?fkaskf!q'?=+!fkaskfl'?!+_vas"
fkaskfqfkaskfl_vas.Parent = TSPc0CZasgadsga
fkaskfqfkaskfl_vas.BackgroundColor3 = Color3.new(1, 1, 1)
fkaskfqfkaskfl_vas.BackgroundTransparency = 1
fkaskfqfkaskfl_vas.BorderColor3 = Color3.new(0, 0, 0)
fkaskfqfkaskfl_vas.BorderSizePixel = 0
fkaskfqfkaskfl_vas.Position = UDim2.new(0, 0, 0.274647892, 0)
fkaskfqfkaskfl_vas.Size = UDim2.new(0, 146, 0, 26)
fkaskfqfkaskfl_vas.Font = Enum.Font.SourceSans
fkaskfqfkaskfl_vas.Text = "nil"
fkaskfqfkaskfl_vas.TextColor3 = Color3.new(1, 1, 1)
fkaskfqfkaskfl_vas.TextScaled = true
fkaskfqfkaskfl_vas.TextSize = 14
fkaskfqfkaskfl_vas.TextWrapped = true

kfsafkafalczxcsqakfosaf.Name = "?'!?+kfsafkafalcözxcösqakfosaşf"
kfsafkafalczxcsqakfosaf.Parent = TSPc0CZasgadsga
kfsafkafalczxcsqakfosaf.BackgroundColor3 = Color3.new(1, 1, 1)
kfsafkafalczxcsqakfosaf.BackgroundTransparency = 1
kfsafkafalczxcsqakfosaf.BorderColor3 = Color3.new(0, 0, 0)
kfsafkafalczxcsqakfosaf.BorderSizePixel = 0
kfsafkafalczxcsqakfosaf.Position = UDim2.new(0, 0, 0.457746476, 0)
kfsafkafalczxcsqakfosaf.Size = UDim2.new(0, 146, 0, 26)
kfsafkafalczxcsqakfosaf.Font = Enum.Font.SourceSans
kfsafkafalczxcsqakfosaf.Text = "nil"
kfsafkafalczxcsqakfosaf.TextColor3 = Color3.new(1, 1, 1)
kfsafkafalczxcsqakfosaf.TextScaled = true
kfsafkafalczxcsqakfosaf.TextSize = 14
kfsafkafalczxcsqakfosaf.TextWrapped = true

_z_cds_ad___fas.Name = "='!?+!_z_cds_ad___'!?=fas"
_z_cds_ad___fas.Parent = TSPc0CZasgadsga
_z_cds_ad___fas.BackgroundColor3 = Color3.new(1, 1, 1)
_z_cds_ad___fas.BackgroundTransparency = 1
_z_cds_ad___fas.BorderColor3 = Color3.new(0, 0, 0)
_z_cds_ad___fas.BorderSizePixel = 0
_z_cds_ad___fas.Position = UDim2.new(0, 0, 0.633802593, 0)
_z_cds_ad___fas.Size = UDim2.new(0, 146, 0, 26)
_z_cds_ad___fas.Font = Enum.Font.SourceSans
_z_cds_ad___fas.Text = "nil"
_z_cds_ad___fas.TextColor3 = Color3.new(1, 1, 1)
_z_cds_ad___fas.TextScaled = true
_z_cds_ad___fas.TextSize = 14
_z_cds_ad___fas.TextWrapped = true

fsajkfakskasf.Name = "!'+!+?!=fsajkfaksk'!=+?asf"
fsajkfakskasf.Parent = TSPc0CZasgadsga
fsajkfakskasf.BackgroundColor3 = Color3.new(1, 1, 1)
fsajkfakskasf.BackgroundTransparency = 1
fsajkfakskasf.BorderColor3 = Color3.new(0, 0, 0)
fsajkfakskasf.BorderSizePixel = 0
fsajkfakskasf.Position = UDim2.new(0, 0, 0.816901207, 0)
fsajkfakskasf.Size = UDim2.new(0, 146, 0, 26)
fsajkfakskasf.Font = Enum.Font.SourceSans
fsajkfakskasf.Text = "nil"
fsajkfakskasf.TextColor3 = Color3.new(1, 1, 1)
fsajkfakskasf.TextScaled = true
fsajkfakskasf.TextSize = 14
fsajkfakskasf.TextWrapped = true

RRIOFASOCROFSAF.Name = "?=!'%R=)'!^RI)OFAS=OC>)=!'^+R!OFSAF"
RRIOFASOCROFSAF.Parent = osACKZXcsaOCXZMCDKAosad
RRIOFASOCROFSAF.BackgroundColor3 = Color3.new(0.0352941, 0, 0.0862745)
RRIOFASOCROFSAF.BorderColor3 = Color3.new(0, 0, 0)
RRIOFASOCROFSAF.BorderSizePixel = 0
RRIOFASOCROFSAF.Position = UDim2.new(0.851581514, 0, 0.752781689, 0)
RRIOFASOCROFSAF.Size = UDim2.new(0, 146, 0, 142)

CPXZKFWAQAfalgfa.Name = "?!'%=!=CP?X=ZKFWAQ?+!'Afalgfa"
CPXZKFWAQAfalgfa.Parent = RRIOFASOCROFSAF

_RLSAFL_ASGOagka.Name = "_!'+!?R=LSAFÖL?!'+_ASGOagka"
_RLSAFL_ASGOagka.Parent = RRIOFASOCROFSAF
_RLSAFL_ASGOagka.BackgroundColor3 = Color3.new(1.56863, 1.56863, 1.56863)
_RLSAFL_ASGOagka.BackgroundTransparency = 1
_RLSAFL_ASGOagka.BorderColor3 = Color3.new(0, 0, 0)
_RLSAFL_ASGOagka.BorderSizePixel = 0
_RLSAFL_ASGOagka.Size = UDim2.new(0, 73, 0, 39)
_RLSAFL_ASGOagka.Font = Enum.Font.FredokaOne
_RLSAFL_ASGOagka.Text = "Current Ammo"
_RLSAFL_ASGOagka.TextColor3 = Color3.new(1, 1, 1)
_RLSAFL_ASGOagka.TextSize = 17
_RLSAFL_ASGOagka.TextWrapped = true

FKASKFakfgkafgaclzxlcqpowpasf.Name = "FKASKFakfgkafgaclzxlcqpowpasf"
FKASKFakfgkafgaclzxlcqpowpasf.Parent = RRIOFASOCROFSAF
FKASKFakfgkafgaclzxlcqpowpasf.BackgroundColor3 = Color3.new(1, 1, 1)
FKASKFakfgkafgaclzxlcqpowpasf.BackgroundTransparency = 1
FKASKFakfgkafgaclzxlcqpowpasf.BorderColor3 = Color3.new(0, 0, 0)
FKASKFakfgkafgaclzxlcqpowpasf.BorderSizePixel = 0
FKASKFakfgkafgaclzxlcqpowpasf.Position = UDim2.new(0, 0, 0.274647892, 0)
FKASKFakfgkafgaclzxlcqpowpasf.Size = UDim2.new(0, 73, 0, 26)
FKASKFakfgkafgaclzxlcqpowpasf.Font = Enum.Font.SourceSans
FKASKFakfgkafgaclzxlcqpowpasf.Text = "nil"
FKASKFakfgkafgaclzxlcqpowpasf.TextColor3 = Color3.new(1, 1, 1)
FKASKFakfgkafgaclzxlcqpowpasf.TextScaled = true
FKASKFakfgkafgaclzxlcqpowpasf.TextSize = 14
FKASKFakfgkafgaclzxlcqpowpasf.TextWrapped = true

ECSZC_ASF__.Name = "?'!?=+=!=+!==E?CSZ?C_ASF__"
ECSZC_ASF__.Parent = RRIOFASOCROFSAF
ECSZC_ASF__.BackgroundColor3 = Color3.new(1, 1, 1)
ECSZC_ASF__.BackgroundTransparency = 1
ECSZC_ASF__.BorderColor3 = Color3.new(0, 0, 0)
ECSZC_ASF__.BorderSizePixel = 0
ECSZC_ASF__.Position = UDim2.new(0, 0, 0.457746476, 0)
ECSZC_ASF__.Size = UDim2.new(0, 73, 0, 26)
ECSZC_ASF__.Font = Enum.Font.SourceSans
ECSZC_ASF__.Text = "nil"
ECSZC_ASF__.TextColor3 = Color3.new(1, 1, 1)
ECSZC_ASF__.TextScaled = true
ECSZC_ASF__.TextSize = 14
ECSZC_ASF__.TextWrapped = true

RFLOASFAKFLLLRLKSAKF.Name = "'!+!=RFLOASFAKFLL!'LRLKSAKF"
RFLOASFAKFLLLRLKSAKF.Parent = RRIOFASOCROFSAF
RFLOASFAKFLLLRLKSAKF.BackgroundColor3 = Color3.new(1, 1, 1)
RFLOASFAKFLLLRLKSAKF.BackgroundTransparency = 1
RFLOASFAKFLLLRLKSAKF.BorderColor3 = Color3.new(0, 0, 0)
RFLOASFAKFLLLRLKSAKF.BorderSizePixel = 0
RFLOASFAKFLLLRLKSAKF.Position = UDim2.new(0, 0, 0.626760364, 0)
RFLOASFAKFLLLRLKSAKF.Size = UDim2.new(0, 73, 0, 26)
RFLOASFAKFLLLRLKSAKF.Font = Enum.Font.SourceSans
RFLOASFAKFLLLRLKSAKF.Text = "nil"
RFLOASFAKFLLLRLKSAKF.TextColor3 = Color3.new(1, 1, 1)
RFLOASFAKFLLLRLKSAKF.TextScaled = true
RFLOASFAKFLLLRLKSAKF.TextSize = 14
RFLOASFAKFLLLRLKSAKF.TextWrapped = true

kgasg2r421qqdkasfafa.Name = "kgasg2*r421q-q-dkasfafa"
kgasg2r421qqdkasfafa.Parent = RRIOFASOCROFSAF
kgasg2r421qqdkasfafa.BackgroundColor3 = Color3.new(1, 1, 1)
kgasg2r421qqdkasfafa.BackgroundTransparency = 1
kgasg2r421qqdkasfafa.BorderColor3 = Color3.new(0, 0, 0)
kgasg2r421qqdkasfafa.BorderSizePixel = 0
kgasg2r421qqdkasfafa.Position = UDim2.new(0.5, 0, 0.274647892, 0)
kgasg2r421qqdkasfafa.Size = UDim2.new(0, 73, 0, 26)
kgasg2r421qqdkasfafa.Font = Enum.Font.SourceSans
kgasg2r421qqdkasfafa.Text = "nil"
kgasg2r421qqdkasfafa.TextColor3 = Color3.new(1, 1, 1)
kgasg2r421qqdkasfafa.TextScaled = true
kgasg2r421qqdkasfafa.TextSize = 14
kgasg2r421qqdkasfafa.TextWrapped = true

OFDSKAFKLOLOWMSAF.Name = "!'?+%?!OFDSKAFÖKLO'!LO+WMSAF"
OFDSKAFKLOLOWMSAF.Parent = RRIOFASOCROFSAF
OFDSKAFKLOLOWMSAF.BackgroundColor3 = Color3.new(1, 1, 1)
OFDSKAFKLOLOWMSAF.BackgroundTransparency = 1
OFDSKAFKLOLOWMSAF.BorderColor3 = Color3.new(0, 0, 0)
OFDSKAFKLOLOWMSAF.BorderSizePixel = 0
OFDSKAFKLOLOWMSAF.Position = UDim2.new(0.5, 0, 0.450704217, 0)
OFDSKAFKLOLOWMSAF.Size = UDim2.new(0, 73, 0, 26)
OFDSKAFKLOLOWMSAF.Font = Enum.Font.SourceSans
OFDSKAFKLOLOWMSAF.Text = "nil"
OFDSKAFKLOLOWMSAF.TextColor3 = Color3.new(1, 1, 1)
OFDSKAFKLOLOWMSAF.TextScaled = true
OFDSKAFKLOLOWMSAF.TextSize = 14
OFDSKAFKLOLOWMSAF.TextWrapped = true

FKSAKFKAZKCXLc.Name = "'!?é+?!?FKSAKFKA!'+?=ZKÖCXÇLcç"
FKSAKFKAZKCXLc.Parent = RRIOFASOCROFSAF
FKSAKFKAZKCXLc.BackgroundColor3 = Color3.new(1, 1, 1)
FKSAKFKAZKCXLc.BackgroundTransparency = 1
FKSAKFKAZKCXLc.BorderColor3 = Color3.new(0, 0, 0)
FKSAKFKAZKCXLc.BorderSizePixel = 0
FKSAKFKAZKCXLc.Position = UDim2.new(0.5, 0, 0.633802593, 0)
FKSAKFKAZKCXLc.Size = UDim2.new(0, 73, 0, 26)
FKSAKFKAZKCXLc.Font = Enum.Font.SourceSans
FKSAKFKAZKCXLc.Text = "nil"
FKSAKFKAZKCXLc.TextColor3 = Color3.new(1, 1, 1)
FKSAKFKAZKCXLc.TextScaled = true
FKSAKFKAZKCXLc.TextSize = 14
FKSAKFKAZKCXLc.TextWrapped = true

orkfdsakasfkvzxasf.Name = "!ıorkfdsakasfkvözxööasf"
orkfdsakasfkvzxasf.Parent = RRIOFASOCROFSAF
orkfdsakasfkvzxasf.BackgroundColor3 = Color3.new(1.56863, 1.56863, 1.56863)
orkfdsakasfkvzxasf.BackgroundTransparency = 1
orkfdsakasfkvzxasf.BorderColor3 = Color3.new(0, 0, 0)
orkfdsakasfkvzxasf.BorderSizePixel = 0
orkfdsakasfkvzxasf.Position = UDim2.new(0.5, 0, 0, 0)
orkfdsakasfkvzxasf.Size = UDim2.new(0, 73, 0, 39)
orkfdsakasfkvzxasf.Font = Enum.Font.FredokaOne
orkfdsakasfkvzxasf.Text = "Stored Ammo"
orkfdsakasfkvzxasf.TextColor3 = Color3.new(1, 1, 1)
orkfdsakasfkvzxasf.TextSize = 17
orkfdsakasfkvzxasf.TextWrapped = true

FKASOGFAKJOGFAOZXC_trkaskfa12401R.Name = "FKASOGFAKJOGFAOZXC_'!'?+!?=trıkaskfa-12401*R"
FKASOGFAKJOGFAOZXC_trkaskfa12401R.Parent = RRIOFASOCROFSAF
FKASOGFAKJOGFAOZXC_trkaskfa12401R.BackgroundColor3 = Color3.new(1, 1, 1)
FKASOGFAKJOGFAOZXC_trkaskfa12401R.BackgroundTransparency = 1
FKASOGFAKJOGFAOZXC_trkaskfa12401R.BorderColor3 = Color3.new(0, 0, 0)
FKASOGFAKJOGFAOZXC_trkaskfa12401R.BorderSizePixel = 0
FKASOGFAKJOGFAOZXC_trkaskfa12401R.Position = UDim2.new(0.33561644, 0, 0.274647892, 0)
FKASOGFAKJOGFAOZXC_trkaskfa12401R.Size = UDim2.new(0, 47, 0, 26)
FKASOGFAKJOGFAOZXC_trkaskfa12401R.Font = Enum.Font.SourceSans
FKASOGFAKJOGFAOZXC_trkaskfa12401R.Text = "/"
FKASOGFAKJOGFAOZXC_trkaskfa12401R.TextColor3 = Color3.new(1, 1, 1)
FKASOGFAKJOGFAOZXC_trkaskfa12401R.TextScaled = true
FKASOGFAKJOGFAOZXC_trkaskfa12401R.TextSize = 14
FKASOGFAKJOGFAOZXC_trkaskfa12401R.TextWrapped = true

r_saofowqkr.Name = "!?'+?!r_?sa)of=?é!)+owqıkr"
r_saofowqkr.Parent = RRIOFASOCROFSAF
r_saofowqkr.BackgroundColor3 = Color3.new(1, 1, 1)
r_saofowqkr.BackgroundTransparency = 1
r_saofowqkr.BorderColor3 = Color3.new(0, 0, 0)
r_saofowqkr.BorderSizePixel = 0
r_saofowqkr.Position = UDim2.new(0.33561644, 0, 0.457746476, 0)
r_saofowqkr.Size = UDim2.new(0, 47, 0, 26)
r_saofowqkr.Font = Enum.Font.SourceSans
r_saofowqkr.Text = "/"
r_saofowqkr.TextColor3 = Color3.new(1, 1, 1)
r_saofowqkr.TextScaled = true
r_saofowqkr.TextSize = 14
r_saofowqkr.TextWrapped = true

_RFSAKFA_OR.Name = "_!'=+!RFSAKFA?_!'+!)O=R"
_RFSAKFA_OR.Parent = RRIOFASOCROFSAF
_RFSAKFA_OR.BackgroundColor3 = Color3.new(1, 1, 1)
_RFSAKFA_OR.BackgroundTransparency = 1
_RFSAKFA_OR.BorderColor3 = Color3.new(0, 0, 0)
_RFSAKFA_OR.BorderSizePixel = 0
_RFSAKFA_OR.Position = UDim2.new(0.33561644, 0, 0.626760364, 0)
_RFSAKFA_OR.Size = UDim2.new(0, 47, 0, 26)
_RFSAKFA_OR.Font = Enum.Font.SourceSans
_RFSAKFA_OR.Text = "/"
_RFSAKFA_OR.TextColor3 = Color3.new(1, 1, 1)
_RFSAKFA_OR.TextScaled = true
_RFSAKFA_OR.TextSize = 14
_RFSAKFA_OR.TextWrapped = true

local gui = game:GetService("Players").LocalPlayer.PlayerGui["?!'+=osACKZX'?=!+csaOCXZMCDKA!êo)=sad"]
gui.ResetOnSpawn = false
local background = gui["!?'+=!%T)=SPc0>'!?+CZasgadsga"]
local background2 = gui["?=!'%R=)'!^RI)OFAS=OC>)=!'^+R!OFSAF"]
background.Visible = false
background2.Visible = false
background.Draggable = true
background2.Draggable = true
background.Active = true
background2.Active = true
background.Selectable = true
background2.Selectable = true

local playernameinvviewer = background[")!)+!'=EQWF?ASZ)F!'=+!Kgjksdagkasg"]

local gunvalue1 = background["?!'?+!?fkaskf!q'?=+!fkaskfl'?!+_vas"]
local gunvalue2 = background["?'!?+kfsafkafalcözxcösqakfosaşf"]
local gunvalue3 = background["='!?+!_z_cds_ad___'!?=fas"]
local gunvalue4 = background["!'+!+?!=fsajkfaksk'!=+?asf"]

local gunvalue1currentammo = background2["FKASKFakfgkafgaclzxlcqpowpasf"]
local gunvalue1storedammo = background2["kgasg2*r421q-q-dkasfafa"]

local gunvalue2currentammo = background2["?'!?=+=!=+!==E?CSZ?C_ASF__"]
local gunvalue2storedammo = background2["!'?+%?!OFDSKAFÖKLO'!LO+WMSAF"]

local gunvalue3currentammo = background2["'!+!=RFLOASFAKFLL!'LRLKSAKF"]
local gunvalue3storedammo = background2["'!?é+?!?FKSAKFKA!'+?=ZKÖCXÇLcç"]


local fovCircle = Drawing.new("Circle")
fovCircle.Thickness = 2
fovCircle.Radius = 50
fovCircle.Filled = false
fovCircle.Color = Color3.fromRGB(0, 0, 255)
fovCircle.Visible = false


local fovActive = false
local Players = game:GetService("Players")


RunService.RenderStepped:Connect(function()
    fovCircle.Position = Vector2.new(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y)
    
    if fovActive then
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer then
                local character = player.Character
                if character and character:FindFirstChild("WorldCharacter") and character.WorldCharacter:FindFirstChild("HumanoidRootPart") then
                    local rootPos = character.HumanoidRootPart.Position
                    local screenPos, onScreen = workspace.CurrentCamera:WorldToViewportPoint(rootPos)
                    local distFromCenter = (Vector2.new(screenPos.X, screenPos.Y) - fovCircle.Position).Magnitude

                    if distFromCenter <= fovCircle.Radius then
                        local inventory = player:FindFirstChild("GunInventory")
                        if inventory then
                            local slot1 = inventory:FindFirstChild("Slot1")
                            local slot2 = inventory:FindFirstChild("Slot2")
                            local slot3 = inventory:FindFirstChild("Slot3")
                            local slot4 = inventory:FindFirstChild("Slot4")
                            
                            gunvalue1.Text = slot1 and slot1:IsA("ValueBase") and tostring(slot1.Value) or "nil"
                            gunvalue2.Text = slot2 and slot2:IsA("ValueBase") and tostring(slot2.Value) or "nil"
                            gunvalue3.Text = slot3 and slot3:IsA("ValueBase") and tostring(slot3.Value) or "nil"
                            gunvalue4.Text = slot4 and slot4:IsA("ValueBase") and tostring(slot4.Value) or "nil"

                            gunvalue1currentammo.Text = slot1.BulletsInMagazine.Value
                            gunvalue1storedammo.Text = slot1.BulletsInReserve.Value
                            gunvalue2currentammo.Text = slot2.BulletsInMagazine.Value
                            gunvalue2storedammo.Text = slot2.BulletsInReserve.Value
                            gunvalue3currentammo.Text = slot3.BulletsInMagazine.Value
                            gunvalue3storedammo.Text = slot3.BulletsInReserve.Value

                            playernameinvviewer.Text = slot1.Parent.Parent.Name
                        end
                    end
                end
            end
        end
    end
end)


VisualMainSection:toggle({
    name = "Inventory Viewer",
    def = false,
    callback = function(Boolean)
        if Boolean then
            fovActive = true
            background.Visible = true
            background2.Visible = true
        else
            fovActive = false
            background.Visible = false
            background2.Visible = false
        end
    end
})



--player esp

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

local ESP_Skeletons = {}
local PlayerESPEnabled = false

local function CreateSkeletonESP(player)
if player == LocalPlayer then return end

local Skeleton = {
    Head = Drawing.new("Line"),
    Torso = Drawing.new("Line"),
    LeftArm = Drawing.new("Line"),
    RightArm = Drawing.new("Line"),
    LeftLeg = Drawing.new("Line"),
    RightLeg = Drawing.new("Line"),
}

for _, line in pairs(Skeleton) do
    line.Color = Color3.fromRGB(255, 255, 255)
    line.Thickness = 1.5
    line.Visible = false
end

ESP_Skeletons[player] = Skeleton

local function Update()
    if not player or not player.Character then
        for _, line in pairs(Skeleton) do
            line.Visible = false
        end
        return
    end

    local Char = player.Character:FindFirstChild("WorldCharacter")
    if not Char then
        for _, line in pairs(Skeleton) do
            line.Visible = false
        end
        return
    end

    local RootPart = Char:FindFirstChild("Root") or Char.PrimaryPart
    local Head = Char:FindFirstChild("Head")

    if not RootPart or not Head then
        for _, line in pairs(Skeleton) do
            line.Visible = false
        end
        return
    end

    local parts = {
        Head = Head,
        UpperTorso = Char:FindFirstChild("UpperTorso") or Char:FindFirstChild("Torso"),
        LowerTorso = Char:FindFirstChild("LowerTorso") or Char:FindFirstChild("Torso"),
        LeftArm = Char:FindFirstChild("LeftUpperArm") or Char:FindFirstChild("Left Arm"),
        RightArm = Char:FindFirstChild("RightUpperArm") or Char:FindFirstChild("Right Arm"),
        LeftLeg = Char:FindFirstChild("LeftUpperLeg") or Char:FindFirstChild("Left Leg"),
        RightLeg = Char:FindFirstChild("RightUpperLeg") or Char:FindFirstChild("Right Leg"),
    }

    local function GetPos(part)
        if part and part:IsA("BasePart") then
            local ScreenPos, IsVisible = Camera:WorldToViewportPoint(part.Position)
            return Vector2.new(ScreenPos.X, ScreenPos.Y), IsVisible
        end
        return nil, false
    end

    local function DrawLine(part1, part2, line)
        local p1, v1 = GetPos(parts[part1])
        local p2, v2 = GetPos(parts[part2])
        if v1 and v2 then
            line.From = p1
            line.To = p2
            line.Visible = true
        else
            line.Visible = false
        end
    end

    DrawLine("Head", "UpperTorso", Skeleton.Head)
    DrawLine("UpperTorso", "LowerTorso", Skeleton.Torso)
    DrawLine("UpperTorso", "LeftArm", Skeleton.LeftArm)
    DrawLine("UpperTorso", "RightArm", Skeleton.RightArm)
    DrawLine("LowerTorso", "LeftLeg", Skeleton.LeftLeg)
    DrawLine("LowerTorso", "RightLeg", Skeleton.RightLeg)
end

RunService.RenderStepped:Connect(Update)
end

local function RemoveSkeletonESP(player)
if ESP_Skeletons[player] then
    for _, line in pairs(ESP_Skeletons[player]) do
        line:Remove()
    end
    ESP_Skeletons[player] = nil
end
end

Fullplresp:toggle({
name = "Player ESP Skeleton",
def = false,
callback = function(Boolean)
    PlayerESPEnabled = Boolean
    for _, player in ipairs(Players:GetPlayers()) do
        if PlayerESPEnabled then
            CreateSkeletonESP(player)
        else
            RemoveSkeletonESP(player)
        end
    end
end
})

Players.PlayerAdded:Connect(function(player)
if PlayerESPEnabled then
    CreateSkeletonESP(player)
end
end)

Players.PlayerRemoving:Connect(RemoveSkeletonESP)

for _, player in ipairs(Players:GetPlayers()) do
if PlayerESPEnabled then
    CreateSkeletonESP(player)
end
end


-------------------------------------------------------------
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

local ESP_Boxes = {}
local ESP_Names = {}
local ESP_Distances = {}

local espSettings = {
showBoxes = false,
showDistance = false,
showName = false,
nameSize = 14,
nameFont = 1,
distanceUnit = "Studs",
nameOutline = false,
}

local function ConvertDistance(value)
if espSettings.distanceUnit == "Meters" then
    return value * 0.28
elseif espSettings.distanceUnit == "Feet" then
    return value * 0.936
elseif espSettings.distanceUnit == "Kilometers" then
    return value * 0.00028
else
    return value
end
end

if not Camera then
pcall(function()
    Camera = workspace.CurrentCamera
end)
end

local function UpdateESP(player)
if not player or not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") then
    return
end

local Character = player.Character
local RootPart = Character:FindFirstChild("HumanoidRootPart")
local Head = Character:FindFirstChild("ServerColliderHead")

if not RootPart or not Head then return end

if not Camera then
    return
end

local Offset = Vector3.new(0, 3, 0)

local Position, OnScreen = Camera:WorldToViewportPoint(RootPart.Position + Offset)
local distance = (RootPart.Position - Camera.CFrame.Position).Magnitude
local convertedDistance = ConvertDistance(distance)

local Box = ESP_Boxes[player]
local Name = ESP_Names[player]
local DistanceText = ESP_Distances[player]

if not Box or not Name or not DistanceText then
    return
end

local SizeY = 0
local SizeX = 0

if RootPart and Camera then
    SizeY = (Camera:WorldToViewportPoint(RootPart.Position + Offset - Vector3.new(0, 3, 0)).Y - Position.Y) * 2
    SizeX = SizeY / 2.2
end

if OnScreen then
    if espSettings.showBoxes then
        Box.Size = Vector2.new(SizeX, SizeY)
        Box.Position = Vector2.new(Position.X - SizeX / 2, Position.Y - SizeY / 2)
        Box.Visible = true
    else
        Box.Visible = false
    end

    if espSettings.showName then
        Name.Size = espSettings.nameSize
        Name.Position = Vector2.new(Position.X, Position.Y - SizeY / 2 - 20)
        Name.Text = player.Name
        Name.Outline = espSettings.nameOutline
        Name.Visible = true
        Name.Font = espSettings.nameFont
    else
        Name.Visible = false
    end

    if espSettings.showDistance then
        DistanceText.Position = Vector2.new(Position.X, Position.Y + SizeY / 2 + 5)
        DistanceText.Text = string.format("%.1f %s", convertedDistance, espSettings.distanceUnit)
        DistanceText.Visible = true
    else
        DistanceText.Visible = false
    end
else
    Box.Visible = false
    Name.Visible = false
    DistanceText.Visible = false
end
end

local function UpdateAllESP()
for _, player in ipairs(Players:GetPlayers()) do
    UpdateESP(player)
end
end

Fullplresp:toggle({
name = "Player ESP Boxes",
def = false,
callback = function(Boolean)
    espSettings.showBoxes = Boolean
end
})

Fullplresp:toggle({
name = "Player ESP Name",
def = false,
callback = function(Boolean)
    espSettings.showName = Boolean
end
})

Fullplresp:toggle({
name = "Player ESP Distance",
def = false,
callback = function(Boolean)
    espSettings.showDistance = Boolean
end
})

Fullplresp:toggle({
name = "Name Outline",
def = false,
callback = function(Boolean)
    espSettings.nameOutline = Boolean
end
})

Fullplresp:toggle({
name = "Distance Outline",
def = false,
callback = function(Boolean)
    for _, player in ipairs(Players:GetPlayers()) do
        local DistanceText = ESP_Distances[player]
        if DistanceText then
            DistanceText.Outline = Boolean
        end
    end
end
})

Fullplresp:slider({
name = "Name Size",
def = 14,
max = 100,
min = 1,
rounding = true,
callback = function(value)
    espSettings.nameSize = value
    UpdateAllESP()
end
})

Fullplresp:slider({
name = "Distance Size",
def = 14,
max = 100,
min = 1,
rounding = true,
callback = function(value)
    for _, player in ipairs(Players:GetPlayers()) do
        local DistanceText = ESP_Distances[player]
        if DistanceText then
            DistanceText.Size = value
        end
    end
end
})

Fullplresp:dropdown({
name = "Name Font",
def = "1",
max = 5,
options = {"0", "1", "2", "3"},
callback = function(value)
    espSettings.nameFont = tonumber(value)
    UpdateAllESP()
end
})

Fullplresp:dropdown({
name = "Convert Distance",
def = "Studs",
max = 4,
options = {"Meters", "Studs", "Feet", "Kilometers"},
callback = function(value)
    espSettings.distanceUnit = value
    UpdateAllESP()
end
})

local function CreateESP(player)
if player == LocalPlayer then return end

local Box = Drawing.new("Square")
Box.Color = Color3.fromRGB(215, 0, 0)
Box.Thickness = 1.5
Box.Filled = true
Box.Visible = false
Box.Transparency = 0.4

local Name = Drawing.new("Text")
Name.Color = Color3.fromRGB(180, 0, 180)
Name.Size = espSettings.nameSize
Name.Center = true
Name.Outline = espSettings.nameOutline
Name.Visible = false
Name.Font = espSettings.nameFont

local DistanceText = Drawing.new("Text")
DistanceText.Color = Color3.fromRGB(255, 255, 255)
DistanceText.Size = 16
DistanceText.Center = true
DistanceText.Outline = true
DistanceText.Visible = false

ESP_Boxes[player] = Box
ESP_Names[player] = Name
ESP_Distances[player] = DistanceText

local function Update()
    if not player or not player.Character or not player.Character.Parent or not player.Character:FindFirstChild("ServerColliderHead") then
        Box.Visible = false
        Name.Visible = false
        DistanceText.Visible = false
        return
    end

    UpdateESP(player)
end

RunService.RenderStepped:Connect(Update)
end

local function RemoveESP(player)
if ESP_Boxes[player] then
    ESP_Boxes[player]:Remove()
    ESP_Boxes[player] = nil
end
if ESP_Names[player] then
    ESP_Names[player]:Remove()
    ESP_Names[player] = nil
end
if ESP_Distances[player] then
    ESP_Distances[player]:Remove()
    ESP_Distances[player] = nil
end
end

Players.PlayerAdded:Connect(CreateESP)
Players.PlayerRemoving:Connect(RemoveESP)

RunService.RenderStepped:Connect(UpdateAllESP)

for _, player in ipairs(Players:GetPlayers()) do
CreateESP(player)
end


--Misc Sections
MiscMoveSettings:toggle({
    name = "Staff Detector",
    def = false,
    callback = function(Boolean)
        local playerConnections = {}

        local function checkPlayer(player)
            task.spawn(function()
                local success, rank = pcall(function()
                    return player:GetRankInGroup(3441839)
                end)

                if success and rank >= 99 then
                    game.StarterGui:SetCore("SendNotification", {
                        Title = "Staff Detected",
                        Text = player.Name .. " is a staff member!",
                        Duration = 1000,
                        Button1 = "Ignore"
                    })
                end
            end)
        end

        if Boolean then
            for _, player in pairs(game.Players:GetPlayers()) do
                checkPlayer(player)
            end
            playerConnections[#playerConnections + 1] = game.Players.PlayerAdded:Connect(function(player)
                checkPlayer(player)
            end)
        else
            for _, connection in pairs(playerConnections) do
                connection:Disconnect()
            end
            playerConnections = {}
        end
    end
})


MiscMoveSettings:button({name = "No Fog", callback = function()
    local atmosphereCount = 0

    for _, child in pairs(lighting:GetChildren()) do
    if not child:IsA("Atmosphere") and child.Name ~= "ColorCorrection" then 
        child:Destroy() 
    elseif child:IsA("Atmosphere") then
        atmosphereCount = atmosphereCount + 1
        end
    end
end})

MiscMoveSettings:toggle({
    name = "Bright",
    def = false,
    callback = function(Boolean)
        local camera = workspace.CurrentCamera
        local colorCorrection = camera:FindFirstChild("FullBrightColorCorrection")

        if Boolean then
            if not colorCorrection then
                local newColorCorrection = Instance.new("ColorCorrectionEffect")
                newColorCorrection.Name = "FullBrightColorCorrection"
                newColorCorrection.Brightness = 0.2
                newColorCorrection.Contrast = 0.1
                newColorCorrection.Saturation = 0.15
                newColorCorrection.Parent = camera
            end
        else
            if colorCorrection then
                colorCorrection:Destroy()
            end
        end
    end
})

MiscMoveSettings:button({name = "Fix Car Being Stuck", callback = function()
    for i,v in pairs(game.Workspace:GetChildren()) do
        if v.Name == "WorldModel" then
            if v:FindFirstChild("CollisionHelper") then
            v.CollisionHelper:Destroy()
            end
        end
    end
end})

    --gun scope modifiers
    local selectedScope = "ACOG"
    local selectedSlot = "Slot1"
    
    SAimSection:dropdown({
        name = "Gun Scope Modifier", 
        def = "ACOG", 
        max = 4, 
        options = {"MicroReflex", "Holo", "ACOG", "SniperScope8x", "SniperScope16xStatic", "SniperScopeLeeEnfield", "ACOGww2", "LaserSight","LPVOScope","RedDot"}, 
        callback = function(value)
            selectedScope = value
        end
    })
    
    SAimSection:dropdown({
        name = "Slot Selector", 
        def = "Slot1", 
        max = 4, 
        options = {"Slot1", "Slot2", "Slot3"}, 
        callback = function(value)
            selectedSlot = value
        end
    })
    
    SAimSection:button({
        name = "Apply Gun Scope", 
        callback = function()
            local player = game:GetService("Players").LocalPlayer
            local gunInventory = player:FindFirstChild("GunInventory")
            if not gunInventory then
                return
            end
    
            local slot = gunInventory:FindFirstChild(selectedSlot)
            if not slot then
                return
            end
    
            local attachmentReticle = slot:FindFirstChild("AttachmentReticle")
            if not attachmentReticle or not attachmentReticle:IsA("ObjectValue") then
                return
            end
    
            local scope = game:GetService("ReplicatedStorage").Assets.Attachments:FindFirstChild(selectedScope)
            if not scope then
                return
            end
    
            attachmentReticle.Value = scope
        end
    })

    --gun other stuff modifiers

local selectedSuppressor = "OspreySuppressor"
local activeSlot = "Slot1"

SAimSection:dropdown({
name = "Gun Suppressor Modifier", 
def = "OspreySuppressor", 
max = 3, 
options = {"OspreySuppressor", "PistolSuppressor", "RifleSuppressor"}, 
callback = function(option)
    selectedSuppressor = option
end
})

SAimSection:dropdown({
name = "Slot Selector", 
def = "Slot1", 
max = 3, 
options = {"Slot1", "Slot2", "Slot3"}, 
callback = function(option)
    activeSlot = option
end
})

SAimSection:button({
name = "Apply Gun Suppressor", 
callback = function()
    local playerInstance = game:GetService("Players").LocalPlayer
    local inventory = playerInstance:FindFirstChild("GunInventory")
    if not inventory then
        return
    end

    local slotInstance = inventory:FindFirstChild(activeSlot)
    if not slotInstance then
        return
    end

    local muzzleAttachment = slotInstance:FindFirstChild("AttachmentMuzzle")
    if not muzzleAttachment or not muzzleAttachment:IsA("ObjectValue") then
        return
    end

    local suppressor = game:GetService("ReplicatedStorage").Assets.Attachments:FindFirstChild(selectedSuppressor)
    if not suppressor then
        return
    end
    muzzleAttachment.Value = suppressor
end
})

AimingTab:openpage()
end
