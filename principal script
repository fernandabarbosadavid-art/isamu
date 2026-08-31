-- ==========================================================
-- UNIVERSAL MULTI-GAME HUB V33.0 - GLOBAL DISCORD SERVER TAB ADDED
-- UI Library: Fluent-modded (FluentPro by StyearX)
-- Supported Games: Evade, Arsenal, Teen Titans, FNP, Funky Friday, Prison Life + UNIVERSAL
-- Created by isamu
-- ==========================================================

if _G.Universal_Hub_Cleanup then
    pcall(_G.Universal_Hub_Cleanup)
    _G.Universal_Hub_Cleanup = nil
end

local Connections = {}
local function track(conn)
    table.insert(Connections, conn)
    return conn
end

-- ----------------------------------------------------------
-- 1. UNC EXECUTOR COMPATIBILITY TEST
-- ----------------------------------------------------------
local UNCSummary = {
    Passed = 0,
    Failed = 0,
    Total  = 0,
    Score  = 0,
    Details = {}
}

local function runUNCTests()
    local tests = {
        { name = "game:HttpGet", test = function() return typeof(game.HttpGet) == "function" end },
        { name = "getcustomasset / customasset", test = function() return (getcustomasset or getsynasset or customasset) ~= nil end },
        { name = "writefile", test = function() return (writefile or (syn and syn.writefile)) ~= nil end },
        { name = "readfile", test = function() return (readfile or (syn and syn.readfile)) ~= nil end },
        { name = "isfile", test = function() return (isfile or (syn and syn.isfile)) ~= nil end },
        { name = "delfile", test = function() return (delfile or (syn and syn.delfile)) ~= nil end },
        { name = "hookmetamethod", test = function() return (hookmetamethod or (hookfunction and getrawmetatable)) ~= nil end },
        { name = "newcclosure", test = function() return type(newcclosure) == "function" end },
        { name = "getrawmetatable", test = function() return type(getrawmetatable) == "function" end },
        { name = "setreadonly", test = function() return type(setreadonly) == "function" or (make_writeable ~= nil and make_readonly ~= nil) end },
        { name = "getnamecallmethod", test = function() return type(getnamecallmethod) == "function" end },
        { name = "mousemoverel", test = function() return type(mousemoverel) == "function" or (Input and type(Input.MouseMove) == "function") end },
        { name = "Drawing API", test = function() return type(Drawing) == "table" and type(Drawing.new) == "function" end },
        { name = "Highlight ESP Support", test = function() local h = Instance.new("Highlight"); local ok = (h ~= nil); if h then h:Destroy() end; return ok end },
        { name = "getgc", test = function() return type(getgc) == "function" end },
        { name = "getupvalues", test = function() return type(getupvalues) == "function" or (debug and type(debug.getupvalues) == "function") end },
        { name = "VirtualInputManager / keypress", test = function() return game:GetService("VirtualInputManager") ~= nil or type(keypress) == "function" end },
        { name = "fireclickdetector", test = function() return type(fireclickdetector) == "function" end },
        { name = "firetouchinterest", test = function() return type(firetouchinterest) == "function" end },
        { name = "setclipboard", test = function() return (setclipboard or (syn and syn.write_clipboard) or toclipboard) ~= nil end },
    }

    UNCSummary.Total = #tests
    for _, item in ipairs(tests) do
        local success, result = pcall(item.test)
        local passed = success and result == true
        if passed then
            UNCSummary.Passed = UNCSummary.Passed + 1
        else
            UNCSummary.Failed = UNCSummary.Failed + 1
        end
        table.insert(UNCSummary.Details, { Name = item.name, Passed = passed })
    end
    UNCSummary.Score = math.floor((UNCSummary.Passed / UNCSummary.Total) * 100)
end

runUNCTests()

-- ----------------------------------------------------------
-- 2. AUTOMATIC GAME DETECTION
-- ----------------------------------------------------------
local PlaceId = game.PlaceId
local CurrentGame = "Universal (Any Game)"

if PlaceId == 9872472334 or PlaceId == 10662095230 or PlaceId == 10662070389 or PlaceId == 10662040689 or PlaceId == 11468159863 then
    CurrentGame = "Evade"
elseif PlaceId == 286090429 or PlaceId == 3016661674 then
    CurrentGame = "Arsenal"
elseif PlaceId == 3082002798 then
    CurrentGame = "Teen Titans"
elseif PlaceId == 97923943452128 or PlaceId == 10258128048 then
    CurrentGame = "Friday Night Partying"
elseif PlaceId == 6447798030 then
    CurrentGame = "Funky Friday"
elseif PlaceId == 155615604 then
    CurrentGame = "Prison Life"
end

-- ----------------------------------------------------------
-- 3. FLUENT UI & SERVICES LOAD
-- ----------------------------------------------------------
local Fluent = loadstring(game:HttpGet("https://raw.githubusercontent.com/StyearX/Fluent-modded/main/dist/main.lua"))()

local Players           = game:GetService("Players")
local RunService        = game:GetService("RunService")
local UserInputService  = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService      = game:GetService("TweenService")
local VirtualUser       = game:GetService("VirtualUser")
local Workspace         = game:GetService("Workspace")
local Camera            = Workspace.CurrentCamera
local LocalPlayer       = Players.LocalPlayer
local PlayerGui         = LocalPlayer:WaitForChild("PlayerGui")
local Mouse             = LocalPlayer:GetMouse()

local DefaultTheme = "Blood Red"
if CurrentGame == "Evade" then
    DefaultTheme = "Midnight"
elseif CurrentGame == "Teen Titans" or CurrentGame == "Friday Night Partying" or CurrentGame == "Funky Friday" then
    DefaultTheme = "Amethyst"
elseif CurrentGame == "Prison Life" then
    DefaultTheme = "Sapphire"
elseif CurrentGame:find("Universal") then
    DefaultTheme = "Darker"
end

-- ----------------------------------------------------------
-- 4. DYNAMIC BANNER BY CATEGORY / GAME
-- ----------------------------------------------------------
local CustomImageURL = "https://i.pinimg.com/736x/db/49/b8/db49b858fe50e8db95595bd82d27fa4a.jpg"
local ImageFileName  = "aicha_banner_universal_v14.jpg"

if not CurrentGame:find("Universal") then
    CustomImageURL = "https://i.pinimg.com/736x/73/db/be/73dbbe30272cb805e6195f997475a91d.jpg"
    ImageFileName  = "aicha_banner_known_v15_4.jpg"
end

if CurrentGame == "Friday Night Partying" or CurrentGame == "Funky Friday" then
    CustomImageURL = "https://i.pinimg.com/736x/2a/51/67/2a5167ed1c514a55ce039d5de214f045.jpg"
    ImageFileName  = "aicha_banner_fnf_v15_5.jpg"
end

local CustomImageAsset = nil

pcall(function()
    local getAsset = getcustomasset or getsynasset or customasset
    local writeFile = writefile or (syn and syn.writefile)
    local isFile = isfile or (syn and syn.isfile)

    if getAsset and writeFile and isFile then
        if not isFile(ImageFileName) then
            local data = game:HttpGet(CustomImageURL)
            if data and #data > 0 then
                writeFile(ImageFileName, data)
            end
        end
        if isFile(ImageFileName) then
            CustomImageAsset = getAsset(ImageFileName)
        end
    end
end)

-- ----------------------------------------------------------
-- 5. MAIN WINDOW (CREATED BY ISAMU)
-- ----------------------------------------------------------
local Window = Fluent:CreateWindow({
    Title            = "Universal Game Hub",
    SubTitle         = "by isamu",
    Version          = "v33.0",
    TabWidth         = 160,
    Size             = UDim2.fromOffset(580, 520),
    Acrylic          = false,
    Theme            = DefaultTheme,
    MinimizeKey      = Enum.KeyCode.RightControl,
    UserInfoTop      = true,
    UserInfoTitle    = "Created by isamu",
    UserInfoSubtitle = LocalPlayer.DisplayName or LocalPlayer.Name,
    Tags = {
        { Text = CurrentGame, Color = (CurrentGame == "Evade" and Color3.fromRGB(255, 60, 0)) or (CurrentGame == "Arsenal" and Color3.fromRGB(220, 20, 60)) or Color3.fromRGB(160, 50, 255) },
        { Text = string.format("UNC Test: %d%%", UNCSummary.Score), Color = (UNCSummary.Score >= 80 and Color3.fromRGB(0, 220, 100)) or Color3.fromRGB(255, 160, 0) },
    }
})

local Tabs = {
    UNCTest   = Window:AddTab({ Title = "UNC Exec Test", Icon = "check-circle" }),
    Games     = Window:AddTab({ Title = "Games",         Icon = "layout-grid" }),
    Discord   = Window:AddTab({ Title = "Discord Server",Icon = "share-2" }),
    Settings  = Window:AddTab({ Title = "Settings",      Icon = "settings" })
}

-- Inject Top Banner into Games Tab
if CustomImageAsset and Tabs.Games and Tabs.Games.Container then
    local bannerHolder = Instance.new("Frame")
    bannerHolder.Name = "IsamuBannerHolder"
    bannerHolder.Size = UDim2.new(1, 0, 0, 180)
    bannerHolder.BackgroundTransparency = 1
    bannerHolder.LayoutOrder = -1
    bannerHolder.Parent = Tabs.Games.Container

    local bannerImg = Instance.new("ImageLabel")
    bannerImg.Name = "IsamuBanner"
    bannerImg.Size = UDim2.new(1, 0, 1, 0)
    bannerImg.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
    bannerImg.Image = CustomImageAsset
    bannerImg.ScaleType = Enum.ScaleType.Crop
    bannerImg.Parent = bannerHolder

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = bannerImg

    local stroke = Instance.new("UIStroke")
    stroke.Color = (CurrentGame == "Evade" and Color3.fromRGB(255, 100, 0)) or Color3.fromRGB(160, 50, 255)
    stroke.Thickness = 1.6
    stroke.Transparency = 0.2
    stroke.Parent = bannerImg
end

-- ===========================================================
-- GLOBAL DISCORD SERVER TAB (AVAILABLE IN ALL MODES)
-- ===========================================================
local DiscordInviteLink = "https://discord.gg/6F2WquY95e"

Tabs.Discord:AddSection("Join Our Official Community")

Tabs.Discord:AddParagraph({
    Title = "Discord Server Invite Link",
    Content = string.format("Join our Discord server for script updates, support, and community discussions!\n\nLink: %s", DiscordInviteLink)
})

Tabs.Discord:AddButton({
    Title = "Copy Discord Link to Clipboard",
    Description = "Click to copy https://discord.gg/6F2WquY95e",
    Callback = function()
        local setClip = setclipboard or (syn and syn.write_clipboard) or toclipboard
        if setClip then
            setClip(DiscordInviteLink)
            Fluent:Notify({
                Title = "Discord Server",
                Content = "Discord invite link copied to clipboard!",
                Duration = 4
            })
        else
            Fluent:Notify({
                Title = "Discord Server",
                Content = "Your executor does not support setclipboard.",
                Duration = 4
            })
        end
    end
})

