# da-hood
da hood gui
 
 Section:NewButton("Enable", "", function(state)
_G.KEY = "q"
_G.PART = "HumanoidRootPart"
_G.PRED = 0.125
_G.NOTIF = false
_G.ENABLED = true
_G.AIR = false
_G.AIRpart = "LowerTorso"
_G.View = false
getgenv().BenNotif = false
_G.DOT = true
_G.BOX = true
local CC = game:GetService"Workspace".CurrentCamera
    local Plr
    local enabled = falseWD
    --local accomidationfactor = 0.13880--
    local mouse = game.Players.LocalPlayer:GetMouse()
    local placemarker = Instance.new("Part", game.Workspace)
 
        function makemarker(Parent, Adornee, Color, Size, Size2)
        local e = Instance.new("BillboardGui", Parent)
        e.Name = "PP"
        e.Adornee = Adornee
        e.Size = UDim2.new(Size, Size2, Size, Size2)
        e.AlwaysOnTop = _G.DOT
        local a = Instance.new("Frame", e)
        if _G.DOT == true then
        a.Size = UDim2.new(1, 0, 1, 0)
        else
        a.Size = UDim2.new(0, 0, 0, 0)
        end
        if _G.DOT == true then
        a.Transparency = 0
        a.BackgroundTransparency = 0
        else
        a.Transparency = 1
        a.BackgroundTransparency = 1
        end
        a.BackgroundColor3 = Color
        local g = Instance.new("UICorner", a)
        if _G.DOT == false then
        g.CornerRadius = UDim.new(0, 0)
        else
        g.CornerRadius = UDim.new(50, 50) 
        end
        return(e)
    end
 
 
    local data = game.Players:GetPlayers()
    function noob(player)
        local character
        repeat wait() until player.Character
        local handler = makemarker(guimain, player.Character:WaitForChild(_G.PART), Color3.fromRGB(238, 130, 238), 0.3, 3)
        handler.Name = player.Name
        player.CharacterAdded:connect(function(Char) handler.Adornee = Char:WaitForChild(_G.PART) end)
 
 
        spawn(function()
            while wait() do
                if player.Character then
                    TextLabel.Text = player.Name..tostring(player:WaitForChild("leaderstats").Wanted.Value).." | "..tostring(math.floor(player.Character:WaitForChild("Humanoid").Health))
                end
            end
        end)
    end
 
    for i = 1, #data do
        if data[i] ~= game.Players.LocalPlayer then
            noob(data[i])
        end
    end
 
    game.Players.PlayerAdded:connect(function(Player)
        noob(Player)
    end)
 
    getgenv().spawn(function()
        placemarker.Anchored = true
        placemarker.CanCollide = false
        if _G.BOX == true then
        placemarker.Size = Vector3.new(7, 7, 7)
        else
        placemarker.Size = Vector3.new(0, 0, 0)
        end
        placemarker.Transparency = 0.7
        if _G.DOT then
        makemarker(placemarker, placemarker, Color3.fromRGB(45, 80, 150), 0.3, 3)
        end
    end)
 
    game.Players.LocalPlayer:GetMouse().KeyDown:Connect(function(k)
        if k == _G.KEY and _G.ENABLED then
            if _G.View then
	game.Workspace.CurrentCamera.CameraSubject = game.Players.LocalPlayer.Character
end
            if enabled == true then
                enabled = false
                       if getgenv().BenNotif then
    sound1:Play()
end
                if _G.NOTIF == true then
                    Plr = getClosestPlayerToCursor()
                game.StarterGui:SetCore("SendNotification", {
                    Title = "EPL";
                    Text = "Unlocked",
                    Duration = 3
                })
            end
            else
                Plr = getClosestPlayerToCursor()
                if _G.View then
                        	game.Workspace.CurrentCamera.CameraSubject = Plr.Character
                end
                    	if getgenv().BenNotif then
                            sound:Play()
                        end
                enabled = true
                if _G.NOTIF == true then
 
                    game.StarterGui:SetCore("SendNotification", {
                        Title = "EPL";
                        Text = ""..tostring(Plr.Character.Humanoid.DisplayName),
                        Duration = 3
                    })
 
                end
            end
        end
    end)
    function getClosestPlayerToCursor()
        local closestPlayer
        local shortestDistance = math.huge
 
        for i, v in pairs(game.Players:GetPlayers()) do
            if v ~= game.Players.LocalPlayer and v.Character and v.Character:FindFirstChild("Humanoid") and v.Character.Humanoid.Health ~= 0 and v.Character:FindFirstChild("HumanoidRootPart") then
                local pos = CC:WorldToViewportPoint(v.Character.PrimaryPart.Position)
                local magnitude = (Vector2.new(pos.X, pos.Y) - Vector2.new(mouse.X, mouse.Y)).magnitude
                if magnitude < shortestDistance then
                    closestPlayer = v
                    shortestDistance = magnitude
                end
            end
        end
        return closestPlayer
    end
 
    game:GetService"RunService".Stepped:connect(function()
        if enabled and Plr.Character and Plr.Character:FindFirstChild("HumanoidRootPart") then
            placemarker.CFrame = CFrame.new(Plr.Character[_G.PART].Position + (Plr.Character[_G.PART].Velocity * _G.PRED))
        else
            placemarker.CFrame = CFrame.new(0, 9999, 0)
        end
    end)
 
    local mt = getrawmetatable(game)
    local old = mt.__namecall
    setreadonly(mt, false)
    mt.__namecall = newcclosure(function(...)
        local args = {...}
        if enabled and getnamecallmethod() == "FireServer" and args[2] == "UpdateMousePos" then
            args[3] = (Plr.Character[_G.PART].Position + (Plr.Character[_G.PART].Velocity * _G.PRED))
            return old(unpack(args))
        end
        return old(...)
    end)
    if _G.AIR == true then
 
        if Plr.Character.Humanoid.Jump == true and Plr.Character.Humanoid.FloorMaterial == Enum.Material.Air then
            _G.AIRpart = "RightFoot"
        else
            Plr.Character:WaitForChild("Humanoid").StateChanged:Connect(function(old,new)
                if new == Enum.HumanoidStateType.Freefall then
                _G.AIRpart = "RightFoot"
                else
                    _G.AIRpart = "LowerTorso"
                end
            end)
        end
    end
end)
Section:NewTextBox("Key", "", function(txt)
	_G.KEY = txt
end)
Section:NewTextBox("Prediction", "", function(txt)
	_G.PRED = txt
end)
Section:NewDropdown("Aim Part", "", {"Head", "UpperTorso", "HumanoidRootPart", "LowerTorso"}, function(arg)
    _G.PART = ""..arg
end)
local Section = Tab:NewSection("Toggles")
Section:NewToggle("Notification", "", function(state)
    _G.NOTIF = state
end)
Section:NewToggle("Ben Notif Sound", "", function(state)
    getgenv().BenNotif = state
end)
Section:NewToggle("ViewPlayer", "", function(state)
    _G.View = state
end)
Section:NewToggle("AirShotFunction", "", function(state)
    _G.AIR = state
end)
Section:NewDropdown("AirShot Part", "", {"Head", "UpperTorso", "HumanoidRootPart", "LowerTorso"}, function(arg)
    _G.AIRpart = ""..arg
end)
local Camlock = Window:NewTab("Camlock")
 
