local CollectionService = game:GetService("CollectionService")
local Players = game:GetService("Players")

-- Marca todos os Parts colecionáveis com a tag "Brainrot"
-- No Roblox Studio, selecione o Part e use CollectionService:AddTag(part, "Brainrot")

-- Cria leaderstats para contar Brainrots
Players.PlayerAdded:Connect(function(player)
    local leaderstats = Instance.new("Folder")
    leaderstats.Name = "leaderstats"
    leaderstats.Parent = player

    local brainrotStat = Instance.new("IntValue")
    brainrotStat.Name = "Brainrots"
    brainrotStat.Value = 0
    brainrotStat.Parent = leaderstats
end)

-- Função para lidar com coleta
local function onTouched(part, player)
    -- Anti-spam: só deixa coletar 1x por player
    if part:GetAttribute("Collected") then return end

    local char = player.Character
    if not char then return end

    part:SetAttribute("Collected", true)
    part.Transparency = 1
    part.CanCollide = false

    -- Atualiza o contador do player
    local stats = player:FindFirstChild("leaderstats")
    if stats and stats:FindFirstChild("Brainrots") then
        stats.Brainrots.Value = stats.Brainrots.Value + 1
    end

    -- Efeito visual: você pode criar um efeito aqui (exemplo: part:Destroy() ou ParticleEmitter)
    task.wait(1)
    part:Destroy()
end

-- Conecta o evento de toque para todos os Brainrots
local function connectBrainrot(part)
    part.Touched:Connect(function(hit)
        local player = Players:GetPlayerFromCharacter(hit.Parent)
        if player then
            onTouched(part, player)
        end
    end)
end

for _, part in ipairs(CollectionService:GetTagged("Brainrot")) do
    connectBrainrot(part)
end

CollectionService:GetInstanceAddedSignal("Brainrot"):Connect(connectBrainrot)