-- ===========================================================
-- UNC EXECUTOR COMPATIBILITY DIAGNOSTICS
-- ===========================================================
Tabs.UNCTest:AddSection("Executor Compatibility Test Result")

Tabs.UNCTest:AddParagraph({
    Title = string.format("Overall Score: %d%% (%d/%d Passed)", UNCSummary.Score, UNCSummary.Passed, UNCSummary.Total),
    Content = string.format("Passed Functions: %d\nMissing Functions: %d\nNote: The test was executed prior to initialization to ensure stability.", UNCSummary.Passed, UNCSummary.Failed)
})

Tabs.UNCTest:AddSection("Detailed Breakdown")
for _, detail in ipairs(UNCSummary.Details) do
    local statusText = detail.Passed and "PASSED" or "NOT SUPPORTED"
    Tabs.UNCTest:AddParagraph({
        Title = string.format("%s: %s", detail.Name, statusText),
        Content = detail.Passed and "This feature is fully operational on your executor." or "Your executor lacks this API. Fallback mechanisms will be used."
    })
end

Tabs.UNCTest:AddButton({
    Title = "Rerun UNC Test",
    Callback = function()
        runUNCTests()
        Fluent:Notify({
            Title = "UNC Tester",
            Content = string.format("Test completed. Success Rate: %d%%", UNCSummary.Score),
            Duration = 4
        })
    end
})

-- Games Tab Info
Tabs.Games:AddSection("Current Game Status")
Tabs.Games:AddParagraph({
    Title = "Detected Game",
    Content = string.format("Active Mode: %s\nPlace ID: %d\nPlayer: %s\nAuthor: isamu", CurrentGame, PlaceId, LocalPlayer.Name)
})

Tabs.Games:AddSection("Integrated Games List")
Tabs.Games:AddParagraph({
    Title = "1. Evade (Nextbots)",
    Content = "Features: TPWalk Speed Engine (Selectable Speed), Bhop Engine, Anti-AFK Engine, Nextbots ESP Vision."
})
Tabs.Games:AddParagraph({
    Title = "2. Friday Night Partying & Funky Friday (FNF)",
    Content = "Features: Engine Native AutoPlayer, Anti-Bad Notes, Kawi Core Engine, 100% Sick Hits."
})
Tabs.Games:AddParagraph({
    Title = "3. Prison Life",
    Content = "Features: Auto Arrest Tween, Aimbot & Aim Assist, Guards/Inmates/Criminals ESP, Speed, NoClip."
})
Tabs.Games:AddParagraph({
    Title = "4. Universal Module (Any Game)",
    Content = "Features: Universal Aimbot, Hitbox Expander, ESP Chams + HP/Names, Speed, Jump, NoClip."
})
Tabs.Games:AddParagraph({
    Title = "5. Arsenal (FPS)",
    Content = "Features: Aim Assist, Silent Aim, Auto Look, Gun Mods, ESP Chams."
})
Tabs.Games:AddParagraph({
    Title = "6. Teen Titans Battlegrounds",
    Content = "Features: Auto Face Aimbot, Camera Lock, Hitbox Expander R6, ESP with Health Bar."
})

-- ===========================================================
-- GLOBAL CONTROL VARIABLES & CLEANUP
-- ===========================================================
local FOVCircles      = {}
local SilentHooked    = false
local OldIndex        = nil
local ESPData         = {}

local function isAlive(char)
    if not char or not char.Parent then return false end
    local h = char:FindFirstChild("Humanoid")
    return h and h.Health > 0 and h:GetState() ~= Enum.HumanoidStateType.Dead
end

local function cleanupESP(plr)
    local d = ESPData[plr.UserId]
    if not d then return end
    for _, c in ipairs(d.connections) do
        if c and c.Connected then c:Disconnect() end
    end
    if d.highlight and d.highlight.Parent then d.highlight:Destroy() end
    if d.billboard and d.billboard.Parent then d.billboard:Destroy() end
    ESPData[plr.UserId] = nil
end

-- ===========================================================
-- GLOBAL ANTI-AFK ENGINE
-- ===========================================================
local AntiAFK_Connection = nil
local function enableAntiAFK(state)
    if state then
        if not AntiAFK_Connection then
            AntiAFK_Connection = track(LocalPlayer.Idled:Connect(function()
                VirtualUser:CaptureController()
                VirtualUser:ClickButton2(Vector2.zero)
            end))
        end
    else
        if AntiAFK_Connection then
            AntiAFK_Connection:Disconnect()
            AntiAFK_Connection = nil
        end
    end
end
enableAntiAFK(true)

-- ===========================================================
-- MODULE: EVADE (MOVEMENT & ESP ONLY - NO AUTOFARM)
-- ===========================================================
if CurrentGame == "Evade" then
    Tabs.EvadeMove  = Window:AddTab({ Title = "Movement",  Icon = "activity" })
    Tabs.EvadeESP   = Window:AddTab({ Title = "ESP Vision", Icon = "eye" })

    local Evade_Config = {
        TPWalkEnabled   = false,
        TPWalkSpeed     = 45,

        BhopEnabled     = false,
        BhopSpeed       = 45,

        ESP_Nextbots    = true,
        AntiAFK         = true,
    }

    -- 1. TPWalk Speed Engine (Bypasses Evade Speed Lock)
    track(RunService.RenderStepped:Connect(function(dt)
        if Evade_Config.TPWalkEnabled and LocalPlayer.Character then
            local hum = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            local hrp = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            if hum and hrp and hum.MoveDirection.Magnitude > 0 then
                local delta = hum.MoveDirection * (Evade_Config.TPWalkSpeed * dt)
                hrp.CFrame = hrp.CFrame + delta
            end
        end
    end))

    -- 2. Bhop (Bunny Hop) Engine
    track(UserInputService.JumpRequest:Connect(function()
        if Evade_Config.BhopEnabled and LocalPlayer.Character then
            local hum = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            if hum then
                hum:ChangeState(Enum.HumanoidStateType.Jumping)
            end
        end
    end))

    track(RunService.Stepped:Connect(function()
        if Evade_Config.BhopEnabled and LocalPlayer.Character then
            local hum = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            local hrp = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            if hum and hrp and hum.MoveDirection.Magnitude > 0 then
                if hum:GetState() == Enum.HumanoidStateType.Freefall or hum:GetState() == Enum.HumanoidStateType.Jumping then
                    hrp.AssemblyLinearVelocity = Vector3.new(
                        hum.MoveDirection.X * Evade_Config.BhopSpeed,
                        hrp.AssemblyLinearVelocity.Y,
                        hum.MoveDirection.Z * Evade_Config.BhopSpeed
                    )
                end
            end
        end
    end))

    -- 3. Nextbots ESP
    local function createNextbotESP(model)
        if not model or not model:IsA("Model") or model == LocalPlayer.Character then return end
        local hrp = model:WaitForChild("HumanoidRootPart", 3) or model:WaitForChild("Torso", 3)
        if not hrp then return end

        local hl = Instance.new("Highlight")
        hl.Name = "Evade_Nextbot_Chams"
        hl.Adornee = model
        hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
        hl.FillColor = Color3.fromRGB(255, 0, 50)
        hl.OutlineColor = Color3.new(1, 1, 1)
        hl.FillTransparency = 0.3
        hl.OutlineTransparency = 0.1
        hl.Parent = model

        local bb = Instance.new("BillboardGui")
        bb.Name = "Evade_Nextbot_Tag"
        bb.Adornee = hrp
        bb.Size = UDim2.fromOffset(140, 40)
        bb.StudsOffset = Vector3.new(0, 3, 0)
        bb.AlwaysOnTop = true

        local lbl = Instance.new("TextLabel", bb)
        lbl.Size = UDim2.fromScale(1, 1)
        lbl.BackgroundTransparency = 1
        lbl.TextColor3 = Color3.fromRGB(255, 50, 50)
        lbl.TextStrokeTransparency = 0
        lbl.TextStrokeColor3 = Color3.new(0, 0, 0)
        lbl.Font = Enum.Font.GothamBold
        lbl.TextSize = 13
        lbl.Text = model.Name
        bb.Parent = hrp

        local conn = RunService.RenderStepped:Connect(function()
            if not model.Parent or not hrp.Parent then
                bb:Destroy()
                hl:Destroy()
                return
            end
            local dist = math.floor((Camera.CFrame.Position - hrp.Position).Magnitude)
            lbl.Text = string.format("NEXTBOT: %s [%dm]", model.Name, dist)
            hl.Enabled = Evade_Config.ESP_Nextbots
            bb.Enabled = Evade_Config.ESP_Nextbots
        end)
        track(conn)
    end

    local function scanEvadeNextbots()
        for _, folder in ipairs({ Workspace:FindFirstChild("Game"), Workspace:FindFirstChild("Bots"), Workspace }) do
            if folder then
                for _, child in ipairs(folder:GetChildren()) do
                    if child:IsA("Model") and (child.Name:find("Bot") or child.Name:find("Nextbot") or child:FindFirstChild("HumanoidRootPart")) then
                        if child ~= LocalPlayer.Character and not Players:GetPlayerFromCharacter(child) then
                            task.spawn(createNextbotESP, child)
                        end
                    end
                end
            end
        end
    end
    scanEvadeNextbots()
    track(Workspace.ChildAdded:Connect(function(child)
        if child:IsA("Model") and not Players:GetPlayerFromCharacter(child) then
            task.spawn(createNextbotESP, child)
        end
    end))

    -- EVADE MOVEMENT & ESP UI CONTROLS
    Tabs.EvadeMove:AddSection("TPWalk Speed (Bypasses Speed Lock)")
    Tabs.EvadeMove:AddToggle("ev_tpwalk", {
        Title = "Enable TPWalk Speed",
        Default = false,
        Callback = function(v) Evade_Config.TPWalkEnabled = v end
    })
    Tabs.EvadeMove:AddSlider("ev_tpwalkspeed", {
        Title = "Select WalkSpeed Multiplier",
        Default = 45, Min = 16, Max = 150, Rounding = 0,
        Description = "Choose custom walking speed in Evade!",
        Callback = function(v) Evade_Config.TPWalkSpeed = v end
    })

    Tabs.EvadeMove:AddSection("Bhop & Anti-AFK")
    Tabs.EvadeMove:AddToggle("ev_bhop", {
        Title = "Enable Bhop (Bunny Hop Engine)",
        Default = false,
        Callback = function(v) Evade_Config.BhopEnabled = v end
    })
    Tabs.EvadeMove:AddSlider("ev_bhopspeed", {
        Title = "Bhop Air Speed",
        Default = 45, Min = 20, Max = 120, Rounding = 0,
        Callback = function(v) Evade_Config.BhopSpeed = v end
    })
    Tabs.EvadeMove:AddToggle("ev_antiafk", {
        Title = "Enable Anti-AFK Engine (Prevents 20m Kick)",
        Default = true,
        Callback = function(v)
            Evade_Config.AntiAFK = v
            enableAntiAFK(v)
        end
    })

    Tabs.EvadeESP:AddSection("Evade ESP Vision")
    Tabs.EvadeESP:AddToggle("ev_espbot", {
        Title = "Nextbots Chams + Tag + Distance",
        Default = true,
        Callback = function(v) Evade_Config.ESP_Nextbots = v end
    })
