-- GUI CORRIGIDA
local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ESP_Aimbot_GUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 150, 0, 40)
toggleButton.Position = UDim2.new(0, 20, 0, 20)
toggleButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.Font = Enum.Font.SourceSansBold
toggleButton.TextSize = 18
toggleButton.Text = "Ativar ESP/Aimbot"
toggleButton.Parent = screenGui

-- ESTADOS
local espEnabled = false
local aimbotEnabled = false

toggleButton.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    aimbotEnabled = not aimbotEnabled
    toggleButton.Text = espEnabled and "Desativar ESP/Aimbot" or "Ativar ESP/Aimbot"
end)
