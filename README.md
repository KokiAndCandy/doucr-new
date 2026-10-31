--[[
    doucr.Main - Airflow UI (Rose Theme) FULL
    Silent Aim Normal + HC + Old Aim + Camlock + Force Hit + Flamelock + ESP + Speed + etc
]]

-- ==================== CLEANUP ====================
pcall(function()
    local lp = game:GetService("Players").LocalPlayer
    local pg = lp and lp:FindFirstChild("PlayerGui")
    for _, parent in ipairs({pg, game:GetService("CoreGui")}) do
        if parent then
            for _, n in ipairs({"MockupInterface","SaturnsMainUI","SaturnSkinChanger","doucrMain","AirflowUI","doucr.Main"}) do
                local o = parent:FindFirstChild(n)
                if o then o:Destroy() end
            end
        end
    end
    if gethui then
        local h = gethui()
        for _, n in ipairs({"MockupInterface","SaturnsMainUI","SaturnSkinChanger","doucrMain","AirflowUI","doucr.Main"}) do
            local o = h:FindFirstChild(n)
            if o then o:Destroy() end
        end
    end
end)

-- ==================== SERVICES ====================
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local me = LocalPlayer
local UIS = game:GetService("UserInputService")
local cam = workspace.CurrentCamera
local mouse = LocalPlayer:GetMouse()
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Workspace = game:GetService("Workspace")
local TweenService = game:GetService("TweenService")
local CAS = game:GetService("ContextActionService")
local Lighting = game:GetService("Lighting")
local StarterGui = game:GetService("StarterGui")

StarterGui:SetCore("SendNotification", { Title = "doucr.Main", Text = "Airflow Rose FULL loading...", Duration = 3 })

-- ==================== GLOBALS ====================
_G.FOV_RADIUS = _G.FOV_RADIUS or 150
_G.ShowFOV = _G.ShowFOV or false
_G.RevolverBypass = _G.RevolverBypass or false
_G.WallCheck = _G.WallCheck or false
_G.SilentAim_Enabled = _G.SilentAim_Enabled or false
_G.SilentAimEnabled = _G.SilentAim_Enabled
_G.KnockCheck = _G.KnockCheck or false
_G.DeathPositions = _G.DeathPositions or {}
_G.ESP_Enabled = _G.ESP_Enabled or false
_G.ESP_Boxes = _G.ESP_Boxes or false
_G.ESP_Names = _G.ESP_Names or false
_G.ESP_DisplayNames = _G.ESP_DisplayNames or false
_G.ESP_Health = _G.ESP_Health or false
_G.ESP_Distance = _G.ESP_Distance or false
_G.ESP_Tracer = _G.ESP_Tracer or false
_G.ESP_Skeleton = _G.ESP_Skeleton or false
_G.ESP_Color = _G.ESP_Color or Color3.fromRGB(255, 140, 185)
_G.SpeedMaster = _G.SpeedMaster or false
_G.SpeedActive = _G.SpeedActive or false
_G.SpeedValue = _G.SpeedValue or 50
_G.SpeedKey = _G.SpeedKey or Enum.KeyCode.X
_G.FlamelockEnabled = _G.FlamelockEnabled or false
_G.FlameMode = _G.FlameMode or "Hold"
_G.FlameKey = _G.FlameKey or Enum.KeyCode.Z
_G.FlameRightClick = _G.FlameRightClick or false
_G.FlameSmoothness = _G.FlameSmoothness or 0.15
_G.FlamePrediction = _G.FlamePrediction or 0
_G.FlameLeftOffset = _G.FlameLeftOffset or 0
_G.FlameUpOffset = _G.FlameUpOffset or 0
_G.FlameHitPart = _G.FlameHitPart or "HumanoidRootPart"
_G.FlameActive = false
_G.FPSTarget = _G.FPSTarget or 240
_G.Whitelist = _G.Whitelist or {}
_G.BulletSpreadAmount = _G.BulletSpreadAmount or 100
_G.TeleportKey = _G.TeleportKey or Enum.KeyCode.T
_G.TeleportMode = _G.TeleportMode or "Hold"

_G.ForceHitEnabled = _G.ForceHitEnabled or false
_G.ForceHitMode = _G.ForceHitMode or "Fov"
_G.ForceHitFOV = _G.ForceHitFOV or 200
_G.ForceHitTracerEnabled = _G.ForceHitTracerEnabled or true
_G.ForceHitFullAutoEnabled = _G.ForceHitFullAutoEnabled or false
_G.ForceHitFireRate = _G.ForceHitFireRate or 0.05

_G.HCSilentAimEnabled = _G.HCSilentAimEnabled or false
_G.HCRevolverBypass = _G.HCRevolverBypass or false
_G.HCWallCheck = _G.HCWallCheck or false
_G.HCKnockCheck = _G.HCKnockCheck or false
_G.HCPrediction = _G.HCPrediction or false
_G.HCPredictionAmount = _G.HCPredictionAmount or 0.1
_G.HCFOVRadius = _G.HCFOVRadius or 200
_G.HCShowFOV = _G.HCShowFOV or false
_G.HCHitPart = _G.HCHitPart or "Head"
_G.HCGodmodeEnabled = _G.HCGodmodeEnabled or false

_G.OldAimEnabled = _G.OldAimEnabled or false
_G.OldAimShowFOV = _G.OldAimShowFOV or false
_G.OldAimFOVRadius = _G.OldAimFOVRadius or 200
_G.OldAimHitChance = _G.OldAimHitChance or 100
_G.OldAimHitPart = _G.OldAimHitPart or "Head"
_G.OldAimClosestPart = _G.OldAimClosestPart or false
_G.OldAimWallCheck = _G.OldAimWallCheck or false
_G.OldAimKnockCheck = _G.OldAimKnockCheck or false
_G.OldAimPredictionX = _G.OldAimPredictionX or 0
_G.OldAimPredictionY = _G.OldAimPredictionY or 0
_G.OldAimPredictionZ = _G.OldAimPredictionZ or 0
_G.OldAimOffsetX = _G.OldAimOffsetX or 0
_G.OldAimOffsetY = _G.OldAimOffsetY or 0
_G.OldAimOffsetZ = _G.OldAimOffsetZ or 0
_G.OldAimMaxDistance = _G.OldAimMaxDistance or 1000
_G.OldAimUseKeybind = _G.OldAimUseKeybind or false
_G.OldAimToggleKey = _G.OldAimToggleKey or Enum.KeyCode.V

_G.CamlockEnabled = _G.CamlockEnabled or false
_G.CamlockToggleKey = _G.CamlockToggleKey or "C"
_G.CamlockMode = _G.CamlockMode or "Toggle"
_G.CamlockAutoToggle = _G.CamlockAutoToggle or false
_G.CamlockHitPart = _G.CamlockHitPart or "HumanoidRootPart"
_G.CamlockEasingStyle = _G.CamlockEasingStyle or "Quad"
_G.CamlockEasingDirection = _G.CamlockEasingDirection or "Out"
_G.CamlockFOVRadius = _G.CamlockFOVRadius or 200
_G.CamlockSmoothness = _G.CamlockSmoothness or 0.15
_G.CamlockPredictionEnabled = _G.CamlockPredictionEnabled or false
_G.CamlockPredictionX = _G.CamlockPredictionX or 0
_G.CamlockPredictionY = _G.CamlockPredictionY or 0
_G.CamlockPredictionZ = _G.CamlockPredictionZ or 0
_G.CamlockConditionsKnocked = _G.CamlockConditionsKnocked or false
_G.CamlockConditionsSelfKnocked = _G.CamlockConditionsSelfKnocked or false

_G.AntiAimViewEnabled = _G.AntiAimViewEnabled or true
_G.AntiModNotification = _G.AntiModNotification or true
_G.AntiModKick = _G.AntiModKick or true
_G.AntiModKickDelay = _G.AntiModKickDelay or 0
_G.AntiFallEnabled = _G.AntiFallEnabled or true
_G.HitboxEnabled = _G.HitboxEnabled or false
_G.HitboxSize = _G.HitboxSize or 5
_G.HitboxTransparency = _G.HitboxTransparency or 0.5
_G.HitboxColor = Color3.fromRGB(255, 140, 185)
_G.ColorCorrectionEnabled = _G.ColorCorrectionEnabled or false

local aimPart = "Head"
local headlessActive = false
local charKorbloxActive = false
local selectedTeleportUserId = nil
local teleportHeld = false
local teleportLoopConnection = nil
local flameTargetPart = nil
local ForceHitIsHoldingMouse = false
local ForceHitLastFireTime = 0
local ForceHitHighlightTarget = nil
local espObjects = {}
local BulletSpreadSettings = { Enabled = true }