end

-- ===========================================================
-- UNIVERSAL MODULE (ANY OTHER ROBLOX GAME)
-- ===========================================================
if CurrentGame:find("Universal") then
    Tabs.UniAim   = Window:AddTab({ Title = "Aimbot",   Icon = "crosshair" })
    Tabs.UniHB    = Window:AddTab({ Title = "Hitbox",   Icon = "box" })
    Tabs.UniESP   = Window:AddTab({ Title = "ESP",      Icon = "eye" })
    Tabs.UniMisc  = Window:AddTab({ Title = "Movement", Icon = "activity" })

    local UC = {
        AimbotEnabled = false,
        AimbotMode    = "Camera Lock",
        AimbotTarget  = "Head",
        AimbotSmooth  = 0.35,
        AimbotFOV     = 150,
        ShowFOV       = true,
        WallCheck     = true,
        TeamCheck     = true,

        HitboxEnabled = false,
        HitboxSize    = 10,
        HitboxTrans   = 0.6,

        ESP_Chams     = false,
        ESP_Names     = false,

        SpeedEnabled  = false,
        SpeedValue    = 24,
        JumpEnabled   = false,
        JumpValue     = 50,
        InfiniteJump  = false,
        NoClip        = false,
    }

    local UniFOV = Drawing.new("Circle")
    UniFOV.Thickness = 1.5; UniFOV.NumSides = 64; UniFOV.Radius = UC.AimbotFOV
    UniFOV.Filled = false; UniFOV.Color = Color3.fromRGB(0, 230, 255); UniFOV.Visible = false
    table.insert(FOVCircles, UniFOV)

    local function isUniEnemy(plr)
        if plr == LocalPlayer then return false end
        if not UC.TeamCheck then return true end
        if not plr.Team or not LocalPlayer.Team then return true end
        return plr.Team ~= LocalPlayer.Team
    end

    local function isUniVisible(part)
        local o = Camera.CFrame.Position
        local d = (part.Position - o)
        local p = RaycastParams.new()
        p.FilterType = Enum.RaycastFilterType.Exclude; p.IgnoreWater = true
        local fl = { Camera }
        if LocalPlayer.Character then table.insert(fl, LocalPlayer.Character) end
        p.FilterDescendantsInstances = fl
        local r = Workspace:Raycast(o, d, p)
        if r == nil then return true end
        return r.Instance and r.Instance:IsDescendantOf(part.Parent)
    end

    local function getClosestUniTarget()
        local best, bestD = nil, UC.AimbotFOV
        local mp = UserInputService:GetMouseLocation()

        for _, plr in ipairs(Players:GetPlayers()) do
            if isUniEnemy(plr) and isAlive(plr.Character) then
                local char = plr.Character
                local part = char:FindFirstChild(UC.AimbotTarget)
                          or char:FindFirstChild("Head")
                          or char:FindFirstChild("HumanoidRootPart")
                          or char:FindFirstChild("Torso")
                          or char:FindFirstChild("UpperTorso")
                if part then
                    if UC.WallCheck and not isUniVisible(part) then continue end
                    local sp, on = Camera:WorldToViewportPoint(part.Position)
                    if on then
                        local d = (Vector2.new(sp.X, sp.Y) - mp).Magnitude
                        if d < bestD then
                            bestD = d
                            best = part
                        end
                    end
                end
            end
        end
        return best, bestD
    end

    track(RunService.RenderStepped:Connect(function(dt)
        Camera = Workspace.CurrentCamera
        local mp = UserInputService:GetMouseLocation()

        UniFOV.Position = mp
        UniFOV.Radius   = UC.AimbotFOV
        UniFOV.Visible  = UC.ShowFOV and UC.AimbotEnabled

        if UC.AimbotEnabled then
            local target, dist = getClosestUniTarget()
            if target and target.Parent then
                local alpha = 1 - math.pow(1 - UC.AimbotSmooth, dt * 60)
                alpha = math.clamp(alpha, 0.01, 1)

                if UC.AimbotMode == "Camera Lock" then
                    Camera.CFrame = Camera.CFrame:Lerp(CFrame.new(Camera.CFrame.Position, target.Position), alpha)
                elseif UC.AimbotMode == "Mouse Smooth" and mousemoverel then
                    local sp = Camera:WorldToViewportPoint(target.Position)
                    local dx = (sp.X - mp.X) * alpha
                    local dy = (sp.Y - mp.Y) * alpha
                    mousemoverel(dx, dy)
                end
            end
        end

        if UC.HitboxEnabled then
            for _, plr in ipairs(Players:GetPlayers()) do
                if isUniEnemy(plr) and isAlive(plr.Character) then
                    local char = plr.Character
                    local sz = UC.HitboxSize
                    local vec = Vector3.new(sz, sz, sz)

                    for _, pName in ipairs({"HumanoidRootPart", "Head", "Torso", "UpperTorso"}) do
                        local part = char:FindFirstChild(pName)
                        if part then
                            if (part.Size - vec).Magnitude > 0.1 then
                                part.Size = vec
                                part.CanCollide = false
                                part.Transparency = UC.HitboxTrans
                            end
                        end
                    end
                end
            end
        end

        if UC.NoClip and LocalPlayer.Character then
            for _, p in ipairs(LocalPlayer.Character:GetDescendants()) do
                if p:IsA("BasePart") then p.CanCollide = false end
            end
        end
    end))

    local function createESP_Universal(plr)
        if plr == LocalPlayer then return end
        local function setup(char)
            if not char then return end
            cleanupESP(plr)
            local d = { connections = {}, highlight = nil, billboard = nil }
            ESPData[plr.UserId] = d

            local hrp = char:WaitForChild("HumanoidRootPart", 5) or char:WaitForChild("Head", 5)
            if not hrp then return end

            local hl = Instance.new("Highlight")
            hl.Name = "Uni_Chams"; hl.Adornee = char
            hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
            hl.FillColor = Color3.fromRGB(0, 200, 255)
            hl.OutlineColor = Color3.new(1, 1, 1); hl.FillTransparency = 0.4; hl.OutlineTransparency = 0.1
            hl.Enabled = UC.ESP_Chams and isUniEnemy(plr); hl.Parent = char; d.highlight = hl

            local head = char:WaitForChild("Head", 5)
            if head then
                local bb = Instance.new("BillboardGui")
                bb.Name = "Uni_Tag"; bb.Adornee = head; bb.Size = UDim2.fromOffset(130, 40)
                bb.StudsOffset = Vector3.new(0, 2.5, 0); bb.AlwaysOnTop = true
                bb.Enabled = UC.ESP_Names and isUniEnemy(plr)

                local lbl = Instance.new("TextLabel", bb)
                lbl.Size = UDim2.fromScale(1, 1); lbl.BackgroundTransparency = 1
                lbl.TextColor3 = Color3.fromRGB(0, 255, 200); lbl.TextStrokeTransparency = 0
                lbl.TextStrokeColor3 = Color3.new(0, 0, 0); lbl.Font = Enum.Font.GothamBold
                lbl.TextSize = 12; lbl.Text = plr.DisplayName or plr.Name
                bb.Parent = head; d.billboard = bb

                local uc = RunService.RenderStepped:Connect(function()
                    if not char.Parent or not head.Parent then bb.Enabled = false; hl.Enabled = false; return end
                    if isUniEnemy(plr) then
                        local hum = char:FindFirstChild("Humanoid")
                        local hp = hum and math.floor(hum.Health) or "?"
                        local dist = math.floor((Camera.CFrame.Position - head.Position).Magnitude)
                        lbl.Text = string.format("%s [%dHP] [%dm]", plr.DisplayName or plr.Name, hp, dist)

                        if hum then
                            local r = math.clamp(hum.Health / hum.MaxHealth, 0, 1)
                            lbl.TextColor3 = Color3.fromRGB(math.floor(255 * (1 - r)), math.floor(255 * r), 100)
                            hl.FillColor = lbl.TextColor3
                        end

                        bb.Enabled = UC.ESP_Names
                        hl.Enabled = UC.ESP_Chams
                    else
                        bb.Enabled = false; hl.Enabled = false
                    end
                end)
                table.insert(d.connections, uc); track(uc)
            end
        end
        if plr.Character then task.spawn(setup, plr.Character) end
        track(plr.CharacterAdded:Connect(function(c) task.spawn(setup, c) end))
    end

    for _, p in ipairs(Players:GetPlayers()) do task.spawn(createESP_Universal, p) end
    track(Players.PlayerAdded:Connect(function(p) task.spawn(createESP_Universal, p) end))
    track(Players.PlayerRemoving:Connect(function(p) cleanupESP(p) end))

    track(UserInputService.JumpRequest:Connect(function()
        if UC.InfiniteJump then
            local c = LocalPlayer.Character
            if c then local h = c:FindFirstChild("Humanoid"); if h then h:ChangeState(Enum.HumanoidStateType.Jumping) end end
        end
    end))

    track(RunService.Heartbeat:Connect(function()
        local c = LocalPlayer.Character
        if c then
            local h = c:FindFirstChild("Humanoid")
            if h then
                if UC.SpeedEnabled then h.WalkSpeed = UC.SpeedValue end
                if UC.JumpEnabled then
                    if h.UseJumpPower then h.JumpPower = UC.JumpValue else h.JumpHeight = UC.JumpValue end
                end
            end
        end
    end))

    Tabs.UniAim:AddSection("Universal Aimbot")
    Tabs.UniAim:AddToggle("u1", { Title = "Enable Aimbot", Default = false, Callback = function(v) UC.AimbotEnabled = v end })
    Tabs.UniAim:AddDropdown("u2", { Title = "Aim Mode", Values = { "Camera Lock", "Mouse Smooth" }, Default = "Camera Lock", Callback = function(v) UC.AimbotMode = v end })
    Tabs.UniAim:AddDropdown("u3", { Title = "Target Part", Values = { "Head", "HumanoidRootPart", "Torso" }, Default = "Head", Callback = function(v) UC.AimbotTarget = v end })
    Tabs.UniAim:AddSlider("u4", { Title = "Smoothness (Lerp)", Default = 0.35, Min = 0.05, Max = 1, Rounding = 2, Callback = function(v) UC.AimbotSmooth = v end })
    Tabs.UniAim:AddSlider("u5", { Title = "FOV Size", Default = 150, Min = 30, Max = 500, Rounding = 0, Callback = function(v) UC.AimbotFOV = v end })
    Tabs.UniAim:AddToggle("u6", { Title = "Show FOV", Default = true, Callback = function(v) UC.ShowFOV = v end })
    Tabs.UniAim:AddToggle("u7", { Title = "Wall Check", Default = true, Callback = function(v) UC.WallCheck = v end })
    Tabs.UniAim:AddToggle("u8", { Title = "Team Check", Default = true, Callback = function(v) UC.TeamCheck = v end })

    Tabs.UniHB:AddSection("Universal Hitbox Expander")
    Tabs.UniHB:AddToggle("hb1", { Title = "Enable Hitbox Expander", Default = false, Callback = function(v)
        UC.HitboxEnabled = v
        if not v then
            for _, p in ipairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character then
                    for _, pName in ipairs({"HumanoidRootPart", "Head", "Torso", "UpperTorso"}) do
                        local part = p.Character:FindFirstChild(pName)
                        if part then
                            if pName == "Head" then part.Size = Vector3.new(2, 1, 1); part.Transparency = 0
                            elseif pName == "HumanoidRootPart" then part.Size = Vector3.new(2, 2, 1); part.Transparency = 1
                            else part.Size = Vector3.new(2, 2, 1); part.Transparency = 0 end
                        end
                    end
                end
            end
        end
    end })
    Tabs.UniHB:AddSlider("hb2", { Title = "Box Size", Default = 10, Min = 4, Max = 30, Rounding = 0, Callback = function(v) UC.HitboxSize = v end })
    Tabs.UniHB:AddSlider("hb3", { Title = "Visual Transparency", Default = 0.6, Min = 0, Max = 0.95, Rounding = 2, Callback = function(v) UC.HitboxTrans = v end })

    Tabs.UniESP:AddSection("Universal ESP")
    Tabs.UniESP:AddToggle("e1", { Title = "Chams (Wall Highlights)", Default = false, Callback = function(v) UC.ESP_Chams = v end })
    Tabs.UniESP:AddToggle("e2", { Title = "Names + HP + Distance", Default = false, Callback = function(v) UC.ESP_Names = v end })

    Tabs.UniMisc:AddSection("Universal Movement")
    Tabs.UniMisc:AddToggle("m1", { Title = "Speed Hack", Default = false, Callback = function(v)
        UC.SpeedEnabled = v
        if not v and LocalPlayer.Character then
            local h = LocalPlayer.Character:FindFirstChild("Humanoid")
            if h then h.WalkSpeed = 16 end
        end
    end })
    Tabs.UniMisc:AddSlider("m2", { Title = "WalkSpeed", Default = 24, Min = 16, Max = 120, Rounding = 0, Callback = function(v) UC.SpeedValue = v end })
    Tabs.UniMisc:AddToggle("m3", { Title = "Super Jump", Default = false, Callback = function(v)
        UC.JumpEnabled = v
        if not v and LocalPlayer.Character then
            local h = LocalPlayer.Character:FindFirstChild("Humanoid")
            if h then if h.UseJumpPower then h.JumpPower = 50 else h.JumpHeight = 7.2 end end
        end
    end })
    Tabs.UniMisc:AddSlider("m4", { Title = "Jump Power", Default = 50, Min = 10, Max = 200, Rounding = 0, Callback = function(v) UC.JumpValue = v end })
    Tabs.UniMisc:AddToggle("m5", { Title = "Infinite Jump", Default = false, Callback = function(v) UC.InfiniteJump = v end })
    Tabs.UniMisc:AddToggle("m6", { Title = "NoClip", Default = false, Callback = function(v) UC.NoClip = v end })