local Ik = Camlock:NewSection("Camlock")
 
Ik:NewButton("Enable", "", function(state)
 getgenv().AimPart = "HumanoidRootPart" -- For R15 Games: {UpperTorso, LowerTorso, HumanoidRootPart, Head} | For R6 Games: {Head, Torso, HumanoidRootPart}  
    getgenv().AimlockKey = "q"
    getgenv().AimRadius = 30 -- How far away from someones character you want to lock on at
    getgenv().ThirdPerson = true 
    getgenv().FirstPerson = true
    getgenv().TeamCheck = false -- Check if Target is on your Team (True means it wont lock onto your teamates, false is vice versa) (Set it to false if there are no teams)
    getgenv().PredictMovement = true -- Predicts if they are moving in fast velocity (like jumping) so the aimbot will go a bit faster to match their speed 
    getgenv().PredictionVelocity = 7.5
    getgenv().Smoothness = false
    getgenv().SmoothnessAmount = 0.022
    getgenv().AOR = false
    getgenv().AORpart = "LowerTorso"
    local Players, Uis, RService, SGui = game:GetService"Players", game:GetService"UserInputService", game:GetService"RunService", game:GetService"StarterGui";
    local Client, Mouse, Camera, CF, RNew, Vec3, Vec2 = Players.LocalPlayer, Players.LocalPlayer:GetMouse(), workspace.CurrentCamera, CFrame.new, Ray.new, Vector3.new, Vector2.new;
    local Aimlock, MousePressed, CanNotify = true, false, false;
    local AimlockTarget;
    local OldPre;
 
 
 
    getgenv().WorldToViewportPoint = function(P)
        return Camera:WorldToViewportPoint(P)
    end
 
    getgenv().WorldToScreenPoint = function(P)
        return Camera.WorldToScreenPoint(Camera, P)
    end
 
    getgenv().GetObscuringObjects = function(T)
        if T and T:FindFirstChild(getgenv().AimPart) and Client and Client.Character:FindFirstChild("Head") then 
            local RayPos = workspace:FindPartOnRay(RNew(
                T[getgenv().AimPart].Position, Client.Character.Head.Position)
            )
            if RayPos then return RayPos:IsDescendantOf(T) end
        end
    end
 
    getgenv().GetNearestTarget = function()
        -- Credits to whoever made this, i didnt make it, and my own mouse2plr function kinda sucks
        local players = {}
        local PLAYER_HOLD  = {}
        local DISTANCES = {}
        for i, v in pairs(Players:GetPlayers()) do
            if v ~= Client then
                table.insert(players, v)
            end
        end
        for i, v in pairs(players) do
            if v.Character ~= nil then
                local AIM = v.Character:FindFirstChild("Head")
                if getgenv().TeamCheck == true and v.Team ~= Client.Team then
                    local DISTANCE = (v.Character:FindFirstChild("Head").Position - game.Workspace.CurrentCamera.CFrame.p).magnitude
                    local RAY = Ray.new(game.Workspace.CurrentCamera.CFrame.p, (Mouse.Hit.p - game.Workspace.CurrentCamera.CFrame.p).unit * DISTANCE)
                    local HIT,POS = game.Workspace:FindPartOnRay(RAY, game.Workspace)
                    local DIFF = math.floor((POS - AIM.Position).magnitude)
                    PLAYER_HOLD[v.Name .. i] = {}
                    PLAYER_HOLD[v.Name .. i].dist= DISTANCE
                    PLAYER_HOLD[v.Name .. i].plr = v
                    PLAYER_HOLD[v.Name .. i].diff = DIFF
                    table.insert(DISTANCES, DIFF)
                elseif getgenv().TeamCheck == false and v.Team == Client.Team then 
                    local DISTANCE = (v.Character:FindFirstChild("Head").Position - game.Workspace.CurrentCamera.CFrame.p).magnitude
                    local RAY = Ray.new(game.Workspace.CurrentCamera.CFrame.p, (Mouse.Hit.p - game.Workspace.CurrentCamera.CFrame.p).unit * DISTANCE)
                    local HIT,POS = game.Workspace:FindPartOnRay(RAY, game.Workspace)
                    local DIFF = math.floor((POS - AIM.Position).magnitude)
                    PLAYER_HOLD[v.Name .. i] = {}
                    PLAYER_HOLD[v.Name .. i].dist= DISTANCE
                    PLAYER_HOLD[v.Name .. i].plr = v
                    PLAYER_HOLD[v.Name .. i].diff = DIFF
                    table.insert(DISTANCES, DIFF)
                end
            end
        end
 
        if unpack(DISTANCES) == nil then
            return nil
        end
 
        local L_DISTANCE = math.floor(math.min(unpack(DISTANCES)))
        if L_DISTANCE > getgenv().AimRadius then
            return nil
        end
 
        for i, v in pairs(PLAYER_HOLD) do
            if v.diff == L_DISTANCE then
                return v.plr
            end
        end
        return nil
    end
 
    Mouse.KeyDown:Connect(function(a)
        if not (Uis:GetFocusedTextBox()) then 
            if a == AimlockKey and AimlockTarget == nil then
                pcall(function()
                    if MousePressed ~= true then MousePressed = true end 
                    local Target;Target = GetNearestTarget()
                    if Target ~= nil then 
                        AimlockTarget = Target
                    end
                end)
            elseif a == AimlockKey and AimlockTarget ~= nil then
                if AimlockTarget ~= nil then AimlockTarget = nil end
                if MousePressed ~= false then 
                    MousePressed = false 
                end
            end
        end
    end)
 
    RService.RenderStepped:Connect(function()
        if getgenv().ThirdPerson == true and getgenv().FirstPerson == true then 
            if (Camera.Focus.p - Camera.CoordinateFrame.p).Magnitude > 1 or (Camera.Focus.p - Camera.CoordinateFrame.p).Magnitude <= 1 then 
                CanNotify = true 
            else 
                CanNotify = false 
            end
        elseif getgenv().ThirdPerson == true and getgenv().FirstPerson == false then 
            if (Camera.Focus.p - Camera.CoordinateFrame.p).Magnitude > 1 then 
                CanNotify = true 
            else 
                CanNotify = false 
            end
        elseif getgenv().ThirdPerson == false and getgenv().FirstPerson == true then 
            if (Camera.Focus.p - Camera.CoordinateFrame.p).Magnitude <= 1 then 
                CanNotify = true 
            else 
                CanNotify = false 
            end
        end
        if Aimlock == true and MousePressed == true then 
            if AimlockTarget and AimlockTarget.Character and AimlockTarget.Character:FindFirstChild(getgenv().AimPart) then 
                if getgenv().FirstPerson == true then
                    if CanNotify == true then
                        if getgenv().PredictMovement == true then
                            if getgenv().Smoothness == true then
                                --// The part we're going to lerp/smoothen \\--
                                local Main = CF(Camera.CFrame.p, AimlockTarget.Character[getgenv().AimPart].Position + AimlockTarget.Character[getgenv().AimPart].Velocity/PredictionVelocity)
 
                                --// Making it work \\--
                                Camera.CFrame = Camera.CFrame:Lerp(Main, getgenv().SmoothnessAmount, Enum.EasingStyle.Elastic, Enum.EasingDirection.InOut)
                            else
                                Camera.CFrame = CF(Camera.CFrame.p, AimlockTarget.Character[getgenv().AimPart].Position + AimlockTarget.Character[getgenv().AimPart].Velocity/PredictionVelocity)
                            end
                        elseif getgenv().PredictMovement == false then 
                            if getgenv().Smoothness == true then
                                --// The part we're going to lerp/smoothen \\--
                                local Main = CF(Camera.CFrame.p, AimlockTarget.Character[getgenv().AimPart].Position)
 
                                --// Making it work \\--
                                Camera.CFrame = Camera.CFrame:Lerp(Main, getgenv().SmoothnessAmount, Enum.EasingStyle.Elastic, Enum.EasingDirection.InOut)
                            else
                                Camera.CFrame = CF(Camera.CFrame.p, AimlockTarget.Character[getgenv().AimPart].Position)
                            end
                        end
                    end
                end
            end
        end
            if getgenv().AOR == true then
 
        if Plr.Character.Humanoid.Jump == true and Plr.Character.Humanoid.FloorMaterial == Enum.Material.Air then
            getgenv().AORpart = "RightFoot"
        else
            Plr.Character:WaitForChild("Humanoid").StateChanged:Connect(function(old,new)
                if new == Enum.HumanoidStateType.Freefall then
                getgenv().AORpart = "RightFoot"
                else
                    getgenv().AORpart = "LowerTorso"
                end
            end)
        end
    end
    end)
    end)