local TriggerBot = {
    MasterEnabled = false,
    Active = false,
    Key = Enum.KeyCode.B,
    Delay = 0.08
}

local AllHitPartOptions = {
    "Head", "UpperTorso", "LowerTorso", "HumanoidRootPart",
    "LeftUpperArm", "RightUpperArm", "LeftLowerArm", "RightLowerArm",
    "LeftUpperLeg", "RightUpperLeg", "LeftLowerLeg", "RightLowerLeg",
    "LeftFoot", "RightFoot", "LeftHand", "RightHand", "Closest Point"
}

local HCForceHitParts = {
    "Head", "UpperTorso", "LowerTorso", "HumanoidRootPart",
    "LeftUpperArm", "LeftLowerArm", "LeftHand",
    "RightUpperArm", "RightLowerArm", "RightHand",
    "LeftUpperLeg", "LeftLowerLeg", "LeftFoot",
    "RightUpperLeg", "RightLowerLeg", "RightFoot"
}

local ForceHitAllowedTools = {
    "[DoubleBarrel]", "[Revolver]", "[Shotgun]",
    "[SMG]", "[Silencer]", "[TacticalShotgun]",
    "[Double-Barrel SG]"
}

-- ==================== UTILS ====================
local function isWhitelisted(player)
    return player and _G.Whitelist and _G.Whitelist[player.UserId] == true
end

local function IsKnocked(character)
    if not character then return false end
    local be = character:FindFirstChild("BodyEffects")
    if be then
        local ko = be:FindFirstChild("K.O")
        return ko and ko.Value == true
    end
    return false
end

local function IsGrabbed(player)
    return player and player.Character and player.Character:FindFirstChild("GRABBING_CONSTRAINT") ~= nil
end

local function IsHoldingGun()
    local char = LocalPlayer.Character
    if not char then return false end
    local tool = char:FindFirstChildOfClass("Tool")
    if not tool then return false end
    if tool:FindFirstChild("Ammo") or tool:FindFirstChild("Magazine") then return true end
    return false
end

local function getClosestPartToMouse(char)
    local m = UIS:GetMouseLocation()
    local nearest, nearestDist = nil, math.huge
    for _, name in ipairs(HCForceHitParts) do
        local part = char:FindFirstChild(name)
        if part then
            local sp, on = cam:WorldToViewportPoint(part.Position)
            if on then
                local d = (Vector2.new(sp.X, sp.Y) - Vector2.new(m.X, m.Y)).Magnitude
                if d < nearestDist then nearestDist = d nearest = part end
            end
        end
    end
    return nearest
end

local function getTargetPosition(player, character)
    if not character then return nil end
    if _G.KnockCheck and IsKnocked(character) then return nil end
    if aimPart == "Closest Point" then
        local p = getClosestPartToMouse(character)
        return p and p.Position
    end
    local part = character:FindFirstChild(aimPart) or character:FindFirstChild("HumanoidRootPart")
    return part and part.Position
end

-- ==================== SILENT AIM NORMAL ====================
local function getClosest()
    if not _G.SilentAim_Enabled then return nil end
    local mousePos = Vector2.new(mouse.X, mouse.Y)
    local best, bestDist = nil, _G.FOV_RADIUS
    for _, v in pairs(Players:GetPlayers()) do
        if v == LocalPlayer or isWhitelisted(v) then continue end
        local char = v.Character
        if not char then continue end
        local targetPos = getTargetPosition(v, char)
        if not targetPos then continue end
        local screenPos, onScreen = cam:WorldToScreenPoint(targetPos)
        if onScreen then
            local dist = (Vector2.new(screenPos.X, screenPos.Y) - mousePos).Magnitude
            if dist < bestDist then
                if _G.WallCheck then
                    local ray = Ray.new(cam.CFrame.Position, (targetPos - cam.CFrame.Position).Unit * 500)
                    local hit = workspace:FindPartOnRayWithIgnoreList(ray, {LocalPlayer.Character, cam})
                    if hit and hit:IsDescendantOf(char) then
                        bestDist = dist
                        best = targetPos
                    end
                else
                    bestDist = dist
                    best = targetPos
                end
            end
        end
    end
    return best
end

pcall(function()
    local modules = ReplicatedStorage:FindFirstChild("Modules")
    if modules then
        local gunHandler = modules:FindFirstChild("GunHandler")
        if gunHandler then
            local handler = require(gunHandler)
            if handler and handler.getAim then
                local oldFunc = handler.getAim
                handler.getAim = function(origin, maxDist)
                    if not _G.SilentAim_Enabled then return oldFunc(origin, maxDist) end
                    if _G.RevolverBypass then
                        local tool = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Tool")
                        if tool and (tool.Name:find("Revolver") or tool.Name:find("revolver")) then
                            return oldFunc(origin, maxDist)
                        end
                    end
                    local targetPos = getClosest()
                    if targetPos then
                        return (targetPos - origin).Unit, math.min((targetPos - origin).Magnitude, maxDist or 200)
                    end
                    return oldFunc(origin, maxDist)
                end
            end
        end
    end
end)

-- ==================== HC SILENT AIM ====================
local oldMouseIndex_HC = nil
local hcSilentAimCachedPart = nil

local function isHoldingRevolverHC()
    if not _G.HCRevolverBypass then return false end
    local char = LocalPlayer.Character
    if not char then return false end
    local tool = char:FindFirstChildOfClass("Tool")
    if tool then
        local n = string.lower(tool.Name)
        if n:find("revolver") or n:find("rev") then return true end
    end
    return false
end

local function hcWallBetween(pos)
    if not _G.HCWallCheck then return false end
    local ro = cam.CFrame.Position
    local rd = pos - ro
    local params = RaycastParams.new()
    params.FilterDescendantsInstances = {LocalPlayer.Character}
    params.FilterType = Enum.RaycastFilterType.Exclude
    local hit = workspace:Raycast(ro, rd, params)
    if not hit then return false end
    for _, p in pairs(Players:GetPlayers()) do
        if p.Character and hit.Instance:IsDescendantOf(p.Character) then return false end
    end
    return true
end

local function hcGetTargetPart(char)
    if not char then return nil end
    if _G.HCHitPart == "Closest Point" then
        return getClosestPartToMouse(char) or char:FindFirstChild("Head") or char:FindFirstChild("HumanoidRootPart")
    end
    return char:FindFirstChild(_G.HCHitPart) or char:FindFirstChild("Head") or char:FindFirstChild("HumanoidRootPart")
end

local function hcGetClosestPlayerToCursor()
    if isHoldingRevolverHC() then return nil end
    local closestPart = nil
    local shortestDist = _G.HCFOVRadius
    local mousePos = UIS:GetMouseLocation()
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and p.Character then
            local hum = p.Character:FindFirstChildOfClass("Humanoid")
            if hum and hum.Health > 0 then
                if _G.HCKnockCheck and IsKnocked(p.Character) then continue end
                if isWhitelisted(p) then continue end
                local hrp = p.Character:FindFirstChild("HumanoidRootPart")
                if hrp then
                    local sp, onScreen = cam:WorldToViewportPoint(hrp.Position)
                    if onScreen then
                        local dist2D = (Vector2.new(sp.X, sp.Y) - mousePos).Magnitude
                        if dist2D < shortestDist then
                            local part = hcGetTargetPart(p.Character)
                            if part and not hcWallBetween(part.Position) then
                                shortestDist = dist2D
                                closestPart = part
                            end
                        end
                    end
                end
            end
        end
    end
    return closestPart
end

