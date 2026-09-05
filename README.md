-- Services
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- ScreenGui Oluşturma
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "LynexHubGlobalPanel"
screenGui.ResetOnSpawn = false
screenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

-- Ana Panel Frame
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 390, 0, 210)
mainFrame.Position = UDim2.new(0.5, -195, 0.35, -105)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainFrame.BorderSizePixel = 0
mainFrame.ClipsDescendants = true
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = screenGui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 8)
corner.Parent = mainFrame

-- Kırmızı Neon Dış Çizgi
local stroke = Instance.new("UIStroke")
stroke.Name = "NeonRedStroke"
stroke.Color = Color3.fromRGB(255, 0, 0)
stroke.Thickness = 2.5
stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
stroke.Parent = mainFrame

---------------------------------------------------------
-- BAŞLIK VE KONTROL BUTONLARI (× VE + / -)
---------------------------------------------------------
local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, -70, 0, 35)
titleLabel.Position = UDim2.new(0, 10, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "LYNEXHUB GLOBAL PANEL SCRIPT"
titleLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
titleLabel.TextSize = 13
titleLabel.Font = Enum.Font.SourceSansBold
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.Parent = mainFrame

local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 25, 0, 25)
closeBtn.Position = UDim2.new(1, -30, 0, 5)
closeBtn.BackgroundColor3 = Color3.fromRGB(220, 50, 50)
closeBtn.Text = "×"
closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
closeBtn.TextSize = 18
closeBtn.Font = Enum.Font.SourceSansBold
closeBtn.Parent = mainFrame

local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(0, 4)
closeCorner.Parent = closeBtn

closeBtn.MouseButton1Click:Connect(function()
	screenGui:Destroy()
end)

local minBtn = Instance.new("TextButton")
minBtn.Size = UDim2.new(0, 25, 0, 25)
minBtn.Position = UDim2.new(1, -60, 0, 5)
minBtn.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
minBtn.Text = "+"
minBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
minBtn.TextSize = 18
minBtn.Font = Enum.Font.SourceSansBold
minBtn.Parent = mainFrame

local minCorner = Instance.new("UICorner")
minCorner.CornerRadius = UDim.new(0, 4)
minCorner.Parent = minBtn

-- 1.0 SANİYE KİLİTLİ ANİMASYON
local isMinimized = false
local isAnimating = false
local normalSize = UDim2.new(0, 390, 0, 210)
local minimizedSize = UDim2.new(0, 390, 0, 35)
local tween1s = TweenInfo.new(1.0, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)

minBtn.MouseButton1Click:Connect(function()
	if isAnimating then return end
	isAnimating = true

	if not isMinimized then
		minBtn.Text = "-"
		minBtn.BackgroundColor3 = Color3.fromRGB(220, 50, 50)
		local tween = TweenService:Create(mainFrame, tween1s, {Size = minimizedSize})
		tween:Play()
		tween.Completed:Wait()
		isMinimized = true
	else
		minBtn.Text = "+"
		minBtn.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
		local tween = TweenService:Create(mainFrame, tween1s, {Size = normalSize})
		tween:Play()
		tween.Completed:Wait()
		isMinimized = false
	end

	isAnimating = false
end)