Ik:NewTextBox("Key", "", function(txt)
	getgenv().AimlockKey = txt
end)
Ik:NewTextBox("Prediction", "", function(txt)
	getgenv().PredictionVelocity = txt
end)
Ik:NewDropdown("Aim Part", "", {"Head", "UpperTorso", "HumanoidRootPart", "LowerTorso"}, function(arg)
    getgenv().AimPart = ""..arg
end)
local Ik = Camlock:NewSection("AirShotFunction")
 
Ik:NewToggle("AirShotFunction", "", function(state)
    getgenv().AOR = state
end)
 
Ik:NewDropdown("AirShot Part", "", {"Head", "UpperTorso", "HumanoidRootPart", "LowerTorso"}, function(arg)
    getgenv().AORpart = ""..arg
end)
local Ik = Camlock:NewSection("Streamable/smoothing")
 
Ik:NewToggle("Smoothing", "", function(state)
    getgenv().Smoothness = state
end)
Ik:NewTextBox("Smoothing Amount", "", function(txt)
	getgenv().SmoothnessAmount = txt
end)
local Esp = Window:NewTab("Esp")
local OB = Esp:NewSection("Esp")
 
OB:NewToggle("Enable ESP", "", function(arg)
    ESP.Enabled = arg
end)
OB:NewToggle("ESP Boxes", "", function(arg)
    ESP.Boxes = arg
end)
OB:NewToggle("ESP Names", "", function(arg)
    ESP.Names = arg
end)
OB:NewToggle("ESP TeamMates", "", function(arg)
    ESP.TeamMates = arg
end)
OB:NewToggle("ESP TeamColor", "", function(arg)
    ESP.TeamColor = arg
end)
OB:NewToggle("ESP FaceCamera", "", function(arg)
    ESP.FaceCamera = arg
end)
OB:NewToggle("ESP Tracers", "", function(arg)
    ESP.Tracers = arg
end)
OB:NewColorPicker("ESP color", "", Color3.fromRGB(0,0,0), function(color)
    print(color)
ESP.Color = color
end)
local OJA = Window:NewTab("Chams")
local PP = OJA:NewSection("BodyChams And GunChams")
local PP = OJA:NewSection("BodyChams")
PP:NewToggle("ForceField Body Chams", "", function(callback)
    if callback then
        game.Players.LocalPlayer.Character.LeftHand.Material = "ForceField"
        game.Players.LocalPlayer.Character.RightHand.Material = "ForceField"
        game.Players.LocalPlayer.Character.LeftLowerArm.Material = "ForceField"
        game.Players.LocalPlayer.Character.RightLowerArm.Material = "ForceField"
        game.Players.LocalPlayer.Character.LeftUpperArm.Material = "ForceField"
        game.Players.LocalPlayer.Character.RightUpperArm.Material = "ForceField"
        game.Players.LocalPlayer.Character.LeftFoot.Material = "ForceField"
        game.Players.LocalPlayer.Character.RightFoot.Material = "ForceField"
        game.Players.LocalPlayer.Character.LeftLowerLeg.Material = "ForceField"
        game.Players.LocalPlayer.Character.RightLowerLeg.Material = "ForceField"
        game.Players.LocalPlayer.Character.UpperTorso.Material = "ForceField"
        game.Players.LocalPlayer.Character.LowerTorso.Material = "ForceField"
        game.Players.LocalPlayer.Character.LeftUpperLeg.Material = "ForceField"
        game.Players.LocalPlayer.Character.RightUpperLeg.Material = "ForceField"
        game.Players.LocalPlayer.Character.Head.Material = "ForceField"
    else
        game.Players.LocalPlayer.Character.LeftHand.Material = "Plastic"
        game.Players.LocalPlayer.Character.RightHand.Material = "Plastic"
        game.Players.LocalPlayer.Character.LeftLowerArm.Material = "Plastic"
        game.Players.LocalPlayer.Character.RightLowerArm.Material = "Plastic"
        game.Players.LocalPlayer.Character.LeftUpperArm.Material = "Plastic"
        game.Players.LocalPlayer.Character.RightUpperArm.Material = "Plastic"
        game.Players.LocalPlayer.Character.LeftFoot.Material = "Plastic"
        game.Players.LocalPlayer.Character.RightFoot.Material = "Plastic"
        game.Players.LocalPlayer.Character.LeftLowerLeg.Material = "Plastic"
        game.Players.LocalPlayer.Character.RightLowerLeg.Material = "Plastic"
        game.Players.LocalPlayer.Character.UpperTorso.Material = "Plastic"
        game.Players.LocalPlayer.Character.LowerTorso.Material = "Plastic"
        game.Players.LocalPlayer.Character.LeftUpperLeg.Material = "Plastic"
        game.Players.LocalPlayer.Character.RightUpperLeg.Material = "Plastic"
        game.Players.LocalPlayer.Character.Head.Material = "Plastic"
    end
end)
 
