-- =====================================================
-- SUPER OPTIMIZER + ANTI AFK + VISUAL TWEAK
-- Fitur: FPS Boost, Box Player, Hide GUI, Anti AFK
-- =====================================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Lighting = game:GetService("Lighting")
local Workspace = game:GetService("Workspace")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- ===== VARIABLES ANTI AFK =====
local antiAFKActive = false
local antiAFKCoroutine = nil
local lastMoveTime = tick()

-- ===== GUI CONTROL =====
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = PlayerGui
ScreenGui.ResetOnSpawn = false

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 300, 0, 300)
MainFrame.Position = UDim2.new(0.5, -150, 0.5, -150)
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 25)
MainFrame.BackgroundTransparency = 0.1
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, 12)
Corner.Parent = MainFrame

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.Text = "⚡ OPTIMIZER HAZ ⚡"
Title.TextColor3 = Color3.fromRGB(0, 255, 200)
Title.TextScaled = true
Title.Font = Enum.Font.GothamBold
Title.Parent = MainFrame

-- ===== FITUR 1: BLACKSCREEN FPS BOOST =====
local BlackScreenBtn = Instance.new("TextButton")
BlackScreenBtn.Size = UDim2.new(0, 260, 0, 35)
BlackScreenBtn.Position = UDim2.new(0.5, -130, 0.18, 0)
BlackScreenBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
BlackScreenBtn.Text = "🔲 BLACKSCREEN: OFF"
BlackScreenBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
BlackScreenBtn.TextScaled = true
BlackScreenBtn.Font = Enum.Font.Gotham
local BSCorner = Instance.new("UICorner")
BSCorner.CornerRadius = UDim.new(0, 6)
BSCorner.Parent = BlackScreenBtn
BlackScreenBtn.Parent = MainFrame

-- Blackscreen background
local BlackScreen = Instance.new("Frame")
BlackScreen.Size = UDim2.new(1, 0, 1, 0)
BlackScreen.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
BlackScreen.BackgroundTransparency = 1
BlackScreen.BorderSizePixel = 0
BlackScreen.ZIndex = 9999
BlackScreen.Parent = PlayerGui

local blackscreenActive = false

-- ===== FITUR 2: PLAYER KOTAK =====
local BoxBtn = Instance.new("TextButton")
BoxBtn.Size = UDim2.new(0, 260, 0, 35)
BoxBtn.Position = UDim2.new(0.5, -130, 0.35, 0)
BoxBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
BoxBtn.Text = "📦 BOX PLAYER: OFF"
BoxBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
BoxBtn.TextScaled = true
BoxBtn.Font = Enum.Font.Gotham
local BoxCorner = Instance.new("UICorner")
BoxCorner.CornerRadius = UDim.new(0, 6)
BoxCorner.Parent = BoxBtn
BoxBtn.Parent = MainFrame

-- ===== FITUR 3: SEMBUNYIKAN GUI =====
local HideGUIBtn = Instance.new("TextButton")
HideGUIBtn.Size = UDim2.new(0, 260, 0, 35)
HideGUIBtn.Position = UDim2.new(0.5, -130, 0.52, 0)
HideGUIBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
HideGUIBtn.Text = "👁️ HIDE GUI: OFF"
HideGUIBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
HideGUIBtn.TextScaled = true
HideGUIBtn.Font = Enum.Font.Gotham
local HideCorner = Instance.new("UICorner")
HideCorner.CornerRadius = UDim.new(0, 6)
HideCorner.Parent = HideGUIBtn
HideGUIBtn.Parent = MainFrame

-- ===== FITUR 4: ANTI AFK =====
local AntiAFKBtn = Instance.new("TextButton")
AntiAFKBtn.Size = UDim2.new(0, 260, 0, 35)
AntiAFKBtn.Position = UDim2.new(0.5, -130, 0.69, 0)
AntiAFKBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
AntiAFKBtn.Text = "🔄 ANTI AFK: OFF"
AntiAFKBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
AntiAFKBtn.TextScaled = true
AntiAFKBtn.Font = Enum.Font.Gotham
local AFKCorner = Instance.new("UICorner")
AFKCorner.CornerRadius = UDim.new(0, 6)
AFKCorner.Parent = AntiAFKBtn
AntiAFKBtn.Parent = MainFrame

-- Status label
local StatusLabel = Instance.new("TextLabel")
StatusLabel.Size = UDim2.new(1, 0, 0, 25)
StatusLabel.Position = UDim2.new(0, 0, 0.87, 0)
StatusLabel.BackgroundTransparency = 1
StatusLabel.Text = "✅ Siap digunakan"
StatusLabel.TextColor3 = Color3.fromRGB(150, 255, 150)
StatusLabel.TextScaled = true
StatusLabel.Font = Enum.Font.Gotham
StatusLabel.Parent = MainFrame

