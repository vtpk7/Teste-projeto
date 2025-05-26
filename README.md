-- Configuração
local allowedTeamName = "Testers"
local unlocked = false

-- Verifica se Drawing está disponível
local Drawing = Drawing or getgenv().Drawing
if not Drawing then
    warn("Drawing API não está disponível no seu exploit.")
    return
end

-- Verifica se está rodando como LocalScript
local Players = game:GetService("Players")
local player = Players.LocalPlayer
if not player then
    warn("Este script deve ser executado como um LocalScript.")
    return
end

-- Função de verificação de time
local function isAllowedTeam()
    return player.Team and player.Team.Name == allowedTeamName
end

-- Criar UI com Drawing API
local box = Drawing.new("Square")
box.Size = Vector2.new(200, 100)
box.Position = Vector2.new(100, 100)
box.Color = Color3.fromRGB(30, 30, 30)
box.Filled = true
box.Visible = true

local text = Drawing.new("Text")
text.Text = "Unlock All"
text.Size = 20
text.Position = Vector2.new(120, 120)
text.Color = Color3.fromRGB(255, 255, 255)
text.Visible = true

-- Função principal
local function unlockAll()
    if unlocked then return end

    if not isAllowedTeam() then
        text.Text = "Acesso Negado"
        text.Color = Color3.fromRGB(255, 0, 0)
        return
    end

    unlocked = true

    local backpack = player:WaitForChild("Backpack")
    local starterPack = game:GetService("StarterPack")

    for _, tool in pairs(starterPack:GetChildren()) do
        if tool:IsA("Tool") then
            local clone = tool:Clone()
            clone.Parent = backpack
        end
    end

    text.Text = "Desbloqueado!"
    text.Color = Color3.fromRGB(0, 255, 0)
end

-- Detectar clique na área do botão
local UserInputService = game:GetService("UserInputService")
local mouse = player:GetMouse()

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        local mousePos = Vector2.new(mouse.X, mouse.Y)

        if mousePos.X >= box.Position.X and mousePos.X <= box.Position.X + box.Size.X and
           mousePos.Y >= box.Position.Y and mousePos.Y <= box.Position.Y + box.Size.Y then
            unlockAll()
        end
    end
end)
