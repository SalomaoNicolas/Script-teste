local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Cria o RemoteEvent se não existir
local spawnEvent = ReplicatedStorage:FindFirstChild("SpawnBrainrotEvent")
if not spawnEvent then
    spawnEvent = Instance.new("RemoteEvent")
    spawnEvent.Name = "SpawnBrainrotEvent"
    spawnEvent.Parent = ReplicatedStorage
end

-- Cria a interface
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "BrainrotSpawnerGUI"
screenGui.Parent = player:WaitForChild("PlayerGui")

local brainrots = {
    {name="Dragon", id="Dragon"},
    {name="Cannelloni", id="Cannelloni"},
    {name="Los", id="Los"},
    {name="Combinasionas", id="Combinasionas"},
    {name="La Supreme Combinasion", id="La Supreme Combinasion"},
    {name="Ketupat Kepat", id="Ketupat Kepat"},
}

for i, brainrot in ipairs(brainrots) do
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0, 250, 0, 40)
    button.Position = UDim2.new(0, 20, 0, 20 + (i-1)*50)
    button.Text = "Spawnar "..brainrot.name.." na Passarela"
    button.Parent = screenGui

    button.MouseButton1Click:Connect(function()
        spawnEvent:FireServer(brainrot.id)
    end)
end

-- Script do lado do servidor (executa apenas se estiver em contexto de servidor)
if game:GetService("RunService"):IsServer() then
    spawnEvent.OnServerEvent:Connect(function(plr, tipo)
        local brainrotsFolder = ReplicatedStorage:FindFirstChild("Brainrots")
        if not brainrotsFolder then return end
        local model = brainrotsFolder:FindFirstChild(tipo)
        if model then
            local clone = model:Clone()
            clone.Parent = workspace

            -- Procura a Passarela no Workspace
            local passarela = workspace:FindFirstChild("Passarela")

            -- Define a PrimaryPart se não tiver
            if not clone.PrimaryPart then
                clone.PrimaryPart = clone:FindFirstChildWhichIsA("BasePart")
            end

            if passarela and passarela:IsA("BasePart") then
                -- Centraliza na passarela, levemente acima
                local pos = passarela.Position + Vector3.new(0, (passarela.Size.Y/2) + 4, 0)
                if clone.PrimaryPart then
                    clone:SetPrimaryPartCFrame(CFrame.new(pos))
                else
                    clone:MoveTo(pos)
                end
            elseif passarela and passarela:IsA("Model") and passarela.PrimaryPart then
                local pos = passarela.PrimaryPart.Position + Vector3.new(0, (passarela.PrimaryPart.Size.Y/2) + 4, 0)
                if clone.PrimaryPart then
                    clone:SetPrimaryPartCFrame(CFrame.new(pos))
                else
                    clone:MoveTo(pos)
                end
            else
                -- Se não existir passarela, spawna na frente do jogador
                local char = plr.Character
                if char and char:FindFirstChild("HumanoidRootPart") then
                    if clone.PrimaryPart then
                        clone:SetPrimaryPartCFrame(char.HumanoidRootPart.CFrame * CFrame.new(0, 0, -5))
                    else
                        clone:MoveTo(char.HumanoidRootPart.Position + Vector3.new(0,0,-5))
                    end
                end
            end
        end
    end)
end