PP:NewColorPicker("BodyCham Color", "", Color3.fromRGB(0,0,0), function(color)
    game.Players.LocalPlayer.Character.LeftHand.Color = color
    game.Players.LocalPlayer.Character.RightHand.Color = color
    game.Players.LocalPlayer.Character.LeftLowerArm.Color = color
    game.Players.LocalPlayer.Character.RightLowerArm.Color = color
    game.Players.LocalPlayer.Character.LeftUpperArm.Color = color
    game.Players.LocalPlayer.Character.RightUpperArm.Color = color
    game.Players.LocalPlayer.Character.LeftFoot.Color = color
    game.Players.LocalPlayer.Character.RightFoot.Color = color
    game.Players.LocalPlayer.Character.LeftLowerLeg.Color = color
    game.Players.LocalPlayer.Character.RightLowerLeg.Color = color
    game.Players.LocalPlayer.Character.UpperTorso.Color = color
    game.Players.LocalPlayer.Character.LowerTorso.Color = color
    game.Players.LocalPlayer.Character.LeftUpperLeg.Color = color
    game.Players.LocalPlayer.Character.RightUpperLeg.Color = color
    game.Players.LocalPlayer.Character.Head.Color = color
end)
local PP = OJA:NewSection("GunChams")
PP:NewToggle("ForceField Gun Chams", "", function(callback)
    if callback then
        local Client = game.GetService(game, "Players").LocalPlayer
        Client.Character:FindFirstChildOfClass("Tool").Default.Material = Enum.Material.ForceField
        Client.Character:FindFirstChildOfClass("Tool").Default.BrickColor  = BrickColor.new(255, 255, 255)
    else
        local Client = game.GetService(game, "Players").LocalPlayer
        Client.Character:FindFirstChildOfClass("Tool").Default.Material = Enum.Material.Plastic
    end
end)
PP:NewColorPicker("GunChams Color", "", Color3.fromRGB(0,0,0), function(color)
game.Players.LocalPlayer.Character:FindFirstChildOfClass("Tool").Default.BrickColor = BrickColor.new(color)
    end)
