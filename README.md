-- Scrappy Life All-in-One Script (Speed, Hitbox, Auto Hit)
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Configurations (Aap inki values apne hisab se change kar sakte hain)
getgenv().SpeedEnabled = true
getgenv().WalkSpeedValue = 25 -- Normal speed se zyada (Default lagभग 16 hoti hai)

getgenv().HitboxEnabled = true
getgenv().HitboxSize = Vector3.new(8, 8, 8)

getgenv().AutoHitEnabled = true

-- 1. Speed Hack
RunService.Heartbeat:Connect(function()
    if getgenv().SpeedEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = getgenv().WalkSpeedValue
    end
end)

-- 2. Hitbox Expander
task.spawn(function()
    while true do
        task.wait(1)
        if getgenv().HitboxEnabled then
            for _, player in pairs(Players:GetPlayers()) do
                if player ~= LocalPlayer and player.Character then
                    local rootPart = player.Character:FindFirstChild("HumanoidRootPart")
                    if rootPart then
                        rootPart.Size = getgenv().HitboxSize
                        rootPart.Transparency = 0.6
                        rootPart.CanCollide = false
                    end
                end
            end
        end
    end
end)

-- 3. Auto Hit / Auto Attack Logic
task.spawn(function()
    while true do
        task.wait(0.1) -- Fast attack interval
        if getgenv().AutoHitEnabled and LocalPlayer.Character then
            -- Yahan game ka specific tool/weapon activate karne ka function hota hai
            local tool = LocalPlayer.Character:FindFirstChildOfClass("Tool")
            if tool then
                tool:Activate()
            end
        end
    end
end)

print("Scrappy Life Advanced Script Loaded Successfully!")
