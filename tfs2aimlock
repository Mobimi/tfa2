--// AIM LOCK SCRIPT WITH FOV + GUI SYSTEM //--

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local camera = workspace.CurrentCamera
local player = Players.LocalPlayer

-- Config
local aimPartName = "Head"
local zombiesFolder = workspace:WaitForChild("Zombies")

-- State
local aimLockActive = false
local aimFOV = 100
local draggingSlider = false
local uiVisible = true
local fovVisible = true
local espVisible = false  -- Mặc định ESP Zombies sẽ ẩn

----------------------------------------------------------------
-- GUI SYSTEM
----------------------------------------------------------------
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AimLockGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Main frame chứa tất cả control
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 260, 0, 170)
mainFrame.Position = UDim2.new(0.5, -130, 0.5, -85) -- Giữa màn hình
mainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui
mainFrame.Active = true
mainFrame.Draggable = true

-- Toggle AimLock Button
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 200, 0, 40)
toggleButton.Position = UDim2.new(0, 30, 0, 10)
toggleButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
toggleButton.Text = "AimLock Off"
toggleButton.Parent = mainFrame
toggleButton.TextColor3 = Color3.new(1,1,1)
toggleButton.Font = Enum.Font.SourceSansBold
toggleButton.TextSize = 20

toggleButton.MouseButton1Click:Connect(function()
	aimLockActive = not aimLockActive
	if aimLockActive then
		toggleButton.Text = "AimLock On"
		toggleButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
	else
		toggleButton.Text = "AimLock Off"
		toggleButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
	end
end)

-- FOV Label
local fovLabel = Instance.new("TextLabel")
fovLabel.Size = UDim2.new(0, 200, 0, 25)
fovLabel.Position = UDim2.new(0, 30, 0, 60)
fovLabel.BackgroundTransparency = 1
fovLabel.TextColor3 = Color3.new(1,1,1)
fovLabel.Text = "Aim FOV: " .. aimFOV
fovLabel.Font = Enum.Font.SourceSans
fovLabel.TextSize = 18
fovLabel.Parent = mainFrame

-- FOV Slider
local slider = Instance.new("TextButton")
slider.Size = UDim2.new(0, 200, 0, 20)
slider.Position = UDim2.new(0, 30, 0, 90)
slider.BackgroundColor3 = Color3.fromRGB(90, 90, 90)
slider.Text = ""
slider.Parent = mainFrame

local sliderKnob = Instance.new("Frame")
sliderKnob.Size = UDim2.new(0, 10, 0, 20)
sliderKnob.Position = UDim2.new(0, (aimFOV / 300) * 200, 0, 0)
sliderKnob.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
sliderKnob.Parent = slider

-- Xử lý kéo slider
slider.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		draggingSlider = true
	end
end)

slider.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		draggingSlider = false
	end
end)

-- FOV vòng tròn hiển thị
local fovCircle = Drawing.new("Circle")
fovCircle.Color = Color3.fromRGB(0, 170, 255)
fovCircle.Thickness = 2
fovCircle.NumSides = 64
fovCircle.Radius = aimFOV
fovCircle.Filled = false
fovCircle.Visible = true

-- Ẩn/Hiện GUI chính
local toggleUIBtn = Instance.new("TextButton")
toggleUIBtn.Size = UDim2.new(0, 100, 0, 40)
toggleUIBtn.Position = UDim2.new(0, 10, 0, 10) -- Nút ở phía trên bên trái
toggleUIBtn.BackgroundColor3 = Color3.fromRGB(100, 100, 255)
toggleUIBtn.Text = "Hide UI"
toggleUIBtn.Parent = screenGui
toggleUIBtn.TextColor3 = Color3.new(1,1,1)
toggleUIBtn.Font = Enum.Font.SourceSansBold
toggleUIBtn.TextSize = 20

-- Nút Show/Hide FOV
local toggleFovBtn = Instance.new("TextButton")
toggleFovBtn.Size = UDim2.new(0, 200, 0, 40)
toggleFovBtn.Position = UDim2.new(0, 30, 0, 140)
toggleFovBtn.BackgroundColor3 = Color3.fromRGB(255, 255, 0)
toggleFovBtn.Text = "Hide FOV Circle"
toggleFovBtn.Parent = mainFrame
toggleFovBtn.TextColor3 = Color3.new(1,1,1)
toggleFovBtn.Font = Enum.Font.SourceSansBold
toggleFovBtn.TextSize = 20