local speed = Window:NewTab("Speed")
local rs = speed:NewSection("Go Fast")
rs:NewButton("Damage Fix", "", function()
    for _, v in pairs(game.Players.LocalPlayer.Character:GetChildren()) do
        if v:IsA("Script") and v.Name ~= "Health" and v.Name ~= "Sound" and v:FindFirstChild("LocalScript") then
            v:Destroy()
        end
    end
    game.Players.LocalPlayer.CharacterAdded:Connect(function(char)
        repeat
            wait()
        until game.Players.LocalPlayer.Character
        char.ChildAdded:Connect(function(child)
            if child:IsA("Script") then 
                wait(0.1)
                if child:FindFirstChild("LocalScript") then
                    child.LocalScript:FireServer()
                end
            end
        end)
    end)
end)
rs:NewButton("CFrame Speed (C)", "", function()
        repeat
        wait()
    until game:IsLoaded()
    local L_134_ = game:service('Players')
    local L_135_ = L_134_.LocalPlayer
    repeat
        wait()
    until L_135_.Character
    local L_136_ = game:service('UserInputService')
    local L_137_ = game:service('RunService')
    getgenv().Multiplier = 0.5
    local L_138_ = true
    local L_139_
    L_136_.InputBegan:connect(function(L_140_arg0)
        if L_140_arg0.KeyCode == Enum.KeyCode.LeftBracket then
            Multiplier = Multiplier + 0.01
            print(Multiplier)
            wait(0.2)
            while L_136_:IsKeyDown(Enum.KeyCode.LeftBracket) do
                wait()
                Multiplier = Multiplier + 0.01
                print(Multiplier)
            end
        end
        if L_140_arg0.KeyCode == Enum.KeyCode.RightBracket then
            Multiplier = Multiplier - 0.01
            print(Multiplier)
            wait(0.2)
            while L_136_:IsKeyDown(Enum.KeyCode.RightBracket) do
                wait()
                Multiplier = Multiplier - 0.01
                print(Multiplier)
            end
        end
        if L_140_arg0.KeyCode == Enum.KeyCode.C then
            L_138_ = not L_138_
            if L_138_ == true then
                repeat
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame + game.Players.LocalPlayer.Character.Humanoid.MoveDirection * Multiplier
                    game:GetService("RunService").Stepped:wait()
                until L_138_ == false
            end
        end
    end)
end)
rs:NewSlider("CFrame Speed ", "", 5, 0, function(s)
    getgenv().Multiplier = s
end)
 