local function enableHCSilentAim(enable)
    if enable then
        if oldMouseIndex_HC then return end
        if not _G.__hcSilentAimLoop then
            _G.__hcSilentAimLoop = RunService.RenderStepped:Connect(function()
                if _G.HCSilentAimEnabled then
                    hcSilentAimCachedPart = hcGetClosestPlayerToCursor()
                else
                    hcSilentAimCachedPart = nil
                end
            end)
        end
        oldMouseIndex_HC = hookmetamethod(game, "__index", function(self, idx)
            if not checkcaller() and _G.HCSilentAimEnabled and self == mouse and (idx == "Hit" or idx == "Target" or idx == "UnitRay") then
                if hcSilentAimCachedPart and not isHoldingRevolverHC() then
                    local origin = cam.CFrame.Position
                    local hitPos = hcSilentAimCachedPart.Position
                    if _G.HCPrediction and _G.HCPredictionAmount > 0 then
                        local vel = hcSilentAimCachedPart.AssemblyLinearVelocity or Vector3.zero
                        hitPos = hitPos + Vector3.new(vel.X * _G.HCPredictionAmount, vel.Y * _G.HCPredictionAmount, vel.Z * _G.HCPredictionAmount)
                    end
                    if idx == "UnitRay" then
                        return Ray.new(origin, (hitPos - origin).Unit)
                    elseif idx == "Hit" then
                        return CFrame.new(hitPos)
                    elseif idx == "Target" then
                        return hcSilentAimCachedPart
                    end
                end
            end
            return oldMouseIndex_HC(self, idx)
        end)
    else
        if oldMouseIndex_HC then
            hookmetamethod(game, "__index", oldMouseIndex_HC)
            oldMouseIndex_HC = nil
        end
        hcSilentAimCachedPart = nil
    end
end

-- ==================== OLD AIM ====================
local oldAimCachedPart = nil
local oldAimOldMouseIndex = nil
local oldAimBodyParts = {
    "Head", "UpperTorso", "HumanoidRootPart", "LowerTorso",
    "LeftUpperArm", "RightUpperArm", "LeftLowerArm", "RightLowerArm",
    "LeftHand", "RightHand", "LeftUpperLeg", "RightUpperLeg",
    "LeftLowerLeg", "RightLowerLeg", "LeftFoot", "RightFoot"
}

local function oldAimWallBetween(pos)
    if not _G.OldAimWallCheck then return false end
    local ro = cam.CFrame.Position
    local rd = pos - ro
    local params = RaycastParams.new()
    params.FilterDescendantsInstances = {LocalPlayer.Character}
    params.FilterType = Enum.RaycastFilterType.Exclude
    local hit = workspace:Raycast(ro, rd, params)
    if not hit then return false end
    for _, p in pairs(Players:GetPlayers()) do
        if p.Character and hit.Instance:IsDescendantOf(p.Character) then return false end
    end
    return true
end

local function oldAimGetClosestBodyPart(char)
    local closest, shortest = nil, math.huge
    local mousePos = UIS:GetMouseLocation()
    for _, name in ipairs(oldAimBodyParts) do
        local part = char:FindFirstChild(name)
        if part then
            local sp, on = cam:WorldToViewportPoint(part.Position)
            if on then
                local d = (Vector2.new(sp.X, sp.Y) - mousePos).Magnitude
                if d < shortest then shortest = d closest = part end
            end
        end
    end
    return closest or char:FindFirstChild("Head") or char:FindFirstChild("HumanoidRootPart")
end

local function oldAimGetTargetPart(char)
    if not char then return nil end
    if _G.OldAimClosestPart then return oldAimGetClosestBodyPart(char) end
    return char:FindFirstChild(_G.OldAimHitPart) or char:FindFirstChild("Head") or char:FindFirstChild("HumanoidRootPart")
end

local function oldAimGetClosestPlayerPart()
    local closest, shortest = nil, _G.OldAimFOVRadius
    local mousePos = UIS:GetMouseLocation()
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and p.Character then
            local hum = p.Character:FindFirstChildOfClass("Humanoid")
            if not hum or hum.Health <= 0 then continue end
            if _G.OldAimKnockCheck and IsKnocked(p.Character) then continue end
            if isWhitelisted(p) then continue end
            local hrp = p.Character:FindFirstChild("HumanoidRootPart")
            if not hrp then continue end
            local dist3 = (hrp.Position - cam.CFrame.Position).Magnitude
            if dist3 > _G.OldAimMaxDistance then continue end
            local sp, on = cam:WorldToViewportPoint(hrp.Position)
            if not on then continue end
            local dist2 = (Vector2.new(sp.X, sp.Y) - mousePos).Magnitude
            if dist2 < shortest then
                local part = oldAimGetTargetPart(p.Character)
                if part and not oldAimWallBetween(part.Position) then
                    shortest = dist2
                    closest = part
                end
            end
        end
    end
    return closest
end

local function oldAimPredictedPos(part)
    local vel = part.AssemblyLinearVelocity or Vector3.zero
    return part.Position
        + Vector3.new(vel.X * _G.OldAimPredictionX, vel.Y * _G.OldAimPredictionY, vel.Z * _G.OldAimPredictionZ)
        + Vector3.new(_G.OldAimOffsetX, _G.OldAimOffsetY, _G.OldAimOffsetZ)
end

local function enableOldAim(enable)
    if enable then
        if oldAimOldMouseIndex then return end
        if not _G.__oldAimLoop then
            _G.__oldAimLoop = RunService.RenderStepped:Connect(function()
                if _G.OldAimEnabled then
                    oldAimCachedPart = oldAimGetClosestPlayerPart()
                else
                    oldAimCachedPart = nil
                end
            end)
        end
        oldAimOldMouseIndex = hookmetamethod(game, "__index", function(self, idx)
            if not checkcaller() and _G.OldAimEnabled and self == mouse and (idx == "Hit" or idx == "Target" or idx == "UnitRay") then
                if oldAimCachedPart and math.random(1, 100) <= _G.OldAimHitChance then
                    local origin = cam.CFrame.Position
                    local hitPos = oldAimPredictedPos(oldAimCachedPart)
                    if idx == "UnitRay" then
                        return Ray.new(origin, (hitPos - origin).Unit)
                    elseif idx == "Hit" then
                        return CFrame.new(hitPos)
                    elseif idx == "Target" then
                        return oldAimCachedPart
                    end
                end
            end
            return oldAimOldMouseIndex(self, idx)
        end)
    else
        if oldAimOldMouseIndex then
            hookmetamethod(game, "__index", oldAimOldMouseIndex)
            oldAimOldMouseIndex = nil
        end
        oldAimCachedPart = nil
    end
end

-- ==================== CAMLOCK ====================
local Camlock = { Target = nil, Active = false, Connection = nil }

local function GetCamlockHitPosition(Target)
    if not Target or not Target.Character then return nil end
    local Character = Target.Character
    local part = Character:FindFirstChild(_G.CamlockHitPart) or Character:FindFirstChild("HumanoidRootPart")
    if not part then return nil end
    local HitPosition = part.Position
    if _G.CamlockPredictionEnabled then
        local RootPart = Character:FindFirstChild("HumanoidRootPart")
        if RootPart then
            HitPosition = HitPosition + RootPart.Velocity * Vector3.new(_G.CamlockPredictionX, _G.CamlockPredictionY, _G.CamlockPredictionZ)
        end
    end
    return HitPosition
end

local function GetBestCamlockTarget()
    if not _G.CamlockEnabled then return nil end
    local Closest, Distance = nil, _G.CamlockFOVRadius > 0 and _G.CamlockFOVRadius or math.huge
    local MousePosition = UIS:GetMouseLocation()
    for _, Player in ipairs(Players:GetPlayers()) do
        if Player == LocalPlayer or not Player.Character then continue end
        if isWhitelisted(Player) then continue end
        local Character = Player.Character
        local HRP = Character:FindFirstChild("HumanoidRootPart")
        if not HRP then continue end
        local Position, OnScreen = cam:WorldToViewportPoint(HRP.Position)
        if not OnScreen then continue end
        if _G.CamlockConditionsKnocked and IsKnocked(Character) then continue end
        if _G.CamlockConditionsSelfKnocked and IsKnocked(LocalPlayer.Character) then continue end
        local Mag = (Vector2.new(Position.X, Position.Y) - MousePosition).Magnitude
        if Mag < Distance then Closest = Player Distance = Mag end
    end
    return Closest
end

local function UpdateCamlock()
    if not _G.CamlockEnabled then
        Camlock.Active = false
        Camlock.Target = nil
        return
    end
    if _G.CamlockAutoToggle then
        if not IsHoldingGun() then
            Camlock.Active = false
            Camlock.Target = nil
            return
        end
        if not Camlock.Active or not Camlock.Target or not Camlock.Target.Character then
            local t = GetBestCamlockTarget()
            if t then Camlock.Target = t Camlock.Active = true else Camlock.Active = false Camlock.Target = nil end
            return
        end
    else
        if not Camlock.Active then return end
    end
    if not Camlock.Active or not Camlock.Target or not Camlock.Target.Character then
        Camlock.Active = false
        return
    end
    local HitPosition = GetCamlockHitPosition(Camlock.Target)
    if not HitPosition then return end
    local Smoothing = _G.CamlockSmoothness
    local Eased = TweenService:GetValue(Smoothing, Enum.EasingStyle[_G.CamlockEasingStyle] or Enum.EasingStyle.Quad, Enum.EasingDirection[_G.CamlockEasingDirection] or Enum.EasingDirection.Out)
    cam.CFrame = cam.CFrame:Lerp(CFrame.new(cam.CFrame.Position, HitPosition), Eased)