-- ===== VARIABLES =====
local boxActive = false
local hideGUIActive = false
local originalGUIVisibility = {}
local originalPartProperties = {}

-- ===== ANTI AFK SYSTEM =====
local function antiAFKLoop()
    while antiAFKActive do
        local char = LocalPlayer.Character
        local root = char and char:FindFirstChild("HumanoidRootPart")
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        
        if root and hum then
            -- Gerakan berputar 360 derajat
            local currentPos = root.Position
            local angle = 0
            
            -- Putar 360 derajat dengan langkah kecil
            for i = 1, 36 do  -- 36 langkah = 10 derajat per langkah
                if not antiAFKActive then break end
                
                angle = (i / 36) * math.pi * 2  -- 360 derajat
                local radius = 3  -- radius putaran
                local newX = currentPos.X + math.cos(angle) * radius
                local newZ = currentPos.Z + math.sin(angle) * radius
                
                -- Gerakan ke posisi baru
                hum:MoveTo(Vector3.new(newX, currentPos.Y, newZ))
                root.CFrame = root.CFrame * CFrame.Angles(0, math.rad(10), 0)
                
                StatusLabel.Text = "🔄 Anti AFK: Berputar " .. math.floor((i/36)*100) .. "%"
                wait(0.3)
            end
            
            -- Kembali ke posisi awal
            hum:MoveTo(currentPos)
            StatusLabel.Text = "🔄 Anti AFK: Selesai 1 siklus"
            wait(0.5)
        else
            StatusLabel.Text = "⚠️ Anti AFK: Karakter tidak ditemukan"
            wait(2)
        end
        
        -- Tunggu 60 detik (1 menit) sebelum siklus berikutnya
        for i = 60, 1, -1 do
            if not antiAFKActive then break end
            StatusLabel.Text = "🔄 Anti AFK: Next cycle in " .. i .. "s"
            wait(1)
        end
    end
    StatusLabel.Text = "✅ Anti AFK: OFF"
end

local function toggleAntiAFK()
    antiAFKActive = not antiAFKActive
    
    if antiAFKActive then
        AntiAFKBtn.Text = "🔄 ANTI AFK: ON"
        AntiAFKBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 0)
        StatusLabel.Text = "🔄 Anti AFK: Aktif (cycle 60s)"
        
        -- Jalankan loop anti AFK di coroutine terpisah
        if antiAFKCoroutine then
            coroutine.close(antiAFKCoroutine)
        end
        antiAFKCoroutine = coroutine.create(antiAFKLoop)
        coroutine.resume(antiAFKCoroutine)
    else
        AntiAFKBtn.Text = "🔄 ANTI AFK: OFF"
        AntiAFKBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
        StatusLabel.Text = "✅ Anti AFK: OFF"
        
        -- Hentikan loop
        if antiAFKCoroutine then
            coroutine.close(antiAFKCoroutine)
            antiAFKCoroutine = nil
        end
        
        -- Stop karakter
        local char = LocalPlayer.Character
        if char then
            local hum = char:FindFirstChildOfClass("Humanoid")
            if hum then
                hum:MoveTo(Vector3.new(0, 0, 0))
            end
        end
    end
end

-- ===== FITUR 1: BLACKSCREEN =====
local function toggleBlackScreen()
    blackscreenActive = not blackscreenActive
    
    if blackscreenActive then
        BlackScreen.BackgroundTransparency = 0
        BlackScreenBtn.Text = "🔲 BLACKSCREEN: ON"
        BlackScreenBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 0)
        StatusLabel.Text = "⚡ FPS Boost: ON (black screen)"
        
        Lighting.Brightness = 0
        Lighting.Ambient = Color3.fromRGB(0, 0, 0)
        Lighting.GlobalShadows = false
        Lighting.FogEnd = 0.1
        
        for _, v in pairs(Workspace:GetDescendants()) do
            if v:IsA("ParticleEmitter") or v:IsA("Trail") or v:IsA("Fire") or v:IsA("Smoke") or v:IsA("Sparkles") then
                v.Enabled = false
            end
            if v:IsA("BasePart") and v.Material ~= Enum.Material.Plastic and v.Material ~= Enum.Material.SmoothPlastic then
                if not originalPartProperties[v] then
                    originalPartProperties[v] = v.Material
                end
                v.Material = Enum.Material.SmoothPlastic
            end
        end
    else
        BlackScreen.BackgroundTransparency = 1
        BlackScreenBtn.Text = "🔲 BLACKSCREEN: OFF"
        BlackScreenBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
        StatusLabel.Text = "✅ FPS Boost: OFF"
        
        Lighting.Brightness = 1
        Lighting.Ambient = Color3.fromRGB(127, 127, 127)
        Lighting.GlobalShadows = true
        Lighting.FogEnd = 100000
        
        for _, v in pairs(Workspace:GetDescendants()) do
            if v:IsA("ParticleEmitter") or v:IsA("Trail") or v:IsA("Fire") or v:IsA("Smoke") or v:IsA("Sparkles") then
                v.Enabled = true
            end
            if v:IsA("BasePart") and originalPartProperties[v] then
                v.Material = originalPartProperties[v]
                originalPartProperties[v] = nil
            end
        end
    end