local Jit = math.random(30, 90)
local Angle = 60
getgenv().Spinbot = true
getgenv().Jitter = true
getgenv().AntiAim1 = true
getgenv().AntiAim2 = true
getgenv().BlatantAA = true
getgenv().antilock = true
getgenv().antilockspeed = 0.18
local AA = Window:NewTab("AntiLock")
 
local Po = AA:NewSection("AntiLock")
 
Po:NewToggle("jitter", "", function(state)
getgenv().Jitter = state
if getgenv().Jitter == true then
    repeat
        task.wait()
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame =
                CFrame.new(game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame.Position) *
                CFrame.Angles(0, math.rad(Angle) + math.rad((math.random(1, 2) == 1 and Jit or -Jit)), 0)
            until getgenv().Jitter == false
        end
end)
Po:NewToggle("Spinbot", "", function(state)
getgenv().Spinbot = state
if getgenv().Spinbot == true then
repeat
    task.wait()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame =
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.Angles(0, math.rad(555), 0)
until getgenv().Spinbot == false
    end
end)
Po:NewToggle("Blatant Spinbot", "", function(state)
getgenv().AntiAim1 = state
if getgenv().AntiAim1 == true then
    repeat
        task.wait()
    game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity =
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame.lookVector * -250
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame =
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.Angles(0, math.rad(555), 0)
    until getgenv().AntiAim1 == false
    end
end)
Po:NewToggle("SlingShot AA (ASS)", "", function(state)
    getgenv().AntiAim2 = state
    if getgenv().AntiAim2 == true then
        repeat
        task.wait()
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame =
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0.999, 0)
        until getgenv().AntiAim2 == false
        end