end

local function EnableCamlock()
    if not _G.CamlockEnabled then return end
    local t = GetBestCamlockTarget()
    if t then Camlock.Target = t Camlock.Active = true end
end

local function DisableCamlock()
    Camlock.Active = false
    Camlock.Target = nil
end

Camlock.Connection = RunService.RenderStepped:Connect(UpdateCamlock)

-- ==================== FORCE HIT ====================
local ForceHitHighlightLine = Drawing.new("Line")
ForceHitHighlightLine.Thickness = 1.5
ForceHitHighlightLine.Color = Color3.fromRGB(255, 140, 185)
ForceHitHighlightLine.Transparency = 0.3
ForceHitHighlightLine.Visible = false

local function ForceHit_IsValidTarget(pl)
    if not pl or pl == LocalPlayer then return false end
    if not pl.Character then return false end
    local hum = pl.Character:FindFirstChild("Humanoid")
    if not hum or hum.Health <= 0 then return false end
    if _G.KnockCheck and IsKnocked(pl.Character) then return false end
    if isWhitelisted(pl) then return false end
    return true
end

local function ForceHit_GetClosestPartToMouse(pl)
    return getClosestPartToMouse(pl.Character)
end

local function ForceHit_GetFovTarget()
    local m = UIS:GetMouseLocation()
    local bestPart, bestDist = nil, _G.ForceHitFOV
    for _, pl in ipairs(Players:GetPlayers()) do
        if ForceHit_IsValidTarget(pl) then
            local part = ForceHit_GetClosestPartToMouse(pl)
            if part then
                local sp, on = cam:WorldToViewportPoint(part.Position)
                if on then
                    local d = (Vector2.new(sp.X, sp.Y) - Vector2.new(m.X, m.Y)).Magnitude
                    if d < bestDist then bestDist = d bestPart = part end
                end
            end
        end
    end
    return bestPart
end

local function ForceHit_GetBarrelPosition()
    local char = LocalPlayer.Character
    if not char then return nil end
    local tool = char:FindFirstChildOfClass("Tool")
    if tool then
        local h = tool:FindFirstChild("Handle") or tool:FindFirstChild("Barrel") or tool:FindFirstChild("Muzzle")
        if h and h:IsA("BasePart") then return h.Position end
    end
    local arm = char:FindFirstChild("Right Arm") or char:FindFirstChild("RightUpperArm")
    if arm then return arm.Position end
    return char:GetPivot().Position
end

local function ForceHit_SpawnTracer(startPos, endPos)
    if (endPos - startPos).Magnitude < 0.1 then return end
    local beam = Instance.new("Beam")
    local a0 = Instance.new("Attachment")
    local a1 = Instance.new("Attachment")
    beam.Segments = 1
    beam.Width0 = 0.1
    beam.Width1 = 0.1
    beam.Color = ColorSequence.new(Color3.fromRGB(255, 140, 185))
    beam.Transparency = NumberSequence.new(0.4)
    beam.FaceCamera = true
    a0.Position = startPos
    a1.Position = endPos
    a0.Parent = workspace.Terrain
    a1.Parent = workspace.Terrain
    beam.Attachment0 = a0
    beam.Attachment1 = a1
    beam.Parent = workspace.Terrain
    task.delay(0.08, function()
        beam:Destroy() a0:Destroy() a1:Destroy()
    end)
end

local function ForceHit_Fire(targetPart)
    if not targetPart then return end
    local impactPos = targetPart.Position
    local hrpPos = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") and LocalPlayer.Character.HumanoidRootPart.Position or Vector3.zero
    pcall(function()
        ReplicatedStorage.MainEvent:FireServer(unpack({
            "Shoot",
            {
                {{ Normal = impactPos, Instance = targetPart, Position = impactPos }, { Normal = impactPos, Instance = targetPart, Position = impactPos }, { Normal = impactPos, Instance = targetPart, Position = impactPos }, { Normal = impactPos, Instance = targetPart, Position = impactPos }, { Normal = impactPos, Instance = targetPart, Position = impactPos }},
                {{ thePart = targetPart, theOffset = Vector3.new(0,0,0) }, { thePart = targetPart, theOffset = Vector3.new(0,0,0) }, { thePart = targetPart, theOffset = Vector3.new(0,0,0) }, { thePart = targetPart, theOffset = Vector3.new(0,0,0) }, { thePart = targetPart, theOffset = Vector3.new(0,0,0) }},
                hrpPos, hrpPos, workspace:GetServerTimeNow()
            }
        }))
    end)
    if _G.ForceHitTracerEnabled then
        local barrel = ForceHit_GetBarrelPosition()
        if barrel then ForceHit_SpawnTracer(barrel, impactPos) end
    end
end

local function ForceHit_MouseClick(action, state, input)
    if state ~= Enum.UserInputState.Begin then return Enum.ContextActionResult.Pass end
    if not _G.ForceHitEnabled then return Enum.ContextActionResult.Pass end
    local char = LocalPlayer.Character
    if not char then return Enum.ContextActionResult.Pass end
    local tool = char:FindFirstChildOfClass("Tool")
    if not tool or not table.find(ForceHitAllowedTools, tool.Name) then return Enum.ContextActionResult.Pass end
    if _G.ForceHitMode == "Fov" then
        local part = ForceHit_GetFovTarget()
        if part then ForceHit_Fire(part) end
    elseif _G.ForceHitMode == "Manual" then
        if ForceHitHighlightTarget and ForceHitHighlightTarget.Character then
            local part = ForceHit_GetClosestPartToMouse(ForceHitHighlightTarget)
            if part then ForceHit_Fire(part) end
        end
    end
    return Enum.ContextActionResult.Sink
end

CAS:BindAction("NHForceHit", ForceHit_MouseClick, false, Enum.UserInputType.MouseButton1)

-- ==================== FLAMELOCK ====================
local function getFlameTarget()
    local mousePos = Vector2.new(mouse.X, mouse.Y)
    local closestPart, closestDist = nil, math.huge
    for _, v in pairs(Players:GetPlayers()) do
        if v == LocalPlayer or isWhitelisted(v) then continue end
        local char = v.Character
        if not char then continue end
        local part = char:FindFirstChild(_G.FlameHitPart) or char:FindFirstChild("HumanoidRootPart")
        if not part then continue end
        local sp, on = cam:WorldToScreenPoint(part.Position)
        if on then
            local d = (Vector2.new(sp.X, sp.Y) - mousePos).Magnitude
            if d < closestDist then closestDist = d closestPart = part end
        end
    end
    return closestPart
end

RunService.RenderStepped:Connect(function()
    if not (_G.FlamelockEnabled and _G.FlameActive and flameTargetPart) then return end
    if not flameTargetPart.Parent then
        _G.FlameActive = false
        flameTargetPart = nil
        return
    end
    local targetPos = flameTargetPart.Position
    if _G.FlamePrediction > 0 then
        local vel = flameTargetPart.AssemblyLinearVelocity or Vector3.zero
        targetPos = targetPos + vel * _G.FlamePrediction
    end
    targetPos = targetPos + Vector3.new(_G.FlameLeftOffset, _G.FlameUpOffset, 0)
    local smooth = math.clamp(_G.FlameSmoothness, 0.01, 1)
    cam.CFrame = cam.CFrame:Lerp(CFrame.new(cam.CFrame.Position, targetPos), smooth)
end)

-- ==================== TRIGGER BOT ====================
local function runTriggerBot()
    if not TriggerBot.MasterEnabled or not TriggerBot.Active then return end
    if not IsHoldingGun() then return end
    local target = mouse.Target
    if not target then return end
    local model = target:FindFirstAncestorOfClass("Model")
    if not model or model == me.Character then return end
    local humanoid = model:FindFirstChildOfClass("Humanoid")
    local player = Players:GetPlayerFromCharacter(model)
    if not humanoid or humanoid.Health <= 0 or not player or isWhitelisted(player) then return end
    local tool = me.Character and me.Character:FindFirstChildOfClass("Tool")
    if not tool then return end
    tool:Activate()
    task.wait(math.max(0, tonumber(TriggerBot.Delay) or 0))
end