end

-- ===== FITUR 2: PLAYER KOTAK =====
local function toggleBoxPlayer()
    boxActive = not boxActive
    
    if boxActive then
        BoxBtn.Text = "📦 BOX PLAYER: ON"
        BoxBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 0)
        StatusLabel.Text = "📦 Semua player menjadi kotak"
        
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character then
                local char = player.Character
                for _, part in pairs(char:GetChildren()) do
                    if part:IsA("BasePart") then
                        if part.Name == "Head" then
                            part.Size = Vector3.new(1.5, 1.5, 1.5)
                        elseif part.Name == "HumanoidRootPart" then
                            part.Size = Vector3.new(1, 1, 1)
                        else
                            part.Size = Vector3.new(0.8, 0.8, 0.8)
                        end
                        part.Shape = Enum.PartType.Block
                        part.Material = Enum.Material.SmoothPlastic
                    end
                end
            end
        end
    else
        BoxBtn.Text = "📦 BOX PLAYER: OFF"
        BoxBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
        StatusLabel.Text = "✅ Box player: OFF"
        
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character then
                local char = player.Character
                for _, part in pairs(char:GetChildren()) do
                    if part:IsA("BasePart") then
                        part.Size = Vector3.new(1, 1, 1)
                    end
                end
            end
        end
    end
end

-- ===== FITUR 3: SEMBUNYIKAN GUI =====
local function toggleHideGUI()
    hideGUIActive = not hideGUIActive
    
    if hideGUIActive then
        HideGUIBtn.Text = "👁️ HIDE GUI: ON"
        HideGUIBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 0)
        StatusLabel.Text = "👁️ Semua GUI disembunyikan"
        
        for _, gui in pairs(PlayerGui:GetChildren()) do
            if gui ~= ScreenGui then
                originalGUIVisibility[gui] = gui.Enabled
                gui.Enabled = false
            end
        end
        
        local CoreGui = game:GetService("CoreGui")
        for _, gui in pairs(CoreGui:GetChildren()) do
            if gui:IsA("ScreenGui") and gui.Name ~= "RobloxGui" then
                originalGUIVisibility[gui] = gui.Enabled
                gui.Enabled = false
            end
        end
    else
        HideGUIBtn.Text = "👁️ HIDE GUI: OFF"
        HideGUIBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
        StatusLabel.Text = "✅ Semua GUI dikembalikan"
        
        for gui, visible in pairs(originalGUIVisibility) do
            if gui and gui.Parent then
                gui.Enabled = visible
            end
        end
        originalGUIVisibility = {}
    end
end

-- ===== CONNECT BUTTONS =====
BlackScreenBtn.MouseButton1Click:Connect(toggleBlackScreen)
BoxBtn.MouseButton1Click:Connect(toggleBoxPlayer)
HideGUIBtn.MouseButton1Click:Connect(toggleHideGUI)
AntiAFKBtn.MouseButton1Click:Connect(toggleAntiAFK)

-- ===== SHORTCUT KEYBOARD =====
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    
    if input.KeyCode == Enum.KeyCode.F1 then
        toggleBlackScreen()
    elseif input.KeyCode == Enum.KeyCode.F2 then
        toggleBoxPlayer()
    elseif input.KeyCode == Enum.KeyCode.F3 then
        toggleHideGUI()
    elseif input.KeyCode == Enum.KeyCode.F4 then
        toggleAntiAFK()
    elseif input.KeyCode == Enum.KeyCode.F5 then
        MainFrame.Visible = not MainFrame.Visible
    end
end)

-- ===== AUTO DETECT PLAYER JOIN =====
Players.PlayerAdded:Connect(function(player)
    wait(1)
    if boxActive and player.Character then
        local char = player.Character
        for _, part in pairs(char:GetChildren()) do
            if part:IsA("BasePart") then
                part.Size = Vector3.new(1, 1, 1)
                part.Shape = Enum.PartType.Block
            end
        end
    end
end)

-- ===== CLEANUP =====
game:BindToClose(function()
    if antiAFKCoroutine then
        coroutine.close(antiAFKCoroutine)
    end
    for gui, visible in pairs(originalGUIVisibility) do
        if gui and gui.Parent then
            gui.Enabled = visible
        end
    end
end)

print("⚡ OPTIMIZER HAZ LOADED!")
print("🔲 F1 = Blackscreen FPS Boost")
print("📦 F2 = Box Player")
print("👁️ F3 = Hide GUI")
print("🔄 F4 = Anti AFK")
print("🔄 F5 = Toggle Menu")
