-- Configurações iniciais
local allowedTeamName = "Testers" -- nome do time permitido para usar o unlock

-- Função de verificação de time
local function isAllowedTeam()
    local player = game.Players.LocalPlayer
    return player.Team and player.Team.Name == allowedTeamName
end

-- Interface (UI simples com Drawing API)
local Drawing = Drawing or getgenv().Drawing

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

-- Função principal de Unlock
local function unlockAll()
    if not isAllowedTeam() then
        text.Text = "Acesso Negado"
        text.Color = Color3.fromRGB(255, 0, 0)
        return
    end

    -- Exemplo de unlock: liberar todas as ferramentas no StarterPack
    local player = game.Players.LocalPlayer
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

-- Detectar clique no UI
local UserInputService = game:GetService("UserInputService")

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        local mouse = game:GetService("Players").LocalPlayer:GetMouse()
        local mousePos = Vector2.new(mouse.X, mouse.Y)
        if mousePos.X >= box.Position.X and mousePos.X <= box.Position.X + box.Size.X and
           mousePos.Y >= box.Position.Y and mousePos.Y <= box.Position.Y + box.Size.Y then
            unlockAll()
        end
    end
end)
# Teste-projeto
