local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Lighting = game:GetService("Lighting")
local SoundService = game:GetService("SoundService")

-- BOTÕES (você deve garantir que esses botões estão presentes na GUI do cliente)
local gui = script.Parent
local escolaBtn = gui:WaitForChild("EscolaBtn")
local hospitalBtn = gui:WaitForChild("HospitalBtn")
local casaBtn = gui:WaitForChild("CasaBtn")
local lojaBtn = gui:WaitForChild("LojaBtn")
local carroBtn = gui:WaitForChild("CarroBtn")
local roupasBtn = gui:WaitForChild("RoupasBtn")
local dinheiroBtn = gui:WaitForChild("DinheiroBtn")
local climaBtn = gui:WaitForChild("ClimaBtn")
local musicaBtn = gui:WaitForChild("MusicaBtn")
local dancarBtn = gui:WaitForChild("DancarBtn")
local sentarBtn = gui:WaitForChild("SentarBtn")
local acenarBtn = gui:WaitForChild("AcenarBtn")
local speedBtn = gui:WaitForChild("SpeedBtn")
local pernaZumbiBtn = gui:WaitForChild("PernaZumbiBtn")
local pernaGeloBtn = gui:WaitForChild("PernaGeloBtn")

-- TELEPORTES
local teleportes = {
    EscolaBtn = Vector3.new(120, 10, -300),
    HospitalBtn = Vector3.new(50, 10, -100),
    CasaBtn = Vector3.new(200, 10, 50),
    LojaBtn = Vector3.new(-100, 10, -200)
}

for btnName, pos in pairs(teleportes) do
    local btn = gui:FindFirstChild(btnName)
    if btn then
        btn.MouseButton1Click:Connect(function()
            char:MoveTo(pos)
        end)
    end
end

-- CARRO
carroBtn.MouseButton1Click:Connect(function()
    local carroModelo = ReplicatedStorage:FindFirstChild("CarroBasico")
    if carroModelo then
        local carroClone = carroModelo:Clone()
        carroClone:SetPrimaryPartCFrame(char.HumanoidRootPart.CFrame * CFrame.new(5, 0, 10))
        carroClone.Parent = workspace
    end
end)

-- ROUPAS
roupasBtn.MouseButton1Click:Connect(function()
    local humanoid = char:FindFirstChildOfClass("Humanoid")
    if humanoid then
        humanoid:ApplyDescription(game.Players:GetHumanoidDescriptionFromUserId(12345678)) -- Substitua por seu UserId
    end
end)

-- DINHEIRO VISUAL
dinheiroBtn.MouseButton1Click:Connect(function()
    local dinheiro = Instance.new("BillboardGui")
    dinheiro.Size = UDim2.new(0, 100, 0, 50)
    dinheiro.StudsOffset = Vector3.new(0, 3, 0)
    dinheiro.Adornee = char:FindFirstChild("Head")
    dinheiro.Parent = char
    local label = Instance.new("TextLabel", dinheiro)
    label.Size = UDim2.new(1, 0, 1, 0)
    label.BackgroundTransparency = 1
    label.TextScaled = true
    label.Text = "$1000"
    label.TextColor3 = Color3.new(0, 1, 0)
end)

-- CLIMA
local climas = {"Claro", "Chuva", "Noite"}
local climaAtual = 1
climaBtn.MouseButton1Click:Connect(function()
    climaAtual += 1
    if climaAtual > #climas then climaAtual = 1 end
    local clima = climas[climaAtual]

    if clima == "Claro" then
        Lighting.ClockTime = 14
        Lighting.FogEnd = 100000
    elseif clima == "Chuva" then
        Lighting.FogEnd = 100
        -- Adicione partículas de chuva no seu mapa se quiser
    elseif clima == "Noite" then
        Lighting.ClockTime = 22
    end
end)

-- MÚSICA
musicaBtn.MouseButton1Click:Connect(function()
    local som = Instance.new("Sound", SoundService)
    som.SoundId = "rbxassetid://1843529273" -- Música exemplo
    som.Volume = 1
    som:Play()
end)

-- ANIMAÇÕES
local function tocarAnim(animId)
    local humanoid = char:FindFirstChildOfClass("Humanoid")
    if humanoid then
        local anim = Instance.new("Animation")
        anim.AnimationId = "rbxassetid://"..animId
        local track = humanoid:LoadAnimation(anim)
        track:Play()
    end
end

dancarBtn.MouseButton1Click:Connect(function()
    tocarAnim(507771019) -- Dançar
end)

sentarBtn.MouseButton1Click:Connect(function()
    tocarAnim(2506281703) -- Sentar
end)

acenarBtn.MouseButton1Click:Connect(function()
    tocarAnim(3394048354) -- Acenar
end)

-- SPEED BOOST
local speedAtivo = false
speedBtn.MouseButton1Click:Connect(function()
    local humanoid = char:FindFirstChildOfClass("Humanoid")
    if humanoid then
        speedAtivo = not speedAtivo
        humanoid.WalkSpeed = speedAtivo and 50 or 16
        speedBtn.Text = speedAtivo and "Velocidade: ON" or "Velocidade: OFF"
    end
end)

-- PERNAS ZUMBI
pernaZumbiBtn.MouseButton1Click:Connect(function()
    local humanoid = char:FindFirstChildOfClass("Humanoid")
    if humanoid then
        local desc = humanoid:GetAppliedDescription()
        desc.LeftLeg = 25063605  -- ID da perna zumbi esquerda
        desc.RightLeg = 25063607 -- ID da perna zumbi direita
        humanoid:ApplyDescription(desc)
    end
end)

-- PERNAS DE GELO
pernaGeloBtn.MouseButton1Click:Connect(function()
    local humanoid = char:FindFirstChildOfClass("Humanoid")
    if humanoid then
        local desc = humanoid:GetAppliedDescription()
        desc.LeftLeg = 61668970  -- ID da perna de gelo esquerda
        desc.RightLeg = 61668976 -- ID da perna de gelo direita
        humanoid:ApplyDescription(desc)
    end
end)