---------------------------------------------------------
-- ŞALTER, BÜYÜTÜLMÜŞ KISA BUTONLAR VE YAZI KUTUSU
---------------------------------------------------------
local function createRow(yPos, labelText, defaultVal)
	local row = Instance.new("Frame")
	row.Size = UDim2.new(1, -20, 0, 45)
	row.Position = UDim2.new(0, 10, 0, yPos)
	row.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	row.Parent = mainFrame

	local rCorner = Instance.new("UICorner")
	rCorner.CornerRadius = UDim.new(0, 6)
	rCorner.Parent = row

	local txt = Instance.new("TextLabel")
	txt.Size = UDim2.new(0, 105, 1, 0)
	txt.Position = UDim2.new(0, 8, 0, 0)
	txt.BackgroundTransparency = 1
	txt.Text = labelText
	txt.TextColor3 = Color3.fromRGB(255, 255, 255)
	txt.TextSize = 12
	txt.Font = Enum.Font.SourceSansBold
	txt.TextXAlignment = Enum.TextXAlignment.Left
	txt.Parent = row

	-- Büyütülmüş Kırmızı (-) Butonu
	local decBtn = Instance.new("TextButton")
	decBtn.Size = UDim2.new(0, 26, 0, 26)
	decBtn.Position = UDim2.new(1, -202, 0.5, -13)
	decBtn.BackgroundColor3 = Color3.fromRGB(220, 50, 50)
	decBtn.Text = "-"
	decBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
	decBtn.TextSize = 18
	decBtn.Font = Enum.Font.SourceSansBold
	decBtn.Parent = row

	local decCorner = Instance.new("UICorner")
	decCorner.CornerRadius = UDim.new(0, 5)
	decCorner.Parent = decBtn

	-- Büyütülmüş Yeşil (+) Butonu
	local incBtn = Instance.new("TextButton")
	incBtn.Size = UDim2.new(0, 26, 0, 26)
	incBtn.Position = UDim2.new(1, -172, 0.5, -13)
	incBtn.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
	incBtn.Text = "+"
	incBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
	incBtn.TextSize = 18
	incBtn.Font = Enum.Font.SourceSansBold
	incBtn.Parent = row

	local incCorner = Instance.new("UICorner")
	incCorner.CornerRadius = UDim.new(0, 5)
	incCorner.Parent = incBtn

	-- Sağında Yazı Kutusu (0.1 - 100 Sınırlandırmalı)
	local valBox = Instance.new("TextBox")
	valBox.Size = UDim2.new(0, 48, 0, 24)
	valBox.Position = UDim2.new(1, -140, 0.5, -12)
	valBox.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
	valBox.Text = tostring(defaultVal or 1)
	valBox.TextColor3 = Color3.fromRGB(255, 255, 255)
	valBox.TextSize = 12
	valBox.Font = Enum.Font.SourceSansBold
	valBox.Parent = row

	local boxCorner = Instance.new("UICorner")
	boxCorner.CornerRadius = UDim.new(0, 4)
	boxCorner.Parent = valBox

	-- Şalter Butonu
	local switchBtn = Instance.new("TextButton")
	switchBtn.Size = UDim2.new(0, 55, 0, 24)
	switchBtn.Position = UDim2.new(1, -65, 0.5, -12)
	switchBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
	switchBtn.Text = ""
	switchBtn.Parent = row

	local sCorner = Instance.new("UICorner")
	sCorner.CornerRadius = UDim.new(1, 0)
	sCorner.Parent = switchBtn

	local knob = Instance.new("Frame")
	knob.Size = UDim2.new(0, 18, 0, 18)
	knob.Position = UDim2.new(0, 3, 0.5, -9)
	knob.BackgroundColor3 = Color3.fromRGB(100, 10, 10)
	knob.Parent = switchBtn

	local kCorner = Instance.new("UICorner")
	kCorner.CornerRadius = UDim.new(1, 0)
	kCorner.Parent = knob

	local currentVal = defaultVal or 1

	local function setVal(v)
		v = math.clamp(v, 0.1, 100)
		v = math.floor(v * 10 + 0.5) / 10
		currentVal = v
		valBox.Text = tostring(v)
	end

	valBox.FocusLost:Connect(function()
		local num = tonumber(valBox.Text)
		if not num then
			setVal(currentVal)
		else
			setVal(num)
		end
	end)

	incBtn.MouseButton1Click:Connect(function()
		setVal(currentVal + 1)
	end)

	decBtn.MouseButton1Click:Connect(function()
		setVal(currentVal - 1)
	end)

	local function getVal()
		return currentVal
	end

	return row, switchBtn, knob, getVal
end

---------------------------------------------------------
-- GENEL UÇUŞ VE HAREKET FONKSİYONU (HAZIR OL DURUŞU İLE)
---------------------------------------------------------
local function processFlight(speedVal, bodyVel, bodyGyro)
	local char = LocalPlayer.Character
	local hrp = char and char:FindFirstChild("HumanoidRootPart")
	local humanoid = char and char:FindFirstChildOfClass("Humanoid")
	local cam = workspace.CurrentCamera

	if hrp and humanoid and cam and bodyVel and bodyGyro then
		humanoid.PlatformStand = true
		bodyGyro.CFrame = cam.CFrame

		local moveDir = humanoid.MoveDirection
		if moveDir.Magnitude > 0 then
			local camCF = cam.CFrame
			local relMove = camCF:VectorToObjectSpace(moveDir)
			local moveVec = (camCF.LookVector * (-relMove.Z) + camCF.RightVector * relMove.X)

			if moveVec.Magnitude > 0 then
				moveVec = moveVec.Unit
			end

			local actualSpeed = speedVal * 16
			bodyVel.Velocity = moveVec * actualSpeed
		else
			bodyVel.Velocity = Vector3.new(0, 0, 0)
		end
	end
end

---------------------------------------------------------
-- 1. FLY BUTTON
---------------------------------------------------------
local _, flySwitch, flyKnob, flyGetVal = createRow(45, "FLY BUTTON", 1)
local flyActive = false
local flyVel, flyGyro

RunService.RenderStepped:Connect(function()
	if flyActive then
		processFlight(flyGetVal(), flyVel, flyGyro)
	end
end)

local function toggleFlyPhysics(state)
	local char = LocalPlayer.Character
	local hrp = char and char:FindFirstChild("HumanoidRootPart")
	local humanoid = char and char:FindFirstChildOfClass("Humanoid")

	if state and hrp then
		flyVel = Instance.new("BodyVelocity")
		flyVel.MaxForce = Vector3.new(1e5, 1e5, 1e5)
		flyVel.Velocity = Vector3.new(0, 0, 0)
		flyVel.Parent = hrp

		flyGyro = Instance.new("BodyGyro")
		flyGyro.MaxTorque = Vector3.new(1e5, 1e5, 1e5)
		flyGyro.CFrame = hrp.CFrame
		flyGyro.Parent = hrp
	else
		if flyVel then flyVel:Destroy() end
		if flyGyro then flyGyro:Destroy() end
		if humanoid then humanoid.PlatformStand = false end
	end