end

-- ===========================================================
-- MODULE 1: ARSENAL (NO HITBOX EXPANDER)
-- ===========================================================
if CurrentGame == "Arsenal" then
    Tabs.AimAssist = Window:AddTab({ Title = "Aim Assist", Icon = "magnet" })
    Tabs.SilentAim = Window:AddTab({ Title = "Silent Aim", Icon = "ghost" })
    Tabs.AutoLook  = Window:AddTab({ Title = "Auto Look",  Icon = "crosshair" })
    Tabs.Guns      = Window:AddTab({ Title = "Gun Mods",   Icon = "swords" })
    Tabs.Visuals   = Window:AddTab({ Title = "ESP",        Icon = "eye" })
    Tabs.Misc      = Window:AddTab({ Title = "Movement",   Icon = "activity" })

    local C = {
        AimAssist    = false, AA_Strength = 0.12, AA_FOV = 120, AA_ShowFOV = true, AA_WallCheck = true,
        SilentAim    = false, SA_FOV = 180, SA_ShowFOV = false, SA_Part = "Head", SA_WallCheck = true, SA_HitChance = 100,
        AutoLook     = false, AL_Smooth = 0.5, AL_ShowFOV = false, AL_WallCheck = true,
        NoRecoil = true, NoSpread = true, FullAuto = true, FastReload = false, RapidFire = false,
        ESP_Chams = true, ESP_Names = true,
        SpeedEnabled = false, SpeedValue = 26, InfiniteJump = false,
    }

    local function makeFOV(r, col)
        local c = Drawing.new("Circle")
        c.Thickness = 1.5; c.NumSides = 64; c.Radius = r; c.Filled = false; c.Color = col; c.Visible = false
        table.insert(FOVCircles, c)
        return c
    end
    local AssistFOV   = makeFOV(C.AA_FOV, Color3.fromRGB(0, 255, 120))
    local SilentFOV   = makeFOV(C.SA_FOV, Color3.fromRGB(180, 50, 255))
    local AutoLookFOV = makeFOV(360, Color3.fromRGB(255, 50, 50))

    local function isEnemy(plr)
        if plr == LocalPlayer then return false end
        if not plr.Team or not LocalPlayer.Team then return true end
        if plr.Team.Name == "FFA" or LocalPlayer.Team.Name == "FFA" then return true end
        return plr.Team ~= LocalPlayer.Team
    end

    local function getTeamColor(plr)
        if not plr.Team then return Color3.fromRGB(255, 60, 60) end
        local n = plr.Team.Name
        if n == "TRC" then return Color3.fromRGB(255, 60, 60)
        elseif n == "TBC" then return Color3.fromRGB(60, 150, 255)
        elseif n == "TYC" then return Color3.fromRGB(255, 220, 50)
        elseif n == "TGC" then return Color3.fromRGB(60, 255, 80) end
        return Color3.fromRGB(255, 60, 60)
    end

    local function isVisible(part)
        local o = Camera.CFrame.Position
        local d = (part.Position - o)
        local p = RaycastParams.new()
        p.FilterType = Enum.RaycastFilterType.Exclude; p.IgnoreWater = true
        local fl = { Camera }
        if LocalPlayer.Character then table.insert(fl, LocalPlayer.Character) end
        p.FilterDescendantsInstances = fl
        local r = Workspace:Raycast(o, d, p)
        if r == nil then return true end
        return r.Instance and r.Instance:IsDescendantOf(part.Parent)
    end

    local function findByScreen(partName, fov, wc)
        local best, bestD = nil, fov
        local mp = UserInputService:GetMouseLocation()
        for _, plr in ipairs(Players:GetPlayers()) do
            if isEnemy(plr) and isAlive(plr.Character) then
                local part = plr.Character:FindFirstChild(partName) or plr.Character:FindFirstChild("Head") or plr.Character:FindFirstChild("HumanoidRootPart")
                if part then
                    if wc and not isVisible(part) then continue end
                    local sp, on = Camera:WorldToViewportPoint(part.Position)
                    if on then
                        local d = (Vector2.new(sp.X, sp.Y) - mp).Magnitude
                        if d < bestD then bestD = d; best = part end
                    end
                end
            end
        end
        return best, bestD
    end

    local function findByDistance(wc)
        local best, bestD = nil, math.huge
        for _, plr in ipairs(Players:GetPlayers()) do
            if isEnemy(plr) and isAlive(plr.Character) then
                local head = plr.Character:FindFirstChild("Head")
                if head then
                    if wc and not isVisible(head) then continue end
                    local d = (Camera.CFrame.Position - head.Position).Magnitude
                    if d < bestD then bestD = d; best = head end
                end
            end
        end
        return best
    end

    local SilentTarget = nil
    local function setupSilentHook()
        if SilentHooked then return end
        pcall(function()
            if hookmetamethod then
                OldIndex = hookmetamethod(game, "__index", newcclosure(function(self, key)
                    if C.SilentAim and SilentTarget and SilentTarget.Parent and self == Mouse then
                        if key == "Hit" then return CFrame.new(SilentTarget.Position)
                        elseif key == "Target" then return SilentTarget
                        elseif key == "X" then return (Camera:WorldToViewportPoint(SilentTarget.Position)).X
                        elseif key == "Y" then return (Camera:WorldToViewportPoint(SilentTarget.Position)).Y
                        elseif key == "UnitRay" then
                            local o = Camera.CFrame.Position
                            return Ray.new(o, (SilentTarget.Position - o).Unit)
                        end
                    end
                    return OldIndex(self, key)
                end))
                SilentHooked = true
            elseif getrawmetatable then
                local mt = getrawmetatable(game)
                if setreadonly then setreadonly(mt, false) end
                OldIndex = mt.__index
                mt.__index = newcclosure(function(self, key)
                    if C.SilentAim and SilentTarget and SilentTarget.Parent and self == Mouse then
                        if key == "Hit" then return CFrame.new(SilentTarget.Position)
                        elseif key == "Target" then return SilentTarget
                        elseif key == "X" then return (Camera:WorldToViewportPoint(SilentTarget.Position)).X
                        elseif key == "Y" then return (Camera:WorldToViewportPoint(SilentTarget.Position)).Y
                        elseif key == "UnitRay" then
                            local o = Camera.CFrame.Position
                            return Ray.new(o, (SilentTarget.Position - o).Unit)
                        end
                    end
                    return OldIndex(self, key)
                end)
                if setreadonly then setreadonly(mt, true) end
                SilentHooked = true
            end
        end)
    end

    track(RunService.RenderStepped:Connect(function(dt)
        Camera = Workspace.CurrentCamera
        local mp = UserInputService:GetMouseLocation()

        AssistFOV.Position  = mp; AssistFOV.Radius = C.AA_FOV; AssistFOV.Visible = C.AA_ShowFOV and C.AimAssist
        SilentFOV.Position  = mp; SilentFOV.Radius = C.SA_FOV; SilentFOV.Visible = C.SA_ShowFOV and C.SilentAim
        AutoLookFOV.Position = mp; AutoLookFOV.Radius = 400; AutoLookFOV.Visible = C.AL_ShowFOV and C.AutoLook

        if C.AimAssist then
            local tgt, dist = findByScreen("Head", C.AA_FOV, C.AA_WallCheck)
            if tgt and tgt.Parent then
                local prox = 1 - (dist / C.AA_FOV)
                local alpha = C.AA_Strength * prox * prox
                alpha = 1 - math.pow(1 - alpha, dt * 60)
                Camera.CFrame = Camera.CFrame:Lerp(CFrame.new(Camera.CFrame.Position, tgt.Position), math.clamp(alpha, 0, 0.3))
            end
        end

        if C.SilentAim then
            local wc = C.SA_WallCheck
            if C.SA_HitChance >= 100 or math.random(100) <= C.SA_HitChance then
                SilentTarget = findByScreen(C.SA_Part, C.SA_FOV, wc)
            else
                SilentTarget = nil
            end
        else
            SilentTarget = nil
        end

        if C.AutoLook then
            local tgt = findByDistance(C.AL_WallCheck)
            if tgt and tgt.Parent then
                local alpha = 1 - math.pow(1 - C.AL_Smooth, dt * 60)
                Camera.CFrame = Camera.CFrame:Lerp(CFrame.new(Camera.CFrame.Position, tgt.Position), math.clamp(alpha, 0.01, 1))
            end
        end
    end))

    local function applyGunMods()
        local wf = ReplicatedStorage:FindFirstChild("Weapons")
        if not wf then return end
        for _, gun in ipairs(wf:GetChildren()) do
            pcall(function()
                if C.NoRecoil and gun:FindFirstChild("RecoilControl") then gun.RecoilControl.Value = 0 end
                if C.NoSpread then
                    if gun:FindFirstChild("Spread") then gun.Spread.Value = 0 end
                    if gun:FindFirstChild("MaxSpread") then gun.MaxSpread.Value = 0 end
                    if gun:FindFirstChild("SpreadRecovery") then gun.SpreadRecovery.Value = 0 end
                end
                if C.FullAuto and gun:FindFirstChild("Auto") then gun.Auto.Value = true end
                if C.RapidFire and gun:FindFirstChild("FireRate") then gun.FireRate.Value = 0.03 end
                if C.FastReload then
                    if gun:FindFirstChild("ReloadTime") then gun.ReloadTime.Value = 0.05 end
                    if gun:FindFirstChild("EquipTime") then gun.EquipTime.Value = 0.05 end
                end
            end)
        end
    end
    local gmt = 0
    track(RunService.Heartbeat:Connect(function(dt)
        gmt = gmt + dt
        if gmt >= 0.5 then gmt = 0; applyGunMods() end
    end))

    local function createESP_Arsenal(plr)
        if plr == LocalPlayer then return end
        local function setup(char)
            if not char then return end
            cleanupESP(plr)
            local d = { connections = {}, highlight = nil, billboard = nil }
            ESPData[plr.UserId] = d
            if not char:WaitForChild("HumanoidRootPart", 5) then return end

            local hl = Instance.new("Highlight")
            hl.Name = "Chams"; hl.Adornee = char
            hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
            hl.FillColor = getTeamColor(plr); hl.OutlineColor = Color3.new(1, 1, 1)
            hl.FillTransparency = 0.4; hl.OutlineTransparency = 0.1
            hl.Enabled = C.ESP_Chams and isEnemy(plr); hl.Parent = char; d.highlight = hl

            local head = char:WaitForChild("Head", 5)
            if head then
                local bb = Instance.new("BillboardGui")
                bb.Name = "Tag"; bb.Adornee = head; bb.Size = UDim2.fromOffset(120, 40)
                bb.StudsOffset = Vector3.new(0, 2.5, 0); bb.AlwaysOnTop = true
                bb.Enabled = C.ESP_Names and isEnemy(plr)

                local lbl = Instance.new("TextLabel", bb)
                lbl.Size = UDim2.fromScale(1, 1); lbl.BackgroundTransparency = 1
                lbl.TextColor3 = getTeamColor(plr); lbl.TextStrokeTransparency = 0
                lbl.TextStrokeColor3 = Color3.new(0, 0, 0); lbl.Font = Enum.Font.GothamBold
                lbl.TextSize = 12; lbl.Text = plr.DisplayName or plr.Name
                bb.Parent = head; d.billboard = bb

                local uc = RunService.RenderStepped:Connect(function()
                    if not char.Parent or not head.Parent then bb.Enabled = false; hl.Enabled = false; return end
                    if isEnemy(plr) then
                        local dist = math.floor((Camera.CFrame.Position - head.Position).Magnitude)
                        lbl.Text = string.format("%s [%dm]", plr.DisplayName or plr.Name, dist)
                        bb.Enabled = C.ESP_Names; hl.Enabled = C.ESP_Chams
                        hl.FillColor = getTeamColor(plr); lbl.TextColor3 = getTeamColor(plr)
                    else
                        bb.Enabled = false; hl.Enabled = false
                    end
                end)
                table.insert(d.connections, uc); track(uc)
            end
        end
        if plr.Character then task.spawn(setup, plr.Character) end
        track(plr.CharacterAdded:Connect(function(c) task.spawn(setup, c) end))
    end
    for _, p in ipairs(Players:GetPlayers()) do task.spawn(createESP_Arsenal, p) end
    track(Players.PlayerAdded:Connect(function(p) task.spawn(createESP_Arsenal, p) end))
    track(Players.PlayerRemoving:Connect(function(p) cleanupESP(p) end))

    Tabs.AimAssist:AddSection("Console Magnetism")
    Tabs.AimAssist:AddToggle("aa1", { Title = "Enable", Default = false, Callback = function(v) C.AimAssist = v end })
    Tabs.AimAssist:AddSlider("aa2", { Title = "Strength", Default = 0.12, Min = 0.01, Max = 0.35, Rounding = 2, Callback = function(v) C.AA_Strength = v end })
    Tabs.AimAssist:AddSlider("aa3", { Title = "FOV (px)", Default = 120, Min = 30, Max = 400, Rounding = 0, Callback = function(v) C.AA_FOV = v end })
    Tabs.AimAssist:AddToggle("aa4", { Title = "Show FOV", Default = true, Callback = function(v) C.AA_ShowFOV = v end })
    Tabs.AimAssist:AddToggle("aa5", { Title = "Wall Check", Default = true, Callback = function(v) C.AA_WallCheck = v end })

    Tabs.SilentAim:AddSection("Silent Aim")
    Tabs.SilentAim:AddToggle("sa1", { Title = "Enable", Default = false, Callback = function(v) C.SilentAim = v; if v then setupSilentHook() end end })
    Tabs.SilentAim:AddDropdown("sa2", { Title = "Target Part", Values = { "Head", "HumanoidRootPart" }, Default = "Head", Callback = function(v) C.SA_Part = v end })
    Tabs.SilentAim:AddSlider("sa3", { Title = "Hit Chance (%)", Default = 100, Min = 10, Max = 100, Rounding = 0, Callback = function(v) C.SA_HitChance = v end })
    Tabs.SilentAim:AddSlider("sa4", { Title = "FOV (px)", Default = 180, Min = 30, Max = 500, Rounding = 0, Callback = function(v) C.SA_FOV = v end })
    Tabs.SilentAim:AddToggle("sa5", { Title = "Show FOV", Default = false, Callback = function(v) C.SA_ShowFOV = v end })
    Tabs.SilentAim:AddToggle("sa6", { Title = "Wall Check", Default = true, Callback = function(v) C.SA_WallCheck = v end })

    Tabs.AutoLook:AddSection("Automatic Lock")
    Tabs.AutoLook:AddToggle("al1", { Title = "Enable", Default = false, Callback = function(v) C.AutoLook = v end })
    Tabs.AutoLook:AddSlider("al2", { Title = "Speed", Default = 0.5, Min = 0.05, Max = 1, Rounding = 2, Callback = function(v) C.AL_Smooth = v end })
    Tabs.AutoLook:AddToggle("al3", { Title = "Show FOV", Default = false, Callback = function(v) C.AL_ShowFOV = v end })
    Tabs.AutoLook:AddToggle("al4", { Title = "Wall Check", Default = true, Callback = function(v) C.AL_WallCheck = v end })

    Tabs.Guns:AddSection("Gun Modifications")
    Tabs.Guns:AddToggle("g1", { Title = "No Recoil", Default = true, Callback = function(v) C.NoRecoil = v; applyGunMods() end })
    Tabs.Guns:AddToggle("g2", { Title = "No Spread", Default = true, Callback = function(v) C.NoSpread = v; applyGunMods() end })
    Tabs.Guns:AddToggle("g3", { Title = "Full Auto", Default = true, Callback = function(v) C.FullAuto = v; applyGunMods() end })
    Tabs.Guns:AddToggle("g4", { Title = "Rapid Fire", Default = false, Callback = function(v) C.RapidFire = v; applyGunMods() end })
    Tabs.Guns:AddToggle("g5", { Title = "Fast Reload", Default = false, Callback = function(v) C.FastReload = v; applyGunMods() end })

    Tabs.Visuals:AddSection("ESP Vision")
    Tabs.Visuals:AddToggle("e1", { Title = "Chams", Default = true, Callback = function(v) C.ESP_Chams = v end })
    Tabs.Visuals:AddToggle("e2", { Title = "Names + Distance", Default = true, Callback = function(v) C.ESP_Names = v end })

    Tabs.Misc:AddSection("Speed & Jump")
    Tabs.Misc:AddToggle("m1", { Title = "Speed", Default = false, Callback = function(v)
        C.SpeedEnabled = v
        if not v then local c = LocalPlayer.Character; if c then local h = c:FindFirstChild("Humanoid"); if h then h.WalkSpeed = 16 end end end
    end })
    Tabs.Misc:AddSlider("m2", { Title = "WalkSpeed", Default = 26, Min = 16, Max = 60, Rounding = 0, Callback = function(v) C.SpeedValue = v end })
    Tabs.Misc:AddToggle("m3", { Title = "Infinite Jump", Default = false, Callback = function(v) C.InfiniteJump = v end })