RunService.Heartbeat:Connect(runTriggerBot)

-- ==================== SPEED ====================
local _speedHum = nil
LocalPlayer.CharacterAdded:Connect(function(char)
    _speedHum = nil
    task.defer(function()
        if not (_G.SpeedMaster and _G.SpeedActive) then return end
        local hum = char:FindFirstChildOfClass("Humanoid")
        if hum then _speedHum = hum hum.WalkSpeed = tonumber(_G.SpeedValue) or 16 end
    end)
end)

RunService.Heartbeat:Connect(function()
    if not (_G.SpeedMaster and _G.SpeedActive) then return end
    local char = LocalPlayer.Character
    if not char then return end
    local hum = _speedHum
    if not hum or hum.Parent ~= char then
        hum = char:FindFirstChildOfClass("Humanoid")
        _speedHum = hum
    end
    if hum then
        local t = tonumber(_G.SpeedValue) or 16
        if t < 16 then t = 16 end
        hum.WalkSpeed = t
    end
end)

-- ==================== ANTI FALL ====================
RunService.Heartbeat:Connect(function()
    if _G.AntiFallEnabled and LocalPlayer.Character then
        local hum = LocalPlayer.Character:FindFirstChild("Humanoid")
        if hum and hum.Health > 1 and hum:GetState() == Enum.HumanoidStateType.FallingDown then
            hum:ChangeState("GettingUp")
        end
    end
end)

-- ==================== FORCE HIT FULL AUTO + HIGHLIGHT ====================
RunService.Heartbeat:Connect(function()
    if _G.ForceHitFullAutoEnabled and ForceHitIsHoldingMouse and _G.ForceHitEnabled then
        local now = tick()
        if now - ForceHitLastFireTime < _G.ForceHitFireRate then return end
        ForceHitLastFireTime = now
        local char = LocalPlayer.Character
        if not char then return end
        local tool = char:FindFirstChildOfClass("Tool")
        if not tool or not table.find(ForceHitAllowedTools, tool.Name) then return end
        if _G.ForceHitMode == "Fov" then
            local part = ForceHit_GetFovTarget()
            if part then ForceHit_Fire(part) end
        end
    end
end)

RunService.RenderStepped:Connect(function()
    if not (_G.ForceHitEnabled and _G.ForceHitMode == "Manual") then
        ForceHitHighlightLine.Visible = false
        return
    end
    if ForceHitHighlightTarget and ForceHitHighlightTarget.Character then
        local part = ForceHit_GetClosestPartToMouse(ForceHitHighlightTarget)
        if part then
            local sp, on = cam:WorldToViewportPoint(part.Position)
            if on then
                ForceHitHighlightLine.From = UIS:GetMouseLocation()
                ForceHitHighlightLine.To = Vector2.new(sp.X, sp.Y)
                ForceHitHighlightLine.Visible = true
            else
                ForceHitHighlightLine.Visible = false
            end
        else
            ForceHitHighlightLine.Visible = false
        end
    else
        ForceHitHighlightLine.Visible = false
    end
end)

-- ==================== HITBOX ====================
local function UpdateHitboxes()
    for _, plr in pairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer and not isWhitelisted(plr) and plr.Character then
            local hrp = plr.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                if _G.HitboxEnabled then
                    hrp.Size = Vector3.new(_G.HitboxSize, _G.HitboxSize, _G.HitboxSize)
                    hrp.Transparency = 1 - (_G.HitboxTransparency or 0)
                    hrp.Color = _G.HitboxColor
                    hrp.Material = Enum.Material.Neon
                    hrp.CanCollide = false
                else
                    hrp.Size = Vector3.new(2, 2, 1)
                    hrp.Transparency = 1
                end
            end
        end
    end
end

task.spawn(function()
    while task.wait(0.4) do
        if _G.HitboxEnabled then UpdateHitboxes() end
    end
end)

-- ==================== CHAR ====================
local function applyCharHeadless(character)
    if not character then return end
    local head = character:FindFirstChild("Head")
    if not head then return end
    head.Transparency = headlessActive and 1 or 0
    local face = head:FindFirstChildWhichIsA("Decal")
    if face then face.Transparency = headlessActive and 1 or 0 end
end

local function applyCharKorblox(character)
    if not character then return end
    local lower = character:FindFirstChild("RightLowerLeg")
    local upper = character:FindFirstChild("RightUpperLeg")
    local foot = character:FindFirstChild("RightFoot")
    if not (lower and upper and foot) then return end
    if charKorbloxActive then
        lower.MeshId = "rbxassetid://902942093"
        lower.Transparency = 1
        upper.MeshId = "rbxassetid://902942096"
        upper.TextureID = "rbxassetid://902843398"
        foot.MeshId = "rbxassetid://902942089"
        foot.Transparency = 1
    end
end

LocalPlayer.CharacterAdded:Connect(function(char)
    task.wait(0.3)
    applyCharHeadless(char)
    applyCharKorblox(char)
    DisableCamlock()
end)

-- ==================== TELEPORT ====================
local function teleportToSelectedPlayer()
    if not selectedTeleportUserId then return end
    local target = Players:GetPlayerByUserId(selectedTeleportUserId)
    local character = LocalPlayer.Character
    local targetCharacter = target and target.Character
    if character and targetCharacter then
        local targetRoot = targetCharacter:FindFirstChild("HumanoidRootPart")
        if targetRoot then
            character:PivotTo(targetRoot.CFrame * CFrame.new(0, 0, 3))
        end
    end
end

local function stopTeleportLoop()
    teleportHeld = false
    if teleportLoopConnection then
        teleportLoopConnection:Disconnect()
        teleportLoopConnection = nil
    end
end

-- ==================== KEYBINDS ====================
UIS.InputBegan:Connect(function(input, gameProcessed)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        ForceHitIsHoldingMouse = true
    end
    if input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == TriggerBot.Key then
        if TriggerBot.MasterEnabled then
            TriggerBot.Active = not TriggerBot.Active
        end
    end
    if _G.OldAimUseKeybind and input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == _G.OldAimToggleKey then
        _G.OldAimEnabled = not _G.OldAimEnabled
        enableOldAim(_G.OldAimEnabled)
    end
    if gameProcessed then return end

    -- Camlock
    if _G.CamlockEnabled and not _G.CamlockAutoToggle then
        local camlockKey = Enum.KeyCode[_G.CamlockToggleKey] or Enum.KeyCode.C
        if input.KeyCode == camlockKey then
            if _G.CamlockMode == "Toggle" then
                if Camlock.Active then DisableCamlock() else EnableCamlock() end
            elseif _G.CamlockMode == "Hold" then
                EnableCamlock()
            end
        end
    end

    -- Flamelock
    if _G.FlamelockEnabled then
        local isTriggered = (_G.FlameRightClick and input.UserInputType == Enum.UserInputType.MouseButton2)
            or (not _G.FlameRightClick and input.KeyCode == _G.FlameKey)
        if isTriggered then
            if _G.FlameMode == "Hold" then
                _G.FlameActive = true
            else
                _G.FlameActive = not _G.FlameActive
            end
            if _G.FlameActive then
                local t = getFlameTarget()
                if t then flameTargetPart = t else _G.FlameActive = false flameTargetPart = nil end
            else
                flameTargetPart = nil
            end
        end
    end

    -- Speed
    if input.KeyCode == _G.SpeedKey then
        if _G.SpeedMaster then
            _G.SpeedActive = not _G.SpeedActive
            local char = LocalPlayer.Character
            local hum = char and char:FindFirstChildOfClass("Humanoid")
            if hum then
                hum.WalkSpeed = _G.SpeedActive and (tonumber(_G.SpeedValue) or 16) or 16
            end
        end
    end

    -- Teleport
    if input.KeyCode == _G.TeleportKey then
        if _G.TeleportMode == "Hold" then
            teleportHeld = true
            if not teleportLoopConnection then
                teleportLoopConnection = RunService.Heartbeat:Connect(function()
                    if teleportHeld then teleportToSelectedPlayer() end
                end)
            end
        else
            teleportToSelectedPlayer()
        end
    end
end)

UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        ForceHitIsHoldingMouse = false
    end
    if _G.CamlockEnabled and not _G.CamlockAutoToggle then
        local camlockKey = Enum.KeyCode[_G.CamlockToggleKey] or Enum.KeyCode.C
        if input.KeyCode == camlockKey and _G.CamlockMode == "Hold" then
            DisableCamlock()
        end
    end
    if _G.FlamelockEnabled and _G.FlameMode == "Hold" then
        local isTriggered = (_G.FlameRightClick and input.UserInputType == Enum.UserInputType.MouseButton2)
            or (not _G.FlameRightClick and input.KeyCode == _G.FlameKey)
        if isTriggered then
            _G.FlameActive = false
            flameTargetPart = nil
        end
    end
    if input.KeyCode == _G.TeleportKey and _G.TeleportMode == "Hold" then
        stopTeleportLoop()
    end
