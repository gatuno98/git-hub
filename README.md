-- Script para exibir nome e linhas invisíveis entre os jogadores no Roblox
local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")

-- Função para criar a linha invisível entre os jogadores
local function createInvisibleLine(player1, player2)
    local part = Instance.new("Part")
    part.Size = Vector3.new(0.1, 0.1, (player1.Character.HumanoidRootPart.Position - player2.Character.HumanoidRootPart.Position).Magnitude)
    part.CFrame = CFrame.new((player1.Character.HumanoidRootPart.Position + player2.Character.HumanoidRootPart.Position) / 2,
                            player1.Character.HumanoidRootPart.Position)
    part.Anchored = true
    part.CanCollide = false
    part.Transparency = 1  -- Linha invisível
    part.Parent = Workspace
end

-- Função para criar o nome acima da cabeça do jogador
local function createNameTag(player)
    local head = player.Character:WaitForChild("Head")
    
    -- Criando o BillboardGui
    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Adornee = head
    billboardGui.Size = UDim2.new(0, 200, 0, 50)  -- Tamanho do nome
    billboardGui.StudsOffset = Vector3.new(0, 3, 0)  -- Posição do nome acima da cabeça
    billboardGui.Parent = head
    
    -- Criando o texto
    local textLabel = Instance.new("TextLabel")
    textLabel.Text = player.Name
    textLabel.Size = UDim2.new(1, 0, 1, 0)
    textLabel.TextColor3 = Color3.fromRGB(255, 0, 0)  -- Cor vermelha
    textLabel.TextScaled = true  -- Ajusta o texto automaticamente
    textLabel.BackgroundTransparency = 1  -- Remove fundo
    textLabel.Parent = billboardGui
end

-- Função para atualizar as linhas e tags a cada frame
local function updatePlayers()
    -- Limpando as linhas invisíveis existentes
    for _, part in pairs(Workspace:GetChildren()) do
        if part:IsA("Part") and part.Transparency == 1 then
            part:Destroy()
        end
    end

    -- Criando novas linhas invisíveis e tags para cada jogador
    for _, player1 in pairs(Players:GetPlayers()) do
        if player1.Character and player1.Character:FindFirstChild("HumanoidRootPart") then
            -- Criando a tag do nome
            if not player1.Character:FindFirstChild("BillboardGui") then
                createNameTag(player1)
            end
            
            for _, player2 in pairs(Players:GetPlayers()) do
                if player1 ~= player2 and player2.Character and player2.Character:FindFirstChild("HumanoidRootPart") then
                    -- Criando a linha invisível entre os jogadores
                    createInvisibleLine(player1, player2)
                end
            end
        end
    end
end

-- Atualizando os jogadores a cada frame
game:GetService("RunService").Heartbeat:Connect(updatePlayers)