end

flySwitch.MouseButton1Click:Connect(function()
	flyActive = not flyActive
	local tInfo = TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
	if flyActive then
		TweenService:Create(flySwitch, tInfo, {BackgroundColor3 = Color3.fromRGB(120, 220, 120)}):Play()
		TweenService:Create(flyKnob, tInfo, {BackgroundColor3 = Color3.fromRGB(20, 120, 20), Position = UDim2.new(1, -21, 0.5, -9)}):Play()
		toggleFlyPhysics(true)
	else
		TweenService:Create(flySwitch, tInfo, {BackgroundColor3 = Color3.fromRGB(180, 40, 40)}):Play()
		TweenService:Create(flyKnob, tInfo, {BackgroundColor3 = Color3.fromRGB(100, 10, 10), Position = UDim2.new(0, 3, 0.5, -9)}):Play()
		toggleFlyPhysics(false)
	end
end)

---------------------------------------------------------
-- 2. NOCLIP BUTTON
---------------------------------------------------------
local _, noclipSwitch, noclipKnob, noclipGetVal = createRow(100, "NOCLIP BUTTON", 1)
local noclipActive = false
local noclipVel, noclipGyro

RunService.Stepped:Connect(function()
	if noclipActive and LocalPlayer.Character then
		for _, part in ipairs(LocalPlayer.Character:GetDescendants()) do
			if part:IsA("BasePart") then
				part.CanCollide = false
			end
		end
		processFlight(noclipGetVal(), noclipVel, noclipGyro)
	end
end)

local function toggleNoclipPhysics(state)
	local char = LocalPlayer.Character
	local hrp = char and char:FindFirstChild("HumanoidRootPart")
	local humanoid = char and char:FindFirstChildOfClass("Humanoid")

	if state and hrp then
		noclipVel = Instance.new("BodyVelocity")
		noclipVel.MaxForce = Vector3.new(1e5, 1e5, 1e5)
		noclipVel.Velocity = Vector3.new(0, 0, 0)
		noclipVel.Parent = hrp

		noclipGyro = Instance.new("BodyGyro")
		noclipGyro.MaxTorque = Vector3.new(1e5, 1e5, 1e5)
		noclipGyro.CFrame = hrp.CFrame
		noclipGyro.Parent = hrp
	else
		if noclipVel then noclipVel:Destroy() end
		if noclipGyro then noclipGyro:Destroy() end
		if humanoid then humanoid.PlatformStand = false end
	end
end

noclipSwitch.MouseButton1Click:Connect(function()
	noclipActive = not noclipActive
	local tInfo = TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
	if noclipActive then
		TweenService:Create(noclipSwitch, tInfo, {BackgroundColor3 = Color3.fromRGB(120, 220, 120)}):Play()
		TweenService:Create(noclipKnob, tInfo, {BackgroundColor3 = Color3.fromRGB(20, 120, 20), Position = UDim2.new(1, -21, 0.5, -9)}):Play()
		toggleNoclipPhysics(true)
	else
		TweenService:Create(noclipSwitch, tInfo, {BackgroundColor3 = Color3.fromRGB(180, 40, 40)}):Play()
		TweenService:Create(noclipKnob, tInfo, {BackgroundColor3 = Color3.fromRGB(100, 10, 10), Position = UDim2.new(0, 3, 0.5, -9)}):Play()
		toggleNoclipPhysics(false)
	end
end)

---------------------------------------------------------
-- 3. SPEED BUTTON
---------------------------------------------------------
local _, speedSwitch, speedKnob, speedGetVal = createRow(155, "SPEED BUTTON", 1)
local speedActive = false

RunService.Heartbeat:Connect(function()
	if speedActive and LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
		LocalPlayer.Character:FindFirstChildOfClass("Humanoid").WalkSpeed = speedGetVal() * 16
	end
end)

speedSwitch.MouseButton1Click:Connect(function()
	speedActive = not speedActive
	local tInfo = TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
	if speedActive then
		TweenService:Create(speedSwitch, tInfo, {BackgroundColor3 = Color3.fromRGB(120, 220, 120)}):Play()
		TweenService:Create(speedKnob, tInfo, {BackgroundColor3 = Color3.fromRGB(20, 120, 20), Position = UDim2.new(1, -21, 0.5, -9)}):Play()
	else
		TweenService:Create(speedSwitch, tInfo, {BackgroundColor3 = Color3.fromRGB(180, 40, 40)}):Play()
		TweenService:Create(speedKnob, tInfo, {BackgroundColor3 = Color3.fromRGB(100, 10, 10), Position = UDim2.new(0, 3, 0.5, -9)}):Play()
		if LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
			LocalPlayer.Character:FindFirstChildOfClass("Humanoid").WalkSpeed = 16
		end
	end
end)