end)
Po:NewToggle("Super Blatant FlySpin AA", "", function(state)
    getgenv().BlatantAA = state
    local Position = game.Players.LocalPlayer.Character.HumanoidRootPart.Position
if getgenv().BlatantAA == true  then
    repeat
    task.wait()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = (CFrame.new(Position) + Vector3.new(math.random(-15, 15), math.random(-15, 15), math.random(-15, 15))) * CFrame.Angles(math.rad(math.random(-180, 180)), math.rad(math.random(-180, 180)), math.rad(math.random(-180, 180)))
until getgenv().BlatantAA == false
end
end)
Po:NewToggle("Normal AA", "", function(state)
getgenv().antilock = state
if getgenv().antilock == true then
    repeat
        task.wait()
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame =
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame +
                -game.Players.LocalPlayer.Character.Humanoid.MoveDirection * getgenv().antilockspeed
until getgenv().antilock == false
end
end)
Po:NewTextBox("Normal AA Speed", "", function(state)
	getgenv().antilockspeed = state
end)
local GUI = Window:NewTab("Gui Toggle")
 
local Al = GUI:NewSection("Gui Toggle")
 
Al:NewKeybind("Toggle GUI", "", Enum.KeyCode.RightShift, function()
    Library.ToggleUI()
end)
