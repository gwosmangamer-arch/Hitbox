-- Complete Updated Roblox Menu Script
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer

-- ScreenGui Create karo (PlayerGui ke andar taaki block na ho)
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ProMenuGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

-- Main Menu Frame
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 320, 0, 400)
MainFrame.Position = UDim2.new(0.5, -160, 0.5, -200)
MainFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true -- Menu drag karke move hoga
MainFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = MainFrame

-- Title Bar
local TitleBar = Instance.new("Frame")
TitleBar.Size = UDim2.new(1, 0, 0, 40)
TitleBar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
TitleBar.BorderSizePixel = 0
TitleBar.Parent = MainFrame

local TitleCorner = Instance.new("UICorner")
TitleCorner.CornerRadius = UDim.new(0, 8)
TitleCorner.Parent = TitleBar

local TitleText = Instance.new("TextLabel")
TitleText.Size = UDim2.new(1, -40, 1, 0)
TitleText.Position = UDim2.new(0, 10, 0, 0)
TitleText.BackgroundTransparency = 1
TitleText.Text = "Roblox Pro Menu"
TitleText.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleText.TextSize = 16
TitleText.Font = Enum.Font.SourceSansBold
TitleText.TextXAlignment = Enum.TextXAlignment.Left
TitleText.Parent = TitleBar

-- Close / Minimize Button (-)
local ToggleBtn = Instance.new("TextButton")
ToggleBtn.Size = UDim2.new(0, 30, 0, 30)
ToggleBtn.Position = UDim2.new(1, -35, 0, 5)
ToggleBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
ToggleBtn.Text = "-"
ToggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleBtn.TextSize = 18
ToggleBtn.Font = Enum.Font.SourceSansBold
ToggleBtn.Parent = TitleBar

local ToggleCorner = Instance.new("UICorner")
ToggleCorner.CornerRadius = UDim.new(0, 6)
ToggleCorner.Parent = ToggleBtn

-- Scrolling Container for Features
local Container = Instance.new("ScrollingFrame")
Container.Size = UDim2.new(1, -16, 1, -55)
Container.Position = UDim2.new(0, 8, 0, 48)
Container.BackgroundTransparency = 1
Container.CanvasSize = UDim2.new(0, 0, 0, 550)
Container.ScrollBarThickness = 4
Container.Parent = MainFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Parent = Container
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 8)

-- Helper function for labels
local function addLabel(text)
    local lbl = Instance.new("TextLabel")
    lbl.Size = UDim2.new(1, 0, 0, 20)
    lbl.BackgroundTransparency = 1
    lbl.Text = text
    lbl.TextColor3 = Color3.fromRGB(200, 200, 200)
    lbl.TextSize = 13
    lbl.Font = Enum.Font.SourceSansBold
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    lbl.Parent = Container
end

-- 1. SPEED FEATURE (1 to 100 with Lock on Hit)
addLabel("Speed (1 to 100):")
local SpeedBox = Instance.new("TextBox")
SpeedBox.Size = UDim2.new(1, 0, 0, 35)
SpeedBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
SpeedBox.Text = "16"
SpeedBox.TextColor3 = Color3.fromRGB(255, 255, 255)
SpeedBox.TextSize = 14
SpeedBox.Parent = Container
local SC = Instance.new("UICorner") SC.CornerRadius = UDim.new(0, 6) SC.Parent = SpeedBox

local selectedSpeed = 16
SpeedBox.FocusLost:Connect(function()
    local val = tonumber(SpeedBox.Text)
    if val then
        selectedSpeed = math.clamp(val, 1, 100)
        SpeedBox.Text = tostring(selectedSpeed)
    end
end)

RunService.Heartbeat:Connect(function()
    local char = LocalPlayer.Character
    if char and char:FindFirstChild("Humanoid") then
        char.Humanoid.WalkSpeed = selectedSpeed
    end
end)

-- 2. INFINITE JUMP
local infJumpEnabled = false
local InfJumpBtn = Instance.new("TextButton")
InfJumpBtn.Size = UDim2.new(1, 0, 0, 35)
InfJumpBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
InfJumpBtn.Text = "Infinite Jump: OFF"
InfJumpBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
InfJumpBtn.TextSize = 14
InfJumpBtn.Parent = Container
local IJC = Instance.new("UICorner") IJC.CornerRadius = UDim.new(0, 6) IJC.Parent = InfJumpBtn

InfJumpBtn.MouseButton1Click:Connect(function()
    infJumpEnabled = not infJumpEnabled
    InfJumpBtn.Text = "Infinite Jump: " .. (infJumpEnabled and "ON" or "OFF")
    InfJumpBtn.BackgroundColor3 = infJumpEnabled and Color3.fromRGB(50, 150, 50) or Color3.fromRGB(60, 60, 60)
end)