end)

-- ==================== FOV CIRCLES ====================
local fovCircle = Drawing.new("Circle")
fovCircle.Thickness = 1
fovCircle.NumSides = 64
fovCircle.Radius = 0
fovCircle.Filled = false
fovCircle.Visible = false
fovCircle.Color = Color3.fromRGB(255, 140, 185)
fovCircle.Transparency = 0.5

local hcFovCircle = Drawing.new("Circle")
hcFovCircle.Thickness = 1
hcFovCircle.NumSides = 64
hcFovCircle.Radius = 0
hcFovCircle.Filled = false
hcFovCircle.Visible = false
hcFovCircle.Color = Color3.fromRGB(255, 100, 150)
hcFovCircle.Transparency = 0.5

local oldAimFovCircle = Drawing.new("Circle")
oldAimFovCircle.Thickness = 1
oldAimFovCircle.NumSides = 64
oldAimFovCircle.Radius = 0
oldAimFovCircle.Filled = false
oldAimFovCircle.Visible = false
oldAimFovCircle.Color = Color3.fromRGB(200, 100, 180)
oldAimFovCircle.Transparency = 0.5

RunService.RenderStepped:Connect(function()
    local mp = UIS:GetMouseLocation()
    if _G.ShowFOV and _G.SilentAim_Enabled then
        fovCircle.Position = mp
        fovCircle.Radius = _G.FOV_RADIUS
        fovCircle.Visible = true
    else
        fovCircle.Visible = false
    end
    if _G.HCShowFOV and _G.HCSilentAimEnabled then
        hcFovCircle.Position = mp
        hcFovCircle.Radius = _G.HCFOVRadius
        hcFovCircle.Visible = true
    else
        hcFovCircle.Visible = false
    end
    if _G.OldAimShowFOV and _G.OldAimEnabled then
        oldAimFovCircle.Position = mp
        oldAimFovCircle.Radius = _G.OldAimFOVRadius
        oldAimFovCircle.Visible = true
    else
        oldAimFovCircle.Visible = false
    end
end)

-- ==================== BASIC ESP ====================
local function createESP(plr)
    if espObjects[plr] then return end
    local box = Drawing.new("Square")
    box.Thickness = 1
    box.Filled = false
    box.Color = _G.ESP_Color
    box.Visible = false
    local name = Drawing.new("Text")
    name.Size = 13
    name.Center = true
    name.Outline = true
    name.Color = _G.ESP_Color
    name.Visible = false
    local health = Drawing.new("Text")
    health.Size = 12
    health.Center = false
    health.Outline = true
    health.Color = Color3.fromRGB(50, 255, 50)
    health.Visible = false
    local distance = Drawing.new("Text")
    distance.Size = 12
    distance.Center = true
    distance.Outline = true
    distance.Color = Color3.fromRGB(200, 200, 200)
    distance.Visible = false
    local tracer = Drawing.new("Line")
    tracer.Thickness = 1
    tracer.Color = _G.ESP_Color
    tracer.Visible = false
    espObjects[plr] = { Box = box, Name = name, Health = health, Distance = distance, Tracer = tracer }
end

for _, p in ipairs(Players:GetPlayers()) do
    if p ~= LocalPlayer then createESP(p) end
end
Players.PlayerAdded:Connect(function(p)
    if p ~= LocalPlayer then createESP(p) end
end)
Players.PlayerRemoving:Connect(function(p)
    if espObjects[p] then
        for _, d in pairs(espObjects[p]) do pcall(function() d:Remove() end) end
        espObjects[p] = nil
    end
end)

RunService.RenderStepped:Connect(function()
    if not _G.ESP_Enabled then
        for _, objs in pairs(espObjects) do
            for _, d in pairs(objs) do d.Visible = false end
        end
        return
    end
    for plr, objs in pairs(espObjects) do
        local char = plr.Character
        local hrp = char and char:FindFirstChild("HumanoidRootPart")
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        local head = char and char:FindFirstChild("Head")
        if not hrp or not hum or hum.Health <= 0 then
            for _, d in pairs(objs) do d.Visible = false end
            continue
        end
        local pos, on = cam:WorldToViewportPoint(hrp.Position)
        if not on then
            for _, d in pairs(objs) do d.Visible = false end
            continue
        end
        local size = 2000 / pos.Z
        if _G.ESP_Boxes then
            objs.Box.Size = Vector2.new(size, size * 1.6)
            objs.Box.Position = Vector2.new(pos.X - size/2, pos.Y - size * 0.8)
            objs.Box.Color = _G.ESP_Color
            objs.Box.Visible = true
        else
            objs.Box.Visible = false
        end
        if _G.ESP_Names then
            objs.Name.Text = plr.DisplayName
            objs.Name.Position = Vector2.new(pos.X, pos.Y - size * 0.9 - 14)
            objs.Name.Color = _G.ESP_Color
            objs.Name.Visible = true
        else
            objs.Name.Visible = false
        end
        if _G.ESP_Health then
            objs.Health.Text = math.floor(hum.Health) .. " HP"
            objs.Health.Position = Vector2.new(pos.X + size/2 + 4, pos.Y - size * 0.4)
            objs.Health.Visible = true
        else
            objs.Health.Visible = false
        end
        if _G.ESP_Distance then
            local dist = (hrp.Position - cam.CFrame.Position).Magnitude
            objs.Distance.Text = math.floor(dist) .. "m"
            objs.Distance.Position = Vector2.new(pos.X, pos.Y + size * 0.85)
            objs.Distance.Visible = true
        else
            objs.Distance.Visible = false
        end
        if _G.ESP_Tracer then
            objs.Tracer.From = Vector2.new(cam.ViewportSize.X/2, cam.ViewportSize.Y)
            objs.Tracer.To = Vector2.new(pos.X, pos.Y + size * 0.8)
            objs.Tracer.Color = _G.ESP_Color
            objs.Tracer.Visible = true
        else
            objs.Tracer.Visible = false
        end
    end
end)

-- ==================== AIRFLOW UI ====================
local Airflow = loadstring(game:HttpGet("https://raw.githubusercontent.com/PookiePepelsss/Airflow-UI/refs/heads/main/Source.luau"))()

pcall(function() Airflow:LoadFont({ Name = "ValleySans" }) end)

local Window = Airflow:CreateWindow({
    Name = "doucr.Main",
    LoadingSubtitle = "Rose FULL",
    Theme = "Rose",
    ToggleUIKeybind = "RightShift",
    ConfigurationSaving = { Enabled = true, FolderName = "doucrMain" }
})

local Combat = Window:CreateTab({ Name = "Combat", Icon = "crosshair" })
local HC = Window:CreateTab({ Name = "Hood Customs", Icon = "zap" })
local Visuals = Window:CreateTab({ Name = "Visuals", Icon = "eye" })
local PlayerTab = Window:CreateTab({ Name = "Player", Icon = "user" })
local Misc = Window:CreateTab({ Name = "Misc", Icon = "settings" })

-- ===== COMBAT =====
Combat:CreateSection("Silent Aim (Normal)")

Combat:CreateToggle({
    Name = "Silent Aim",
    Flag = "SilentAim",
    CurrentValue = _G.SilentAim_Enabled,
    Callback = function(v) _G.SilentAim_Enabled = v _G.SilentAimEnabled = v end
})
Combat:CreateToggle({ Name = "Show FOV", Flag = "ShowFOV", CurrentValue = _G.ShowFOV, Callback = function(v) _G.ShowFOV = v end })
Combat:CreateToggle({ Name = "Wall Check", Flag = "WallCheck", CurrentValue = _G.WallCheck, Callback = function(v) _G.WallCheck = v end })
Combat:CreateToggle({ Name = "Knock Check", Flag = "KnockCheck", CurrentValue = _G.KnockCheck, Callback = function(v) _G.KnockCheck = v end })
Combat:CreateToggle({ Name = "Revolver Bypass", Flag = "RevolverBypass", CurrentValue = _G.RevolverBypass, Callback = function(v) _G.RevolverBypass = v end })
Combat:CreateSlider({ Name = "FOV Radius", Flag = "FOVRadius", Range = {0,1000}, Increment = 1, CurrentValue = _G.FOV_RADIUS, Callback = function(v) _G.FOV_RADIUS = v end })
Combat:CreateDropdown({ Name = "Hit Part", Flag = "HitPart", Options = {"Head","UpperTorso","HumanoidRootPart","Closest Point"}, CurrentOption = "Head", Callback = function(v) aimPart = v end })

