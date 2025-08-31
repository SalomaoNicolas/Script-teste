local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Cria o RemoteEvent se nÃ£o existir
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
    {name="Comum", id="Comum"},
    {name="Raro", id="Raro"},
    {name="Ã‰pico", id="Epico"},
    {name="LendÃ¡rio", id="Lendario"},
    {name="OG", id="OG"}
}

for i, brainrot in ipairs(brainrots) do
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0, 160, 0, 40)
    button.Position = UDim2.new(0, 20, 0, 20 + (i-1)*50)
    button.Text = "Spawnar "..brainrot.name
    button.Parent = screenGui

    button.MouseButton1Click:Connect(function()
        spawnEvent:FireServer(brainrot.id)
    end)
end

-- Script do lado do servidor
if game:GetService("RunService"):IsServer() then
    spawnEvent.OnServerEvent:Connect(function(plr, tipo)
        -- Procura o modelo correspondente na pasta Brainrots
        local brainrotsFolder = ReplicatedStorage:FindFirstChild("Brainrots")
        if not brainrotsFolder then return end
        local model = brainrotsFolder:FindFirstChild(tipo)
        if model then
            local clone = model:Clone()
            local char = plr.Character
            if char and char:FindFirstChild("HumanoidRootPart") then
                clone.Parent = workspace
                if clone.PrimaryPart == nil then
                    clone.PrimaryPart = clone:FindFirstChildWhichIsA("BasePart")
                end
                if clone.PrimaryPart then
                    clone:SetPrimaryPartCFrame(char.HumanoidRootPart.CFrame * CFrame.new(0, 0, -5))
                else
                    clone:MoveTo(char.HumanoidRootPart.Position + Vector3.new(0,0,-5))
                end
            end
        end
    end)
end