-- ===========================================================
-- MODULE 2: TEEN TITANS BATTLEGROUNDS
-- ===========================================================
elseif CurrentGame == "Teen Titans" then
    Tabs.Aimbot  = Window:AddTab({ Title = "Aimbot",          Icon = "crosshair" })
    Tabs.Hitbox  = Window:AddTab({ Title = "Hitbox Expander",  Icon = "box" })
    Tabs.Visuals = Window:AddTab({ Title = "ESP Vision",       Icon = "eye" })
    Tabs.Misc    = Window:AddTab({ Title = "Movement",        Icon = "activity" })

    local C = {
        AimbotEnabled = false, AimbotSmooth = 0.4, AimbotRange = 80, AimbotTarget = "Torso",
        AimbotAutoFace = true, AimbotCamLock = true,
        HitboxEnabled = false, HitboxTorso = 8, HitboxHead = 5, HitboxLimbs = 5, HitboxTrans = 0.7, HitboxShowVisual = false,
        ESP_Chams = false, ESP_Names = false,
        SpeedEnabled = false, SpeedValue = 26, InfiniteJump = false, NoClip = false,
    }

    local function isInCombatArea(char)
        if not char or not char.Parent then return false end
        local spawn = Workspace:FindFirstChild("Spawn")
        if spawn and char:IsDescendantOf(spawn) then return false end
        return true
    end

    local function findClosestEnemy()
        local myChar = LocalPlayer.Character; if not myChar or not isAlive(myChar) then return nil, math.huge end
        local myRoot = myChar:FindFirstChild("HumanoidRootPart"); if not myRoot then return nil, math.huge end
        local best, bestDist = nil, C.AimbotRange; local myPos = myRoot.Position
        for _, plr in ipairs(Players:GetPlayers()) do
            if plr ~= LocalPlayer and isAlive(plr.Character) and isInCombatArea(plr.Character) then
                local part = plr.Character:FindFirstChild(C.AimbotTarget) or plr.Character:FindFirstChild("Torso") or plr.Character:FindFirstChild("HumanoidRootPart")
                if part then
                    local d = (myPos - part.Position).Magnitude
                    if d < bestDist then bestDist = d; best = part end
                end
            end
        end
        return best, bestDist
    end

    local TargetIndicator = Drawing.new("Circle")
    TargetIndicator.Thickness = 2; TargetIndicator.NumSides = 32; TargetIndicator.Radius = 20
    TargetIndicator.Filled = false; TargetIndicator.Color = Color3.fromRGB(255, 50, 50); TargetIndicator.Visible = false
    table.insert(FOVCircles, TargetIndicator)

    track(RunService.RenderStepped:Connect(function(dt)
        Camera = Workspace.CurrentCamera
        local myChar = LocalPlayer.Character; local myRoot = myChar and myChar:FindFirstChild("HumanoidRootPart")

        if C.AimbotEnabled and myRoot and isAlive(myChar) then
            local target, dist = findClosestEnemy()
            if target and target.Parent then
                local sp, on = Camera:WorldToViewportPoint(target.Position)
                TargetIndicator.Visible = on
                if on then
                    TargetIndicator.Position = Vector2.new(sp.X, sp.Y)
                    local ratio = math.clamp(dist / C.AimbotRange, 0, 1)
                    TargetIndicator.Color = Color3.fromRGB(math.floor(255 * ratio), math.floor(255 * (1 - ratio)), 50)
                end
                local alpha = 1 - math.pow(1 - C.AimbotSmooth, dt * 60); alpha = math.clamp(alpha, 0.01, 1)
                if C.AimbotAutoFace then
                    local lookAt = Vector3.new(target.Position.X, myRoot.Position.Y, target.Position.Z)
                    myRoot.CFrame = myRoot.CFrame:Lerp(CFrame.new(myRoot.Position, lookAt), alpha)
                end
                if C.AimbotCamLock then
                    Camera.CFrame = Camera.CFrame:Lerp(CFrame.new(Camera.CFrame.Position, target.Position), alpha)
                end
            else TargetIndicator.Visible = false end
        else TargetIndicator.Visible = false end

        if C.HitboxEnabled then
            for _, plr in ipairs(Players:GetPlayers()) do
                if plr ~= LocalPlayer and isAlive(plr.Character) and isInCombatArea(plr.Character) then
                    local char = plr.Character
                    local torso = char:FindFirstChild("Torso")
                    if torso then
                        local sz = C.HitboxTorso; local w = Vector3.new(sz, sz, sz)
                        if (torso.Size - w).Magnitude > 0.1 then torso.Size = w; torso.CanCollide = false; torso.Transparency = C.HitboxShowVisual and C.HitboxTrans or 0 end
                    end
                    local head = char:FindFirstChild("Head")
                    if head then
                        local sz = C.HitboxHead; local w = Vector3.new(sz, sz, sz)
                        if (head.Size - w).Magnitude > 0.1 then head.Size = w; head.CanCollide = false; head.Transparency = C.HitboxShowVisual and C.HitboxTrans or 0 end
                    end
                    local hrp = char:FindFirstChild("HumanoidRootPart")
                    if hrp then
                        local sz = C.HitboxTorso; local w = Vector3.new(sz, sz, sz)
                        if (hrp.Size - w).Magnitude > 0.1 then hrp.Size = w; hrp.CanCollide = false; hrp.Transparency = 1 end
                    end
                    for _, ln in ipairs({ "Left Arm", "Right Arm", "Left Leg", "Right Leg" }) do
                        local l = char:FindFirstChild(ln)
                        if l then
                            local sz = C.HitboxLimbs; local w = Vector3.new(sz, sz, sz)
                            if (l.Size - w).Magnitude > 0.1 then l.Size = w; l.CanCollide = false; l.Transparency = C.HitboxShowVisual and C.HitboxTrans or 0 end
                        end
                    end
                end
            end
        end

        if C.NoClip and myChar then
            for _, part in ipairs(myChar:GetDescendants()) do if part:IsA("BasePart") then part.CanCollide = false end end
        end
    end))

    local function createESP_TTitans(plr)
        if plr == LocalPlayer then return end
        local function setup(char)
            if not char then return end; cleanupESP(plr)
            if not isInCombatArea(char) then return end
            local d = { connections = {}, highlight = nil, billboard = nil }; ESPData[plr.UserId] = d
            if not char:WaitForChild("HumanoidRootPart", 5) then return end

            local hl = Instance.new("Highlight"); hl.Name = "Chams"; hl.Adornee = char
            hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop; hl.FillColor = Color3.fromRGB(255, 60, 60)
            hl.OutlineColor = Color3.new(1, 1, 1); hl.FillTransparency = 0.4; hl.OutlineTransparency = 0.1
            hl.Enabled = C.ESP_Chams; hl.Parent = char; d.highlight = hl

            local head = char:WaitForChild("Head", 5)
            if head then
                local bb = Instance.new("BillboardGui"); bb.Name = "Tag"; bb.Adornee = head
                bb.Size = UDim2.fromOffset(140, 40); bb.StudsOffset = Vector3.new(0, 2.5, 0); bb.AlwaysOnTop = true; bb.Enabled = C.ESP_Names
                local lbl = Instance.new("TextLabel", bb); lbl.Size = UDim2.fromScale(1, 1); lbl.BackgroundTransparency = 1
                lbl.TextColor3 = Color3.fromRGB(255, 255, 100); lbl.TextStrokeTransparency = 0; lbl.TextStrokeColor3 = Color3.new(0, 0, 0)
                lbl.Font = Enum.Font.GothamBold; lbl.TextSize = 12; bb.Parent = head; d.billboard = bb

                local uc = RunService.RenderStepped:Connect(function()
                    if not char.Parent or not head.Parent then bb.Enabled = false; hl.Enabled = false; return end
                    if isInCombatArea(char) then
                        local hum = char:FindFirstChild("Humanoid"); local hp = hum and math.floor(hum.Health) or "?"
                        local dist = math.floor((Camera.CFrame.Position - head.Position).Magnitude)
                        lbl.Text = string.format("%s [%dHP] [%dm]", plr.DisplayName or plr.Name, hp, dist)
                        if hum then
                            local r = math.clamp(hum.Health / hum.MaxHealth, 0, 1)
                            lbl.TextColor3 = Color3.fromRGB(math.floor(255 * (1 - r)), math.floor(255 * r), 50)
                            hl.FillColor = lbl.TextColor3
                        end
                        bb.Enabled = C.ESP_Names; hl.Enabled = C.ESP_Chams
                    else
                        bb.Enabled = false; hl.Enabled = false
                    end
                end); table.insert(d.connections, uc); track(uc)
            end
        end
        if plr.Character then task.spawn(setup, plr.Character) end
        track(plr.CharacterAdded:Connect(function(c) task.spawn(setup, c) end))
    end
    for _, p in ipairs(Players:GetPlayers()) do task.spawn(createESP_TTitans, p) end
    track(Players.PlayerAdded:Connect(function(p) task.spawn(createESP_TTitans, p) end))
    track(Players.PlayerRemoving:Connect(function(p) cleanupESP(p) end))

    Tabs.Aimbot:AddSection("Auto Face + Camera Lock")
    Tabs.Aimbot:AddToggle("aim1", { Title = "Enable Aimbot", Default = false, Callback = function(v) C.AimbotEnabled = v end })
    Tabs.Aimbot:AddToggle("aim2", { Title = "Auto Face Character", Default = true, Callback = function(v) C.AimbotAutoFace = v end })
    Tabs.Aimbot:AddToggle("aim3", { Title = "Camera Lock", Default = true, Callback = function(v) C.AimbotCamLock = v end })
    Tabs.Aimbot:AddSlider("aim4", { Title = "Lock Speed", Default = 0.4, Min = 0.05, Max = 1, Rounding = 2, Callback = function(v) C.AimbotSmooth = v end })
    Tabs.Aimbot:AddSlider("aim5", { Title = "Range (studs)", Default = 80, Min = 15, Max = 200, Rounding = 0, Callback = function(v) C.AimbotRange = v end })
    Tabs.Aimbot:AddDropdown("aim6", { Title = "Target Part", Values = { "Torso", "Head", "HumanoidRootPart" }, Default = "Torso", Callback = function(v) C.AimbotTarget = v end })

    Tabs.Hitbox:AddSection("Hitbox Expander R6")
    Tabs.Hitbox:AddToggle("hb1", { Title = "Enable", Default = false, Callback = function(v)
        C.HitboxEnabled = v
        if not v then
            for _, p in ipairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character then
                    local ch = p.Character
                    local t = ch:FindFirstChild("Torso"); if t then t.Size = Vector3.new(2, 2, 1); t.Transparency = 0 end
                    local h = ch:FindFirstChild("Head"); if h then h.Size = Vector3.new(2, 1, 1); h.Transparency = 0 end
                    local r = ch:FindFirstChild("HumanoidRootPart"); if r then r.Size = Vector3.new(2, 2, 1); r.Transparency = 1 end
                    for _, ln in ipairs({ "Left Arm", "Right Arm", "Left Leg", "Right Leg" }) do
                        local l = ch:FindFirstChild(ln); if l then l.Size = Vector3.new(1, 2, 1); l.Transparency = 0 end
                    end
                end
            end
        end
    end })
    Tabs.Hitbox:AddSlider("hb2", { Title = "Torso", Default = 8, Min = 3, Max = 20, Rounding = 0, Callback = function(v) C.HitboxTorso = v end })
    Tabs.Hitbox:AddSlider("hb3", { Title = "Head", Default = 5, Min = 2, Max = 15, Rounding = 0, Callback = function(v) C.HitboxHead = v end })
    Tabs.Hitbox:AddSlider("hb4", { Title = "Limbs", Default = 5, Min = 2, Max = 15, Rounding = 0, Callback = function(v) C.HitboxLimbs = v end })
    Tabs.Hitbox:AddToggle("hb5", { Title = "Show Visual Hitbox", Default = false, Callback = function(v) C.HitboxShowVisual = v end })

    Tabs.Visuals:AddSection("ESP Vision")
    Tabs.Visuals:AddToggle("e1", { Title = "Chams", Default = false, Callback = function(v) C.ESP_Chams = v end })
    Tabs.Visuals:AddToggle("e2", { Title = "Names + HP + Distance", Default = false, Callback = function(v) C.ESP_Names = v end })

    Tabs.Misc:AddSection("Movement")
    Tabs.Misc:AddToggle("m1", { Title = "Speed", Default = false, Callback = function(v)
        C.SpeedEnabled = v
        if not v then local c = LocalPlayer.Character; if c then local h = c:FindFirstChild("Humanoid"); if h then h.WalkSpeed = 16 end end end
    end })
    Tabs.Misc:AddSlider("m2", { Title = "WalkSpeed", Default = 26, Min = 16, Max = 80, Rounding = 0, Callback = function(v) C.SpeedValue = v end })
    Tabs.Misc:AddToggle("m3", { Title = "Infinite Jump", Default = false, Callback = function(v) C.InfiniteJump = v end })
    Tabs.Misc:AddToggle("m4", { Title = "NoClip", Default = false, Callback = function(v) C.NoClip = v end })