Combat:CreateSection("Old Aim")

Combat:CreateToggle({
    Name = "Old Aim",
    Flag = "OldAim",
    CurrentValue = _G.OldAimEnabled,
    Callback = function(v) _G.OldAimEnabled = v enableOldAim(v) end
})
Combat:CreateToggle({ Name = "Old Show FOV", Flag = "OldShowFOV", CurrentValue = _G.OldAimShowFOV, Callback = function(v) _G.OldAimShowFOV = v end })
Combat:CreateToggle({ Name = "Old Closest Part", Flag = "OldClosest", CurrentValue = _G.OldAimClosestPart, Callback = function(v) _G.OldAimClosestPart = v end })
Combat:CreateToggle({ Name = "Old Wall Check", Flag = "OldWall", CurrentValue = _G.OldAimWallCheck, Callback = function(v) _G.OldAimWallCheck = v end })
Combat:CreateToggle({ Name = "Old Knock Check", Flag = "OldKnock", CurrentValue = _G.OldAimKnockCheck, Callback = function(v) _G.OldAimKnockCheck = v end })
Combat:CreateToggle({ Name = "Use Keybind", Flag = "OldKeybind", CurrentValue = _G.OldAimUseKeybind, Callback = function(v) _G.OldAimUseKeybind = v end })
Combat:CreateSlider({ Name = "Old FOV", Flag = "OldFOV", Range = {0,1000}, Increment = 1, CurrentValue = _G.OldAimFOVRadius, Callback = function(v) _G.OldAimFOVRadius = v end })
Combat:CreateSlider({ Name = "Hit Chance %", Flag = "OldChance", Range = {0,100}, Increment = 1, CurrentValue = _G.OldAimHitChance, Callback = function(v) _G.OldAimHitChance = v end })
Combat:CreateDropdown({ Name = "Old Hit Part", Flag = "OldHitPart", Options = {"Head","UpperTorso","HumanoidRootPart"}, CurrentOption = _G.OldAimHitPart, Callback = function(v) _G.OldAimHitPart = v end })

Combat:CreateSection("Camlock")

Combat:CreateToggle({
    Name = "Camlock",
    Flag = "Camlock",
    CurrentValue = _G.CamlockEnabled,
    Callback = function(v) _G.CamlockEnabled = v if not v then DisableCamlock() end end
})
Combat:CreateToggle({ Name = "Auto Toggle (Gun)", Flag = "CamlockAuto", CurrentValue = _G.CamlockAutoToggle, Callback = function(v) _G.CamlockAutoToggle = v end })
Combat:CreateSlider({ Name = "Camlock FOV", Flag = "CamlockFOV", Range = {0,1000}, Increment = 1, CurrentValue = _G.CamlockFOVRadius, Callback = function(v) _G.CamlockFOVRadius = v end })
Combat:CreateSlider({ Name = "Smoothness", Flag = "CamlockSmooth", Range = {0,1}, Increment = 0.01, CurrentValue = _G.CamlockSmoothness, Callback = function(v) _G.CamlockSmoothness = v end })
Combat:CreateDropdown({ Name = "Camlock Hit Part", Flag = "CamlockHit", Options = {"Head","UpperTorso","HumanoidRootPart"}, CurrentOption = _G.CamlockHitPart, Callback = function(v) _G.CamlockHitPart = v end })
Combat:CreateDropdown({ Name = "Mode", Flag = "CamlockMode", Options = {"Toggle","Hold"}, CurrentOption = _G.CamlockMode, Callback = function(v) _G.CamlockMode = v end })

Combat:CreateSection("Trigger Bot")

Combat:CreateToggle({
    Name = "Trigger Bot",
    Flag = "TriggerBot",
    CurrentValue = TriggerBot.MasterEnabled,
    Callback = function(v) TriggerBot.MasterEnabled = v if not v then TriggerBot.Active = false end end
})
Combat:CreateSlider({ Name = "Delay", Flag = "TriggerDelay", Range = {0,0.5}, Increment = 0.01, CurrentValue = TriggerBot.Delay, Callback = function(v) TriggerBot.Delay = v end })

Combat:CreateSection("Flamelock")

Combat:CreateToggle({
    Name = "Flamelock",
    Flag = "Flamelock",
    CurrentValue = _G.FlamelockEnabled,
    Callback = function(v) _G.FlamelockEnabled = v if not v then _G.FlameActive = false flameTargetPart = nil end end
})
Combat:CreateToggle({ Name = "Right Click", Flag = "FlameRC", CurrentValue = _G.FlameRightClick, Callback = function(v) _G.FlameRightClick = v end })
Combat:CreateDropdown({ Name = "Mode", Flag = "FlameMode", Options = {"Hold","Toggle"}, CurrentOption = _G.FlameMode, Callback = function(v) _G.FlameMode = v end })
Combat:CreateDropdown({ Name = "Hit Part", Flag = "FlameHit", Options = {"HumanoidRootPart","Head","UpperTorso"}, CurrentOption = _G.FlameHitPart, Callback = function(v) _G.FlameHitPart = v end })
Combat:CreateSlider({ Name = "Smoothness", Flag = "FlameSmooth", Range = {0,1}, Increment = 0.01, CurrentValue = _G.FlameSmoothness, Callback = function(v) _G.FlameSmoothness = v end })
Combat:CreateSlider({ Name = "Prediction", Flag = "FlamePred", Range = {0,0.5}, Increment = 0.01, CurrentValue = _G.FlamePrediction, Callback = function(v) _G.FlamePrediction = v end })

-- ===== HOOD CUSTOMS =====
HC:CreateSection("HC Silent Aim")

HC:CreateToggle({
    Name = "HC Silent Aim",
    Flag = "HCSilent",
    CurrentValue = _G.HCSilentAimEnabled,
    Callback = function(v) _G.HCSilentAimEnabled = v enableHCSilentAim(v) end
})
HC:CreateToggle({ Name = "HC Show FOV", Flag = "HCShowFOV", CurrentValue = _G.HCShowFOV, Callback = function(v) _G.HCShowFOV = v end })
HC:CreateToggle({ Name = "HC Revolver Bypass", Flag = "HCRev", CurrentValue = _G.HCRevolverBypass, Callback = function(v) _G.HCRevolverBypass = v end })
HC:CreateToggle({ Name = "HC Wall Check", Flag = "HCWall", CurrentValue = _G.HCWallCheck, Callback = function(v) _G.HCWallCheck = v end })
HC:CreateToggle({ Name = "HC Knock Check", Flag = "HCKnock", CurrentValue = _G.HCKnockCheck, Callback = function(v) _G.HCKnockCheck = v end })
HC:CreateToggle({ Name = "HC Prediction", Flag = "HCPred", CurrentValue = _G.HCPrediction, Callback = function(v) _G.HCPrediction = v end })
HC:CreateSlider({ Name = "HC FOV", Flag = "HCFOV", Range = {0,1000}, Increment = 1, CurrentValue = _G.HCFOVRadius, Callback = function(v) _G.HCFOVRadius = v end })
HC:CreateSlider({ Name = "HC Pred Amount", Flag = "HCPredAmt", Range = {0,0.5}, Increment = 0.01, CurrentValue = _G.HCPredictionAmount, Callback = function(v) _G.HCPredictionAmount = v end })
HC:CreateDropdown({ Name = "HC Hit Part", Flag = "HCHit", Options = {"Head","UpperTorso","HumanoidRootPart","Closest Point"}, CurrentOption = _G.HCHitPart, Callback = function(v) _G.HCHitPart = v end })

HC:CreateSection("Force Hit")

