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
                for _, part in pairs(character:GetChildren()) do
                    if part:IsA("BasePart") then
                        if hitboxEnabled then
                            part.Size = part.Size -- Mantém o tamanho original
                            part.Transparency = 0.5 -- Torna o quadro transparente
                            part.Color = Color3.fromRGB(255, 0, 0) -- Vermelho
                        else
                            part.Transparency = 0 -- Remove a transparência
                            part.Color = Color3.fromRGB(255, 255, 255) -- Branco
                        end
                    end
                end
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
                    for _, part in pairs(character:GetChildren()) do
                        if part:IsA("BasePart") then
                            -- Verifica se o quadro foi acertado
                            if part.Transparency == 0.5 and part.Color == Color3.fromRGB(255, 0, 0) then
                                -- Código para matar o personagem
                                character:BreakJoints()
                            end
                        end
                    end
                end
            end
        end
    end
end)
