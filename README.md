--==================================================
-- SLEEPYYY FPS BOOSTER
-- FPS + PING + SIGNAL HUD
-- LocalScript
--==================================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local Stats = game:GetService("Stats")

local player = Players.LocalPlayer

--==================================================
-- SETTINGS
--==================================================

local BOOST_ENABLED = true

--==================================================
-- FPS BOOST
--==================================================

local function boostGraphics()

	-- Kurangi efek Lighting
	Lighting.GlobalShadows = false
	Lighting.EnvironmentDiffuseScale = 0
	Lighting.EnvironmentSpecularScale = 0

	-- Hapus efek visual berat
	for _, obj in ipairs(Lighting:GetChildren()) do
		if obj:IsA("BlurEffect")
		or obj:IsA("DepthOfFieldEffect")
		or obj:IsA("SunRaysEffect")
		or obj:IsA("BloomEffect")
		or obj:IsA("ColorCorrectionEffect") then

			obj.Enabled = false
		end
	end

	-- Optimasi object
	for _, obj in ipairs(workspace:GetDescendants()) do

		if obj:IsA("ParticleEmitter")
		or obj:IsA("Trail")
		or obj:IsA("Beam") then

			obj.Enabled = false

		elseif obj:IsA("Smoke")
		or obj:IsA("Fire")
		or obj:IsA("Sparkles") then

			obj.Enabled = false

		elseif obj:IsA("BasePart") then

			-- Kurangi beban rendering
			obj.CastShadow = false
		end
	end
end

boostGraphics()

--==================================================
-- GUI
--==================================================

local gui = Instance.new("ScreenGui")
gui.Name = "SleepyyyFPS"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.Parent = player:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0,185,0,105)
frame.Position = UDim2.new(1,-200,0,15)
frame.BackgroundColor3 = Color3.fromRGB(15,15,20)
frame.BackgroundTransparency = 0.12
frame.BorderSizePixel = 0
frame.Parent = gui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0,12)
corner.Parent = frame

local stroke = Instance.new("UIStroke")
stroke.Thickness = 1
stroke.Transparency = 0.35
stroke.Color = Color3.fromRGB(100,100,120)
stroke.Parent = frame

--==================================================
-- TEXT
--==================================================

local fpsText = Instance.new("TextLabel")
fpsText.Size = UDim2.new(1,-20,0,25)
fpsText.Position = UDim2.new(0,10,0,8)
fpsText.BackgroundTransparency = 1
fpsText.Text = "FPS : 0"
fpsText.TextColor3 = Color3.fromRGB(255,255,255)
fpsText.TextSize = 14
fpsText.Font = Enum.Font.GothamBold
fpsText.TextXAlignment = Enum.TextXAlignment.Left
fpsText.Parent = frame

local pingText = Instance.new("TextLabel")
pingText.Size = UDim2.new(1,-20,0,25)
pingText.Position = UDim2.new(0,10,0,33)
pingText.BackgroundTransparency = 1
pingText.Text = "PING : 0 ms"
pingText.TextColor3 = Color3.fromRGB(255,255,255)
pingText.TextSize = 14
pingText.Font = Enum.Font.GothamBold
pingText.TextXAlignment = Enum.TextXAlignment.Left
pingText.Parent = frame

local signalText = Instance.new("TextLabel")
signalText.Size = UDim2.new(1,-20,0,25)
signalText.Position = UDim2.new(0,10,0,58)
signalText.BackgroundTransparency = 1
signalText.Text = "SIGNAL : ●"
signalText.TextColor3 = Color3.fromRGB(100,255,160)
signalText.TextSize = 13
signalText.Font = Enum.Font.GothamBold
signalText.TextXAlignment = Enum.TextXAlignment.Left
signalText.Parent = frame

local byText = Instance.new("TextLabel")
byText.Size = UDim2.new(1,-20,0,18)
byText.Position = UDim2.new(0,10,1,-21)
byText.BackgroundTransparency = 1
byText.Text = "by sleeppyyy"
byText.TextColor3 = Color3.fromRGB(130,130,145)
byText.TextSize = 10
byText.Font = Enum.Font.Gotham
byText.TextXAlignment = Enum.TextXAlignment.Right
byText.Parent = frame

--==================================================
-- FPS COUNTER
--==================================================

local frames = 0
local lastTime = os.clock()
local fps = 0

RunService.RenderStepped:Connect(function()

	frames += 1

	local now = os.clock()

	if now - lastTime >= 1 then

		fps = frames
		frames = 0
		lastTime = now

		fpsText.Text = "FPS : "..fps

		-- Warna FPS
		if fps >= 50 then
			fpsText.TextColor3 =
				Color3.fromRGB(100,255,160)

		elseif fps >= 30 then
			fpsText.TextColor3 =
				Color3.fromRGB(255,220,100)

		else
			fpsText.TextColor3 =
				Color3.fromRGB(255,100,100)
		end
	end
end)

--==================================================
-- PING
--==================================================

task.spawn(function()

	while true do

		task.wait(1)

		local ping = 0

		pcall(function()
			ping = math.floor(
				player:GetNetworkPing() * 1000
			)
		end)

		pingText.Text =
			"PING : "..ping.." ms"

		-- Signal indicator
		if ping <= 60 then

			signalText.Text =
				"SIGNAL : ●●●●"

			signalText.TextColor3 =
				Color3.fromRGB(100,255,160)

		elseif ping <= 120 then

			signalText.Text =
				"SIGNAL : ●●●○"

			signalText.TextColor3 =
				Color3.fromRGB(180,255,100)

		elseif ping <= 200 then

			signalText.Text =
				"SIGNAL : ●●○○"

			signalText.TextColor3 =
				Color3.fromRGB(255,220,100)

		else

			signalText.Text =
				"SIGNAL : ●○○○"

			signalText.TextColor3 =
				Color3.fromRGB(255,100,100)

		end
	end
end)BoxBtn.Position = UDim2.new(0.5, -130, 0.35, 0)
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
