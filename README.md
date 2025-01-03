local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local ScreenGui = Instance.new("ScreenGui")
local ToggleButton = Instance.new("TextButton")

-- Configurações do botão
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
ToggleButton.Parent = ScreenGui
ToggleButton.Size = UDim2.new(0, 100, 0, 50)
ToggleButton.Position = UDim2.new(0, 10, 0, 10)
ToggleButton.Text = "Ativar Hitbox"
ToggleButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)

local hitboxEnabled = false

-- Função para alternar a hitbox
local function toggleHitbox()
    hitboxEnabled = not hitboxEnabled
    ToggleButton.Text = hitboxEnabled and "Desativar Hitbox" or "Ativar Hitbox"
    
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            local character = player.Character
            if character then
                local hitbox = character:FindFirstChild("Hitbox")
                if not hitbox then
                    hitbox = Instance.new("Part")
                    hitbox.Name = "Hitbox"
                    hitbox.Size = Vector3.new(10, 10, 10) -- Tamanho da hitbox
                    hitbox.Transparency = 0.5 -- Transparência da hitbox
                    hitbox.Color = Color3.fromRGB(255, 0, 0) -- Cor da hitbox
                    hitbox.Anchored = true
                    hitbox.CanCollide = false
                    hitbox.Parent = character
                end
                hitbox.CFrame = character.PrimaryPart.CFrame
                hitbox.Visible = hitboxEnabled
            end
        end
    end
end

ToggleButton.MouseButton1Click:Connect(toggleHitbox)

-- Função para garantir que o jogo não trave
RunService.Heartbeat:Connect(function()
    if hitboxEnabled then
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer then
                local character = player.Character
                if character then
                    local hitbox = character:FindFirstChild("Hitbox")
                    if hitbox then
                        hitbox.CFrame = character.PrimaryPart.CFrame
                    end
                end
            end
        end
    end
end)

-- Sistema de antiban (simples)
local function onPlayerAdded(player)
    player.Chatted:Connect(function(message)
        if message:lower():find("ban") then
            player:Kick("Tentativa de ban detectada.")
        end
    end)
end

Players.PlayerAdded:Connect(onPlayerAdded)
for _, player in pairs(Players:GetPlayers()) do
    onPlayerAdded(player)
end