-- Nút Show/Hide ESP Zombies
local toggleEspBtn = Instance.new("TextButton")
toggleEspBtn.Size = UDim2.new(0, 200, 0, 40)
toggleEspBtn.Position = UDim2.new(0, 30, 0, 190)
toggleEspBtn.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
toggleEspBtn.Text = "Hide Body ESP Zombies"
toggleEspBtn.Parent = mainFrame
toggleEspBtn.TextColor3 = Color3.new(1,1,1)
toggleEspBtn.Font = Enum.Font.SourceSansBold
toggleEspBtn.TextSize = 20

toggleUIBtn.MouseButton1Click:Connect(function()
	uiVisible = not uiVisible
	mainFrame.Visible = uiVisible
	if uiVisible then
		toggleUIBtn.Text = "Hide UI"
	else
		toggleUIBtn.Text = "Show UI"
	end
end)

toggleFovBtn.MouseButton1Click:Connect(function()
	fovVisible = not fovVisible
	fovCircle.Visible = fovVisible
	toggleFovBtn.Text = fovVisible and "Hide FOV Circle" or "Show FOV Circle"
end)

toggleEspBtn.MouseButton1Click:Connect(function()
	espVisible = not espVisible
	toggleEspBtn.Text = espVisible and "Hide Body ESP Zombies" or "Show Body ESP Zombies"
end)

----------------------------------------------------------------
-- HÀM TÌM ZOMBIE GẦN NHẤT VÀ CÓ TRONG FOV
----------------------------------------------------------------
local function getClosestZombieInFOV()
	local screenCenter = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)
	local closestDist = aimFOV
	local closestZombie = nil

	for _, zombie in ipairs(zombiesFolder:GetChildren()) do
		if zombie:IsA("Model") and zombie:FindFirstChild(aimPartName) then
			local part = zombie[aimPartName]
			local screenPos, onScreen = camera:WorldToScreenPoint(part.Position)
			if onScreen then
				local dist = (Vector2.new(screenPos.X, screenPos.Y) - screenCenter).Magnitude
				if dist < closestDist then
					closestDist = dist
					closestZombie = zombie
				end
			end
		end
	end

	return closestZombie
end

----------------------------------------------------------------
-- VÒNG LẶP CHÍNH
----------------------------------------------------------------
RunService.RenderStepped:Connect(function()
	-- Cập nhật slider nếu đang kéo
	if draggingSlider then
		local mouseX = UserInputService:GetMouseLocation().X
		local sliderX = slider.AbsolutePosition.X
		local relativeX = math.clamp(mouseX - sliderX, 0, 200)

		sliderKnob.Position = UDim2.new(0, relativeX, 0, 0)
		aimFOV = math.clamp(math.floor((relativeX / 200) * 300), 20, 300)
		fovLabel.Text = "Aim FOV: " .. aimFOV
	end

	-- Cập nhật vòng tròn FOV
	local center = camera.ViewportSize / 2
	fovCircle.Position = center
	fovCircle.Radius = aimFOV
	fovCircle.Visible = fovVisible

	-- AimLock xử lý
	if aimLockActive then
		local closestZombie = getClosestZombieInFOV()
		if closestZombie then
			local targetPart = closestZombie[aimPartName]
			camera.CFrame = CFrame.new(camera.CFrame.Position, targetPart.Position)
		end
	end
	
	-- Hiển thị ESP cho Zombies nếu bật
	if espVisible then
		for _, zombie in ipairs(zombiesFolder:GetChildren()) do
			if zombie:IsA("Model") then
				for _, part in pairs(zombie:GetChildren()) do
					if part:IsA("BasePart") then
						local screenPos, onScreen = camera:WorldToScreenPoint(part.Position)
						if onScreen then
							-- Outline ESP
							local espOutline = Drawing.new("Square")
							espOutline.Visible = true
							espOutline.Color = Color3.fromRGB(255, 0, 0)
							espOutline.Thickness = 3
							espOutline.Size = Vector2.new(100, 100)
							espOutline.Position = Vector2.new(screenPos.X - 50, screenPos.Y - 50)
						end
					end
				end
			end
		end
	end
end)