-- ===========================================================
-- MODULE 3: FRIDAY NIGHT PARTYING (FNP)
-- ===========================================================
elseif CurrentGame == "Friday Night Partying" then
    Tabs.FNP    = Window:AddTab({ Title = "FNP AutoPlayer", Icon = "gamepad-2" })
    Tabs.FNPBad = Window:AddTab({ Title = "FNP Bad Notes",  Icon = "shield-alert" })

    local Conductor = require(ReplicatedStorage:WaitForChild("Modules"):WaitForChild("Conductor"))

    local FNP_Config = {
        AutoPlayerEnabled = true,
        StrictHitWindow   = 6,
        SelectedSide      = "Auto",
        AntiBadNotes      = true,
        LegitMode         = false,
        HarmfulKeywords   = {
            "kill", "instahurt", "knife", "bknife", "axe", "extricky", "trickyex", "mystery", "saw", "mine", "double killl", "sacrifice",
            "poison", "ebola", "bone", "blacknotebob", "karma", "trap", "drain", "hurtnote", "hurt", "fire", "flame", "spikes", "shock", "electric",
            "finnglitch", "glitch", "missingno", "sonic.exe", "lord x", "betrayal", "pibby", "pibbyno", "vivisection", "strangled", "taintedfate", "ice", "phantom"
        }
    }

    local botplayFlag = nil
    local activeNotesList = nil

    local function hookEnginePointers()
        for _, fn in pairs(getgc(false)) do
            if type(fn) == "function" and islclosure(fn) then
                local upvalues = getupvalues(fn)
                local foundFlag = false
                for _, up in pairs(upvalues) do
                    if type(up) == "table" and rawget(up, "Value") ~= nil and type(up.Value) == "boolean" then
                        botplayFlag = up
                        foundFlag = true
                        break
                    end
                end
                if foundFlag then
                    for _, up in pairs(upvalues) do
                        if type(up) == "table" and up ~= botplayFlag then
                            activeNotesList = up
                            return
                        end
                    end
                end
            end
        end
    end
    hookEnginePointers()

    local function isHarmful(obj)
        if not FNP_Config.AntiBadNotes or not obj then return false end
        local typeStr = tostring(obj.Type or obj.dType or obj.Name or ""):lower()
        if obj.Animation and obj.Animation.Name then typeStr = typeStr .. " " .. tostring(obj.Animation.Name):lower() end
        for _, kw in ipairs(FNP_Config.HarmfulKeywords) do
            if typeStr:find(kw) then return true end
        end
        return false
    end

    track(RunService.RenderStepped:Connect(function()
        local songPos = Conductor.SongPos or 0
        if songPos <= 0 then return end
        if not botplayFlag then hookEnginePointers() end
        if botplayFlag then botplayFlag.Value = FNP_Config.AutoPlayerEnabled end
        if not FNP_Config.AutoPlayerEnabled then return end

        if activeNotesList then
            for i = 1, #activeNotesList do
                local obj = activeNotesList[i]
                if type(obj) == "table" and not obj.GoodHit and not obj.Destroyed then
                    local isPlayer = (FNP_Config.SelectedSide == "Auto" and obj.MustPress == true)
                                  or (FNP_Config.SelectedSide == "Both")
                                  or (FNP_Config.SelectedSide == "BF" and obj.MustPress == true)
                                  or (FNP_Config.SelectedSide == "Dad" and obj.MustPress == false)
                    if isPlayer then
                        if isHarmful(obj) then
                            obj.shouldPress = false
                            obj.CanBeHit = false
                        else
                            local strum = obj.StrumTime
                            if strum then
                                if obj.IsSustain then
                                    obj.shouldPress = true
                                else
                                    local diff = strum - songPos
                                    local maxW = (FNP_Config.LegitMode and math.random(5, 25)) or FNP_Config.StrictHitWindow
                                    obj.shouldPress = (diff <= maxW)
                                end
                            end
                        end
                    else
                        obj.shouldPress = false
                    end
                end
            end
        end
    end))

    Tabs.FNP:AddSection("Main Controls")
    Tabs.FNP:AddToggle("FNP_AP", {
        Title = "Enable AutoPlayer",
        Default = true,
        Callback = function(v) FNP_Config.AutoPlayerEnabled = v; if botplayFlag then botplayFlag.Value = v end end
    })
    Tabs.FNP:AddDropdown("FNP_Side", {
        Title = "Player Side",
        Values = { "Auto (Recommended)", "Boyfriend (Right)", "Opponent / Dad (Left)", "Both Sides (Dual Play)" },
        Default = "Auto (Recommended)",
        Callback = function(v)
            if v:find("Auto") then FNP_Config.SelectedSide = "Auto"
            elseif v:find("Boyfriend") then FNP_Config.SelectedSide = "BF"
            elseif v:find("Opponent") then FNP_Config.SelectedSide = "Dad"
            elseif v:find("Both") then FNP_Config.SelectedSide = "Both" end
        end
    })
    Tabs.FNP:AddSlider("FNP_Timing", {
        Title = "Strict Hit Window (ms)",
        Default = 6, Min = 2, Max = 30, Rounding = 0,
        Callback = function(v) FNP_Config.StrictHitWindow = v end
    })
    Tabs.FNPBad:AddSection("Bad Notes Filter")
    Tabs.FNPBad:AddToggle("FNP_BadNotes", {
        Title = "Enable Anti-Bad Notes (40+ Types)",
        Default = true,
        Callback = function(v) FNP_Config.AntiBadNotes = v end
    })