HC:CreateToggle({ Name = "Force Hit", Flag = "ForceHit", CurrentValue = _G.ForceHitEnabled, Callback = function(v) _G.ForceHitEnabled = v end })
HC:CreateDropdown({ Name = "Mode", Flag = "FHMode", Options = {"Fov","Manual"}, CurrentOption = _G.ForceHitMode, Callback = function(v) _G.ForceHitMode = v end })
HC:CreateSlider({ Name = "FH FOV", Flag = "FHFOV", Range = {0,1000}, Increment = 1, CurrentValue = _G.ForceHitFOV, Callback = function(v) _G.ForceHitFOV = v end })
HC:CreateToggle({ Name = "FH Tracer", Flag = "FHTracer", CurrentValue = _G.ForceHitTracerEnabled, Callback = function(v) _G.ForceHitTracerEnabled = v end })
HC:CreateToggle({ Name = "FH Full Auto", Flag = "FHAuto", CurrentValue = _G.ForceHitFullAutoEnabled, Callback = function(v) _G.ForceHitFullAutoEnabled = v end })
HC:CreateSlider({ Name = "Fire Rate", Flag = "FHRate", Range = {0.01,0.5}, Increment = 0.01, CurrentValue = _G.ForceHitFireRate, Callback = function(v) _G.ForceHitFireRate = v end })

HC:CreateSection("Hitbox Expander")

HC:CreateToggle({
    Name = "Hitbox Expander",
    Flag = "Hitbox",
    CurrentValue = _G.HitboxEnabled,
    Callback = function(v)
        _G.HitboxEnabled = v
        if not v then
            for _, plr in pairs(Players:GetPlayers()) do
                if plr ~= LocalPlayer and plr.Character then
                    local hrp = plr.Character:FindFirstChild("HumanoidRootPart")
                    if hrp then hrp.Size = Vector3.new(2,2,1) hrp.Transparency = 1 end
                end
            end
        end
    end
})
HC:CreateSlider({ Name = "Hitbox Size", Flag = "HitboxSize", Range = {0,20}, Increment = 0.5, CurrentValue = _G.HitboxSize, Callback = function(v) _G.HitboxSize = v end })
HC:CreateSlider({ Name = "Visibility", Flag = "HitboxVis", Range = {0,1}, Increment = 0.05, CurrentValue = _G.HitboxTransparency, Callback = function(v) _G.HitboxTransparency = v end })

-- ===== VISUALS =====
Visuals:CreateSection("ESP")

Visuals:CreateToggle({ Name = "ESP Enabled", Flag = "ESP", CurrentValue = _G.ESP_Enabled, Callback = function(v) _G.ESP_Enabled = v end })
Visuals:CreateToggle({ Name = "Boxes", Flag = "ESPBoxes", CurrentValue = _G.ESP_Boxes, Callback = function(v) _G.ESP_Boxes = v end })
Visuals:CreateToggle({ Name = "Names", Flag = "ESPNames", CurrentValue = _G.ESP_Names, Callback = function(v) _G.ESP_Names = v end })
Visuals:CreateToggle({ Name = "Health", Flag = "ESPHealth", CurrentValue = _G.ESP_Health, Callback = function(v) _G.ESP_Health = v end })
Visuals:CreateToggle({ Name = "Distance", Flag = "ESPDist", CurrentValue = _G.ESP_Distance, Callback = function(v) _G.ESP_Distance = v end })
Visuals:CreateToggle({ Name = "Tracer", Flag = "ESPTracer", CurrentValue = _G.ESP_Tracer, Callback = function(v) _G.ESP_Tracer = v end })

Visuals:CreateSection("Atmosphere")

Visuals:CreateToggle({
    Name = "Color Correction",
    Flag = "CC",
    CurrentValue = _G.ColorCorrectionEnabled,
    Callback = function(v)
        _G.ColorCorrectionEnabled = v
        local cc = Lighting:FindFirstChild("ValColorEffect")
        if v then
            if not cc then cc = Instance.new("ColorCorrectionEffect") cc.Name = "ValColorEffect" cc.Parent = Lighting end
            cc.Enabled = true
        else
            if cc then cc.Enabled = false end
        end
    end
})

-- ===== PLAYER =====
PlayerTab:CreateSection("Movement")

PlayerTab:CreateToggle({
    Name = "Speed Master",
    Flag = "SpeedMaster",
    CurrentValue = _G.SpeedMaster,
    Callback = function(v)
        _G.SpeedMaster = v
        if v then _G.SpeedActive = true else
            _G.SpeedActive = false
            local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            if hum then hum.WalkSpeed = 16 end
        end
    end
})
PlayerTab:CreateSlider({ Name = "Speed Value", Flag = "SpeedVal", Range = {16,500}, Increment = 1, CurrentValue = _G.SpeedValue, Callback = function(v) _G.SpeedValue = v end })
PlayerTab:CreateToggle({ Name = "Anti Fall", Flag = "AntiFall", CurrentValue = _G.AntiFallEnabled, Callback = function(v) _G.AntiFallEnabled = v end })

PlayerTab:CreateSection("Character")

PlayerTab:CreateToggle({
    Name = "Headless",
    Flag = "Headless",
    CurrentValue = headlessActive,
    Callback = function(v) headlessActive = v applyCharHeadless(LocalPlayer.Character) end
})
PlayerTab:CreateToggle({
    Name = "Korblox Leg",
    Flag = "Korblox",
    CurrentValue = charKorbloxActive,
    Callback = function(v) charKorbloxActive = v applyCharKorblox(LocalPlayer.Character) end
})

PlayerTab:CreateSection("Teleport")

PlayerTab:CreateDropdown({
    Name = "Teleport Mode",
    Flag = "TPMode",
    Options = {"Hold","Toggle"},
    CurrentOption = _G.TeleportMode,
    Callback = function(v) _G.TeleportMode = v stopTeleportLoop() end
})

local playerNames = {}
local playerMap = {}
for _, p in ipairs(Players:GetPlayers()) do
    if p ~= LocalPlayer then
        local label = p.DisplayName .. " (@" .. p.Name .. ")"
        table.insert(playerNames, label)
        playerMap[label] = p.UserId
    end
end

PlayerTab:CreateDropdown({
    Name = "Select Player",
    Flag = "TPPlayer",
    Options = #playerNames > 0 and playerNames or {"No players"},
    CurrentOption = playerNames[1] or "No players",
    Callback = function(v)
        selectedTeleportUserId = playerMap[v]
    end
})

PlayerTab:CreateButton({
    Name = "Refresh Players",
    Callback = function()
        Airflow:Notify({ Title = "Teleport", Content = "Reabra a aba Player para atualizar a lista", Duration = 3 })
    end
})

-- ===== MISC =====
Misc:CreateSection("Protection")

Misc:CreateToggle({ Name = "Anti Aim View", Flag = "AntiAim", CurrentValue = _G.AntiAimViewEnabled, Callback = function(v) _G.AntiAimViewEnabled = v end })
Misc:CreateToggle({ Name = "Anti Mod Notify", Flag = "AntiModN", CurrentValue = _G.AntiModNotification, Callback = function(v) _G.AntiModNotification = v end })
Misc:CreateToggle({ Name = "Anti Mod Kick", Flag = "AntiModK", CurrentValue = _G.AntiModKick, Callback = function(v) _G.AntiModKick = v end })

Misc:CreateSection("Settings")

Misc:CreateSlider({
    Name = "Target FPS",
    Flag = "FPS",
    Range = {60,1000},
    Increment = 10,
    CurrentValue = _G.FPSTarget,
    Callback = function(v) _G.FPSTarget = v if setfpscap then setfpscap(v) end end
})

Misc:CreateButton({
    Name = "KILL SWITCH (Unload)",
    Callback = function()
        _G.SilentAim_Enabled = false
        _G.HCSilentAimEnabled = false
        _G.OldAimEnabled = false
        _G.CamlockEnabled = false
        _G.ForceHitEnabled = false
        _G.FlamelockEnabled = false
        _G.SpeedMaster = false
        _G.SpeedActive = false
        _G.HitboxEnabled = false
        _G.ESP_Enabled = false
        TriggerBot.MasterEnabled = false
        TriggerBot.Active = false
        enableHCSilentAim(false)
        enableOldAim(false)
        DisableCamlock()
        for _, objs in pairs(espObjects) do
            for _, d in pairs(objs) do pcall(function() d:Remove() end) end
        end
        pcall(function() fovCircle:Remove() end)
        pcall(function() hcFovCircle:Remove() end)
        pcall(function() oldAimFovCircle:Remove() end)
        pcall(function() ForceHitHighlightLine:Remove() end)
        pcall(function() Window:Destroy() end)
        StarterGui:SetCore("SendNotification", { Title = "doucr.Main", Text = "Unloaded.", Duration = 3 })
    end
})

pcall(function() Window:LoadConfig() end)

print("[doucr.Main] Airflow Rose FULL loaded!")
StarterGui:SetCore("SendNotification", { Title = "doucr.Main", Text = "Rose FULL loaded!", Duration = 4 })
