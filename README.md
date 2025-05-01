-- CONFIG
local aimbotEnabled = false
local espEnabled = false
local teamCheck = true
local aimFov = 100

-- GUI
local ScreenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
local ToggleButton = Instance.new("TextButton", ScreenGui)
ToggleButton.Size = UDim2.new(0, 100, 0, 30)
ToggleButton.Position = UDim2.new(0, 10, 0, 10)
ToggleButton.Text = "ESP/Aimbot"

ToggleButton.MouseButton1Click:Connect(function()
    aimbotEnabled = not aimbotEnabled
    espEnabled = not espEnabled
    ToggleButton.Text = aimbotEnabled and "Desligar" or "ESP/Aimbot"
end)

-- ESP Function
function createESP(player)
    if player == game.Players.LocalPlayer then return end
    local box = Drawing.new("Square")
    box.Thickness = 2
    box.Size = Vector2.new(50, 100)
    box.Color = Color3.new(1, 0, 0)
    box.Visible = true

    game:GetService("RunService").RenderStepped:Connect(function()
        if espEnabled and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local pos, onScreen = workspace.CurrentCamera:WorldToViewportPoint(player.Character.HumanoidRootPart.Position)
            box.Position = Vector2.new(pos.X - 25, pos.Y - 50)
            box.Visible = onScreen
        else
            box.Visible = false
        end
    end)
end

-- Aimbot Function
function getClosestPlayer()
    local closestPlayer = nil
    local shortestDistance = math.huge

    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            if teamCheck and player.Team == game.Players.LocalPlayer.Team then
                continue
            end

            local pos, onScreen = workspace.CurrentCamera:WorldToViewportPoint(player.Character.HumanoidRootPart.Position)
            local distance = (Vector2.new(pos.X, pos.Y) - Vector2.new(mouse.X, mouse.Y)).Magnitude

            if distance < aimFov and distance < shortestDistance then
                shortestDistance = distance
                closestPlayer = player
            end
        end
    end

    return closestPlayer
end

-- Aimbot Execution
game:GetService("RunService").RenderStepped:Connect(function()
    if aimbotEnabled then
        local target = getClosestPlayer()
        if target and target.Character and target.Character:FindFirstChild("Head") then
            workspace.CurrentCamera.CFrame = CFrame.new(workspace.CurrentCamera.CFrame.Position, target.Character.Head.Position)
        end
    end
end)

-- Init ESP
for _, player in pairs(game.Players:GetPlayers()) do
    createESP(player)
end

game.Players.PlayerAdded:Connect(createESP)