-- ===========================================================
-- MODULE 4: FUNKY FRIDAY
-- ===========================================================
elseif CurrentGame == "Funky Friday" then
    Tabs.FF = Window:AddTab({ Title = "Funky Friday", Icon = "gamepad-2" })

    local FF_Config = {
        AutoPlay     = true,
        HitOffset    = 0,
        HoldDuration = 0.05,
        AntiBadNotes = true,
    }

    local inputs = {}
    local tempConns = {}
    local vsrgConnection, vsrgConn2
    local scr = Enum.InputBindingType.Scriptable

    local function onInput(v, overwrite)
        local input
        local laneNum = tonumber(v.Name:sub(5))
        if not laneNum then return end
        local start = tick()

        while not input and vsrgConnection do
            for _, val in ipairs(v:GetChildren()) do
                if val.Name:match("Gamepad") or (tick() - start > 2 and val.Name:match("Keyboard")) or tick() - start > 4 then
                    input = val
                    break
                end
            end
            if input then break end
            task.wait()
        end

        if not input then return end
        input.Type = scr

        if not overwrite then
            table.insert(tempConns, input:GetPropertyChangedSignal("Type"):Connect(function()
                input.Type = scr
            end))
            table.insert(tempConns, v.ChildAdded:Connect(function()
                onInput(v, true)
            end))
        end

        inputs[laneNum] = input
    end

    local function hookInputs(vsrgContext)
        inputs = {}
        vsrgConnection = vsrgContext:GetPropertyChangedSignal("Parent"):Connect(function()
            if not vsrgContext.Parent then
                for _, v in ipairs(tempConns) do
                    if v and v.Connected then v:Disconnect() end
                end
                inputs = {}
                if vsrgConnection then vsrgConnection:Disconnect() end
                if vsrgConn2 then vsrgConn2:Disconnect() end
                vsrgConnection = nil
            end
        end)

        for _, v in ipairs(vsrgContext:GetChildren()) do
            task.spawn(onInput, v)
        end
        vsrgConn2 = vsrgContext.ChildAdded:Connect(onInput)
    end

    local function fireLane(laneIndex, state)
        local inp = inputs[laneIndex]
        if not inp then return end
        local s, _ = pcall(inp.Fire, inp, state)
        if not s then
            inp.Type = scr
            pcall(inp.Fire, inp, state)
        end
    end

    local laneStates = {}
    local laneHitIndexes = {}
    local function hitLane(laneIndex, duration)
        duration = duration or 0

        if laneStates[laneIndex] then
            fireLane(laneIndex, false)
            laneStates[laneIndex] = false
        end

        fireLane(laneIndex, true)
        laneStates[laneIndex] = true

        laneHitIndexes[laneIndex] = (laneHitIndexes[laneIndex] or -1) + 1
        local myIndex = laneHitIndexes[laneIndex]

        if duration > 0 then
            task.wait(duration)
        else
            task.wait(0.02)
        end

        if laneHitIndexes[laneIndex] == myIndex then
            fireLane(laneIndex, false)
            laneStates[laneIndex] = false
        end
    end

    local badNoteAssets = {
        ["rbxassetid://103483801062498"] = true,
        ["rbxassetid://88530467220950"]  = true,
        ["rbxassetid://109130876544260"] = true,
        ["rbxassetid://120222801097284"] = true,
        ["rbxassetid://101951481332606"] = true
    }

    local offsets = { Sick = 0.05, Good = 0.1, Ok = 0.15, Bad = 0.2, Miss = 0.3 }
    local offsetOffset = 0.01
    for i, v in pairs(offsets) do offsets[i] = v - offsetOffset end
    local sickOffset = offsets.Sick
    local badOffset  = offsets.Bad
    local missOffset2 = offsets.Miss * 2

    local function getDistance(a, b) return (a - b).Magnitude end
    local function UDimToVector2(ud) return Vector2.new(0, ud.Y.Scale) end

    local function isDownS(data)
        local topOnes = 0
        local receptor = data.Receptor
        for _, note in ipairs(data.Notes) do
            if note.Note.Position.Y.Scale < receptor.Position.Y.Scale + 0.5 then
                topOnes = topOnes + 1
            end
        end
        return (#data.Notes > 0) and (topOnes / #data.Notes > 0.75) or true
    end

    local function sortLane(data)
        table.sort(data.Notes, function(a, b)
            local cond = a.Note.Position.Y.Scale < b.Note.Position.Y.Scale
            if data.IsDownScroll then cond = not cond end
            return cond
        end)
    end

    local function isBehind(data, x, y)
        local is = x.Y > y.Y
        if data.IsDownScroll then is = not is end
        return is
    end

    local function canHit(noteData, laneData, far)
        local receptorPos = UDimToVector2(laneData.Receptor.Position) + Vector2.new(0, 0.5)
        local notePos = UDimToVector2(noteData.Note.Position)
        local speed = laneData.ScrollSpeed > 0 and laneData.ScrollSpeed or 1
        local dist = getDistance(receptorPos, notePos) / speed
        local behind = isBehind(laneData, receptorPos, notePos)

        dist = dist - (FF_Config.HitOffset / 1000)
        if dist < 0 then
            dist = -dist
            behind = not behind
        end

        if noteData.IsBad then return dist, behind end
        if far then return dist <= missOffset2 end
        if behind then return dist <= badOffset, true, dist, dist <= sickOffset end
        return (dist <= sickOffset or dist <= noteData.HitDistance), false, dist, (dist <= sickOffset)
    end

    local function calcHold(v, laneData)
        local speed = laneData.ScrollSpeed > 0 and laneData.ScrollSpeed or 1
        return math.abs(v.Size.Y.Scale / speed) + 0.075
    end

    local function hitNote(noteData, laneData, dist, sick)
        local Note = noteData.Note
        if noteData.Hit then return end
        noteData.Hit = true

        local holdTime = 0
        for _, v in ipairs(Note:GetChildren()) do
            if v:IsA("GuiObject") and v.Size ~= UDim2.fromScale(1, 1) then
                holdTime = calcHold(v, laneData)
                break
            end
        end

        local finalDuration = (holdTime > 0 and holdTime or FF_Config.HoldDuration)
        task.spawn(function()
            hitLane(laneData.LaneIndex, finalDuration)
        end)
    end

    local function playLane(lane)
        local notes = lane.Notes
        local toRemove = {}
        for i = 1, #notes do
            local noteData = notes[i]
            local hit, far, dist, sick = canHit(noteData, lane)
            if hit then
                table.insert(toRemove, i)
                task.defer(hitNote, noteData, lane, dist, sick)
            else
                break
            end
        end
        for i = #toRemove, 1, -1 do
            table.remove(notes, toRemove[i])
        end
    end

    local function processLane(lane, isMine)
        local noteData = lane.Notes[1]
        if not noteData then return end

        local receptor = lane.Receptor
        local noteStart = UDimToVector2(noteData.Note.Position)
        local receptorStart = UDimToVector2(receptor.Position)
        local start = tick()

        RunService.RenderStepped:Wait()

        local took = tick() - start
        local receptorEnd = UDimToVector2(receptor.Position)
        local receptorTravel = receptorEnd - receptorStart
        local rawSpeed = getDistance(noteStart, UDimToVector2(noteData.Note.Position) - receptorTravel) / took

        if rawSpeed > 0 then lane.ScrollSpeed = rawSpeed end
        if FF_Config.AutoPlay and isMine then playLane(lane) end
    end

    local function onNote(note, laneData, isMine)
        local sprite = note:WaitForChild("LayeredSprite", 3) and note.LayeredSprite:WaitForChild("1", 3)
        local isBad = (sprite and badNoteAssets[sprite.Image] == true)

        local noteData = {
            Note = note,
            HitDistance = offsets.Sick,
            IsBad = isBad,
            Hit = false,
            Lane = laneData,
            LaneIndex = laneData.LaneIndex
        }

        if isBad and FF_Config.AntiBadNotes then return end
        table.insert(laneData.Notes, noteData)

        local isDownScrollSpawn = note.Position.Y.Scale < laneData.Receptor.Position.Y.Scale + 0.5
        if isDownScrollSpawn ~= laneData.IsDownScroll then
            laneData.IsDownScroll = isDownScrollSpawn
            sortLane(laneData)
        end

        note.AncestryChanged:Connect(function(_, parent)
            if not parent then
                for idx, nd in ipairs(laneData.Notes) do
                    if nd.Note == note then
                        table.remove(laneData.Notes, idx)
                        break
                    end
                end
            end
        end)
    end

    local function onLane(lane, fieldData, isMine)
        local laneNum = tonumber(lane.Name:sub(5))
        if not laneNum then return end

        local receptor = lane:WaitForChild("Receptor", 5)
        local notes = lane:WaitForChild("Notes", 5)
        if not receptor or not notes then return end

        local laneData = {
            ScrollSpeed = 1,
            LaneIndex = laneNum,
            Lane = lane,
            Notes = {},
            IsDownScroll = true,
            Receptor = receptor
        }
        fieldData.Lanes[laneNum] = laneData

        for _, v in ipairs(notes:GetChildren()) do
            task.spawn(onNote, v, laneData, isMine)
        end

        if #laneData.Notes > 0 then
            laneData.IsDownScroll = isDownS(laneData)
            sortLane(laneData)
        end

        notes.ChildAdded:Connect(function(v)
            onNote(v, laneData, isMine)
        end)
    end

    local function getClosest(toIterate)
        if not LocalPlayer.Character then return end
        local c, d = nil, math.huge
        for _, v in ipairs(toIterate:GetChildren()) do
            local m = (v:GetPivot().Position - LocalPlayer.Character:GetPivot().Position).Magnitude
            if m < d then c, d = v, m end
        end
        return c, d
    end

    local function getMySide()
        if workspace:FindFirstChild("Map") and workspace.Map:FindFirstChild("Stages") then
            local stage = getClosest(workspace.Map.Stages)
            if stage and stage:FindFirstChild("Nodes") then
                local node = getClosest(stage.Nodes)
                if node then
                    return node.Name:sub(1, node.Name:find("_") - 1)
                end
            end
        end
        return "Right"
    end

    local function onField(field, fieldData, isMine)
        local inner = field:WaitForChild("Inner", 5)
        if not inner then return end
        for _, v in ipairs(inner:GetChildren()) do
            task.spawn(onLane, v, fieldData, isMine)
        end
    end

    local function onWindow(window)
        local gameFrame = window:WaitForChild("Game", 5)
        if not gameFrame then return end
        local fields = gameFrame:WaitForChild("Fields", 5)
        if not fields then return end

        local sharedData = {
            Fields = {
                Left = { Lanes = {} },
                Right = { Lanes = {} }
            }
        }

        local side = getMySide()
        for sideName, fData in pairs(sharedData.Fields) do
            local fObj = fields:WaitForChild(sideName, 5)
            if fObj then
                task.spawn(onField, fObj, fData, sideName == side)
            end
        end

        track(RunService.RenderStepped:Connect(function()
            if not window.Parent then return end
            side = getMySide() or side
            local myField = sharedData.Fields[side]
            if myField then
                for _, lane in pairs(myField.Lanes) do
                    task.spawn(processLane, lane, true)
                end
            end
        end))
    end

    track(PlayerGui.ChildAdded:Connect(function(ch)
        if ch.ClassName == "InputContext" then
            hookInputs(ch)
        elseif ch.Name == "Window" then
            task.spawn(onWindow, ch)
        end
    end))

    for _, ch in ipairs(PlayerGui:GetChildren()) do
        if ch.ClassName == "InputContext" then
            hookInputs(ch)
        elseif ch.Name == "Window" then
            task.spawn(onWindow, ch)
        end
    end

    Tabs.FF:AddSection("Main Controls")
    Tabs.FF:AddToggle("FF_AP", {
        Title = "Enable AutoPlayer (Kawi Core)",
        Default = true,
        Description = "Calculates scroll speed and hits 100% Sick",
        Callback = function(v)
            FF_Config.AutoPlay = v
            Fluent:Notify({
                Title = "Funky Friday",
                Content = v and "AutoPlayer ENABLED" or "AutoPlayer DISABLED",
                Duration = 2
            })
        end
    })
    Tabs.FF:AddToggle("FF_AntiBad", {
        Title = "Anti-Bad Notes",
        Default = true,
        Callback = function(v) FF_Config.AntiBadNotes = v end
    })
    Tabs.FF:AddSlider("FF_Offset", {
        Title = "Hit Offset (ms)",
        Default = 0, Min = -50, Max = 50, Rounding = 0,
        Callback = function(v) FF_Config.HitOffset = v end
    })
    Tabs.FF:AddSlider("FF_Hold", {
        Title = "Minimum Key Hold Duration (s)",
        Default = 0.05, Min = 0.01, Max = 0.15, Rounding = 2,
        Callback = function(v) FF_Config.HoldDuration = v end
    })
end

-- ===========================================================
-- GLOBAL SETTINGS TAB
-- ===========================================================
Tabs.Settings:AddSection("Appearance & Keybinds")

Tabs.Settings:AddDropdown("ThemeDropdown", {
    Title = "Interface Theme",
    Values = { "Amethyst", "Blood Red", "Dark", "Darker", "Ocean", "Rose", "Sapphire", "Midnight" },
    Default = DefaultTheme,
    Callback = function(Value) Fluent:SetTheme(Value) end
})

Tabs.Settings:AddKeybind("MinimizeKeybind", {
    Title = "Toggle Keybind",
    Mode = "Toggle",
    Default = "RightControl"
})

Tabs.Settings:AddButton({
    Title = "Unload Script",
    Callback = function()
        if _G.Universal_Hub_Cleanup then _G.Universal_Hub_Cleanup() end
    end
})

-- ----------------------------------------------------------
-- 6. GLOBAL CLEANUP
-- ----------------------------------------------------------
_G.Universal_Hub_Cleanup = function()
    for _, conn in ipairs(Connections) do
        if conn and typeof(conn) == "RBXScriptConnection" and conn.Connected then
            conn:Disconnect()
        end
    end
    table.clear(Connections)

    if SilentHooked and OldIndex then
        pcall(function()
            if hookmetamethod then hookmetamethod(game, "__index", OldIndex)
            elseif getrawmetatable then
                local mt = getrawmetatable(game)
                if setreadonly then setreadonly(mt, false) end
                mt.__index = OldIndex
                if setreadonly then setreadonly(mt, true) end
            end
        end)
    end

    for _, c in ipairs(FOVCircles) do
        pcall(function() c:Remove() end)
    end
    table.clear(FOVCircles)

    if ESPData then
        for _, d in pairs(ESPData) do
            for _, c in ipairs(d.connections) do
                if c and c.Connected then c:Disconnect() end
            end
        end
    end

    Window:Destroy()
    print("Universal Hub v33.0 by isamu successfully unloaded.")
end

Window:SelectTab(1)

Fluent:Notify({
    Title = "Universal Multi-Game Hub v33.0",
    Content = string.format("Detected Game: %s\nCreated by isamu\nPress [RightControl] to toggle menu.", CurrentGame),
    Duration = 5
})