UserInputService.JumpRequest:Connect(function()
    if infJumpEnabled then
        local char = LocalPlayer.Character
        if char and char:FindFirstChild("Humanoid") then
            char.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

-- 4. AURA / STUCK FIX
local auraFixEnabled = false
local AuraBtn = Instance.new("TextButton")
AuraBtn.Size = UDim2.new(1, 0, 0, 35)
AuraBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
AuraBtn.Text = "Aura Fall/Stuck Fix: OFF"
AuraBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
AuraBtn.TextSize = 14
AuraBtn.Parent = Container
local AFC = Instance.new("UICorner") AFC.CornerRadius = UDim.new(0, 6) AFC.Parent = AuraBtn

AuraBtn.MouseButton1Click:Connect(function()
    auraFixEnabled = not auraFixEnabled
    AuraBtn.Text = "Aura Fall/Stuck Fix: " .. (auraFixEnabled and "ON" or "OFF")
    AuraBtn.BackgroundColor3 = auraFixEnabled and Color3.fromRGB(50, 150, 50) or Color3.fromRGB(60, 60, 60)
end)

RunService.RenderStepped:Connect(function()
    if auraFixEnabled then
        local char = LocalPlayer.Character
        if char and char:FindFirstChild("Humanoid") then
            char.Humanoid.PlatformStand = false
            char.Humanoid:SetStateEnabled(Enum.HumanoidStateType.FallingDown, false)
            char.Humanoid:SetStateEnabled(Enum.HumanoidStateType.Ragdoll, false)
        end
    end
end)

-- 3. PLAYER LIST, FLY FOLLOW & STOP
addLabel("Server Players (Click to Fly & Follow):")
local PlayerScroll = Instance.new("ScrollingFrame")
PlayerScroll.Size = UDim2.new(1, 0, 0, 140)
PlayerScroll.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
PlayerScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
PlayerScroll.ScrollBarThickness = 4
PlayerScroll.Parent = Container

local PSCorner = Instance.new("UICorner") PSCorner.CornerRadius = UDim.new(0, 6) PSCorner.Parent = PlayerScroll

local PlayerListLayout = Instance.new("UIListLayout")
PlayerListLayout.Parent = PlayerScroll
PlayerListLayout.SortOrder = Enum.SortOrder.LayoutOrder

local targetPlayer = nil
local isFollowing = false

local function refreshPlayerList()
    for _, child in pairs(PlayerScroll:GetChildren()) do
        if child:IsA("TextButton") then child:Destroy() end
    end
    
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer then
            local pBtn = Instance.new("TextButton")
            pBtn.Size = UDim2.new(1, 0, 0, 28)
            pBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
            pBtn.Text = p.Name
            pBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
            pBtn.TextSize = 13
            pBtn.Parent = PlayerScroll
            
            pBtn.MouseButton1Click:Connect(function()
                if targetPlayer == p then
                    targetPlayer = nil
                    isFollowing = false
                    pBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
                else
                    targetPlayer = p
                    isFollowing = true
                    for _, c in pairs(PlayerScroll:GetChildren()) do
                        if c:IsA("TextButton") then c.BackgroundColor3 = Color3.fromRGB(50, 50, 50) end
                    end
                    pBtn.BackgroundColor3 = Color3.fromRGB(50, 150, 50)
                end
            end)
        end
    end
    PlayerScroll.CanvasSize = UDim2.new(0, 0, 0, PlayerListLayout.AbsoluteContentSize.Y)
end

Players.PlayerAdded:Connect(refreshPlayerList)
Players.PlayerRemoving:Connect(refreshPlayerList)
refreshPlayerList()

-- Stop Following Button
local StopBtn = Instance.new("TextButton")
StopBtn.Size = UDim2.new(1, 0, 0, 35)
StopBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
StopBtn.Text = "Stop Following Player"
StopBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
StopBtn.TextSize = 14
StopBtn.Parent = Container
local STC = Instance.new("UICorner") STC.CornerRadius = UDim.new(0, 6) STC.Parent = StopBtn

StopBtn.MouseButton1Click:Connect(function()
    targetPlayer = nil
    isFollowing = false
    for _, c in pairs(PlayerScroll:GetChildren()) do
        if c:IsA("TextButton") then c.BackgroundColor3 = Color3.fromRGB(50, 50, 50) end
    end
end)

-- Fly & Follow Loop Logic
RunService.RenderStepped:Connect(function()
    if isFollowing and targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local myChar = LocalPlayer.Character
        if myChar and myChar:FindFirstChild("HumanoidRootPart") then
            local targetPos = targetPlayer.Character.HumanoidRootPart.Position + Vector3.new(0, 3, 0)
            myChar.HumanoidRootPart.Velocity = Vector3.new(0, 0, 0)
            myChar.HumanoidRootPart.CFrame = CFrame.new(myChar.HumanoidRootPart.Position, targetPos)
            myChar.HumanoidRootPart.CFrame = CFrame.new(targetPos)
        end
    end
end)

-- Minimize / Open Close Menu Toggle Logic
local menuVisible = true
ToggleBtn.MouseButton1Click:Connect(function()
    menuVisible = not menuVisible
    Container.Visible = menuVisible
    MainFrame.Size = menuVisible and UDim2.new(0, 320, 0, 400) or UDim2.new(0, 320, 0, 40)
    ToggleBtn.Text = menuVisible and "-" or "+"
end)
