-- KamuiHub - GUI arrastável e com rolagem para "Steal a Brainrot"
-- Inclui: Inf Jump, GodMode, Teleporte, NoClip, Velocidade, Teleporte para Jogador, Click Teleport, Anti-AFK
-- NOVAS FUNÇÕES: Auto Steal, Auto Collect, Auto Lock, TP para Brainrot Players, Brainrot ESP

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer

-- CRIE UMA SCREENGUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "KamuiHub"
ScreenGui.Parent = player:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- Frame principal (arrastável)
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 360, 0, 540) -- Aumentado para mais funções
MainFrame.Position = UDim2.new(0.5, -180, 0.12, 0)
MainFrame.BackgroundTransparency = 1
MainFrame.Parent = ScreenGui
MainFrame.Active = true
MainFrame.Draggable = true

-- Barra superior
local TopBar = Instance.new("Frame")
TopBar.Size = UDim2.new(1, 0, 0, 36)
TopBar.Position = UDim2.new(0, 0, 0, 0)
TopBar.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
TopBar.BackgroundTransparency = 0.1
TopBar.Parent = MainFrame
local corner = Instance.new("UICorner", TopBar)
corner.CornerRadius = UDim.new(0, 18)
TopBar.Active = false

-- Título
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -70, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "KamuiHub - Brainrot"
Title.Font = Enum.Font.GothamBold
Title.TextColor3 = Color3.fromRGB(230, 230, 255)
Title.TextStrokeTransparency = 0.7
Title.TextSize = 20
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = TopBar

-- Botão minimizar
local MinButton = Instance.new("TextButton")
MinButton.Size = UDim2.new(0, 34, 0, 28)
MinButton.Position = UDim2.new(1, -68, 0.5, -14)
MinButton.Text = "-"
MinButton.Font = Enum.Font.GothamBold
MinButton.TextSize = 26
MinButton.BackgroundColor3 = Color3.fromRGB(85, 85, 120)
MinButton.TextColor3 = Color3.new(1, 1, 1)
MinButton.Parent = TopBar
local corner2 = Instance.new("UICorner", MinButton)
corner2.CornerRadius = UDim.new(0, 10)

MinButton.MouseEnter:Connect(function()
    MinButton.BackgroundColor3 = Color3.fromRGB(110,110,160)
end)
MinButton.MouseLeave:Connect(function()
    MinButton.BackgroundColor3 = Color3.fromRGB(85,85,120)
end)

-- Botão fechar
local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0, 34, 0, 28)
CloseButton.Position = UDim2.new(1, -34, 0.5, -14)
CloseButton.Text = "X"
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextSize = 22
CloseButton.BackgroundColor3 = Color3.fromRGB(140, 40, 60)
CloseButton.TextColor3 = Color3.new(1, 1, 1)
CloseButton.Parent = TopBar
local corner3 = Instance.new("UICorner", CloseButton)
corner3.CornerRadius = UDim.new(0, 10)

CloseButton.MouseEnter:Connect(function()
    CloseButton.BackgroundColor3 = Color3.fromRGB(190,70,90)
end)
CloseButton.MouseLeave:Connect(function()
    CloseButton.BackgroundColor3 = Color3.fromRGB(140,40,60)
end)

-- Sombra para TopBar
local function addShadow(obj)
    local shadow = Instance.new("ImageLabel")
    shadow.Name = "Shadow"
    shadow.Image = "rbxassetid://1316045217"
    shadow.ImageTransparency = 0.25
    shadow.ScaleType = Enum.ScaleType.Slice
    shadow.SliceCenter = Rect.new(10,10,118,118)
    shadow.Size = UDim2.new(1, 24, 1, 24)
    shadow.Position = UDim2.new(0, -12, 0, -12)
    shadow.BackgroundTransparency = 1
    shadow.ZIndex = obj.ZIndex - 1
    shadow.Parent = obj
end
addShadow(TopBar)

-- ScrollFrame para os botões (rolagem)
local BtnScroll = Instance.new("ScrollingFrame")
BtnScroll.Size = UDim2.new(1, 0, 1, -46)
BtnScroll.Position = UDim2.new(0, 0, 0, 46)
BtnScroll.BackgroundColor3 = Color3.fromRGB(20, 20, 40)
BtnScroll.BackgroundTransparency = 0.15
BtnScroll.BorderSizePixel = 0
BtnScroll.Parent = MainFrame
BtnScroll.CanvasSize = UDim2.new(0, 0, 0, 700) -- Aumentado para mais botões
BtnScroll.ScrollBarThickness = 8
local corner4 = Instance.new("UICorner", BtnScroll)
corner4.CornerRadius = UDim.new(0, 18)
addShadow(BtnScroll)

-- Template botão estilizado
local function createButton(txt, posY)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.94, 0, 0, 38)
    btn.Position = UDim2.new(0.03, 0, 0, posY)
    btn.Text = txt
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 16 -- Diminuído para caber mais texto
    btn.BackgroundColor3 = Color3.fromRGB(60, 85, 135)
    btn.TextColor3 = Color3.fromRGB(240, 240, 255)
    btn.AutoButtonColor = false
    btn.Parent = BtnScroll
    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 10)
    
    btn.MouseEnter:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(100,130,200)
    end)
    btn.MouseLeave:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(60,85,135)
    end)
    
    return btn
end

-- Botões existentes e novos
local posY = 10

-- FUNÇÕES ORIGINAIS
local InfJumpButton = createButton("Inf Jump: OFF", posY); posY = posY + 48
local GodModeButton = createButton("Invencibilidade: OFF", posY); posY = posY + 48
local TeleportButton = createButton("Teleporte para Base", posY); posY = posY + 48
local NoClipButton = createButton("NoClip: OFF", posY); posY = posY + 48
local SpeedButton = createButton("Velocidade: NORMAL", posY); posY = posY + 48
local TeleportToPlayerButton = createButton("Teleportar para Jogador", posY); posY = posY + 48
local ClickTeleportButton = createButton("Click Teleport: OFF (CTRL+Clique)", posY); posY = posY + 48

-- NOVAS FUNÇÕES PARA STEAL A BRAINROT
local AutoStealButton = createButton("Auto Steal Brainrot: OFF", posY); posY = posY + 48
local AutoCollectButton = createButton("Auto Collect Cash: OFF", posY); posY = posY + 48
local AutoLockButton = createButton("Auto Lock Target: OFF", posY); posY = posY + 48
local BrainrotESPButton = createButton("Brainrot ESP: OFF", posY); posY = posY + 48
local TpBrainrotButton = createButton("TP para Player c/ Brainrot", posY); posY = posY + 48

-- TextBox para nome do jogador
local TargetBox = Instance.new("TextBox")
TargetBox.Size = UDim2.new(0.94, 0, 0, 30)
TargetBox.Position = UDim2.new(0.03, 0, 0, posY)
TargetBox.BackgroundColor3 = Color3.fromRGB(40, 60, 110)
TargetBox.TextColor3 = Color3.new(1,1,1)
TargetBox.PlaceholderText = "Nome do Jogador para TP"
TargetBox.Font = Enum.Font.Gotham
TargetBox.TextSize = 16
TargetBox.Parent = BtnScroll
local corner5 = Instance.new("UICorner", TargetBox)
corner5.CornerRadius = UDim.new(0, 10)

posY = posY + 38
BtnScroll.CanvasSize = UDim2.new(0, 0, 0, posY + 20)

-- Ícone reabrir
local OpenIcon = Instance.new("ImageButton")
OpenIcon.Size = UDim2.new(0, 48, 0, 48)
OpenIcon.Position = UDim2.new(0, 20, 0.92, 0)
OpenIcon.Image = "rbxassetid://6031091002"
OpenIcon.BackgroundTransparency = 1
OpenIcon.Visible = false
OpenIcon.Parent = ScreenGui

-- VARIÁVEIS DE CONTROLE
local infJump = false
local godMode = false
local godModeLoop = nil
local noclipActive = false
local noclipConnection = nil
local speedActive = false
local normalSpeed = 16
local fastSpeed = 100
local clickTeleportActive = false
local clickTeleportConn

-- NOVAS VARIÁVEIS PARA STEAL A BRAINROT
local autoStealActive = false
local autoStealLoop = nil
local autoCollectActive = false
local autoCollectLoop = nil
local autoLockActive = false
local autoLockLoop = nil
local brainrotESPActive = false
local espConnections = {}

-- FUNÇÕES ORIGINAIS

-- Inf Jump
InfJumpButton.MouseButton1Click:Connect(function()
    infJump = not infJump
    InfJumpButton.Text = infJump and "Inf Jump: ON" or "Inf Jump: OFF"
end)

UserInputService.JumpRequest:Connect(function()
    if infJump then
        local char = player.Character or player.CharacterAdded:Wait()
        local hum = char:FindFirstChildOfClass("Humanoid")
        if hum then
            hum:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

-- GodMode
GodModeButton.MouseButton1Click:Connect(function()
    godMode = not godMode
    GodModeButton.Text = godMode and "Invencibilidade: ON" or "Invencibilidade: OFF"
    
    if godMode and not godModeLoop then
        godModeLoop = task.spawn(function()
            while godMode do
                local char = player.Character or player.CharacterAdded:Wait()
                local hum = char:FindFirstChildOfClass("Humanoid")
                if hum then
                    hum.Health = hum.MaxHealth
                    hum.MaxHealth = math.huge
                    hum.Health = math.huge
                    hum:SetStateEnabled(Enum.HumanoidStateType.Dead, false)
                end
                task.wait(0.05)
            end
            godModeLoop = nil
        end)
    elseif not godMode and godModeLoop then
        godModeLoop = nil
    end
end)

-- Teleporte (ajuste basePosition para sua base)
local basePosition = Vector3.new(100, 5, 200)
TeleportButton.MouseButton1Click:Connect(function()
    local char = player.Character or player.CharacterAdded:Wait()
    if char and char:FindFirstChild("HumanoidRootPart") then
        char.HumanoidRootPart.CFrame = CFrame.new(basePosition)
    end
end)

-- NoClip
local function onTouched(part)
    if not noclipActive then return end
    if not part:IsA("BasePart") then return end
    if not part.Anchored then return end
    if not part.CanCollide then return end
    
    local character = player.Character
    if not character then return end
    
    local hrp = character:FindFirstChild("HumanoidRootPart")
    if not hrp then return end
    
    if part.Position.Y < (hrp.Position.Y - hrp.Size.Y) then return end
    
    part.CanCollide = false
end

local function enableNoClip()
    if noclipConnection then
        noclipConnection:Disconnect()
    end
    local character = player.Character
    if not character then return end
    
    local hrp = character:FindFirstChild("HumanoidRootPart")
    if not hrp then return end
    
    noclipConnection = hrp.Touched:Connect(onTouched)
end

local function disableNoClip()
    if noclipConnection then
        noclipConnection:Disconnect()
        noclipConnection = nil
    end
end

NoClipButton.MouseButton1Click:Connect(function()
    noclipActive = not noclipActive
    NoClipButton.Text = noclipActive and "NoClip: ON" or "NoClip: OFF"
    
    disableNoClip()
    if noclipActive then
        enableNoClip()
    end
end)

player.CharacterAdded:Connect(function()
    if noclipActive then
        enableNoClip()
    end
end)

-- Velocidade
SpeedButton.MouseButton1Click:Connect(function()
    speedActive = not speedActive
    local char = player.Character or player.CharacterAdded:Wait()
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    
    if hum then
        if speedActive then
            hum.WalkSpeed = fastSpeed
            SpeedButton.Text = "Velocidade: RÁPIDA"
        else
            hum.WalkSpeed = normalSpeed
            SpeedButton.Text = "Velocidade: NORMAL"
        end
    end
end)

player.CharacterAdded:Connect(function(char)
    local hum = char:WaitForChild("Humanoid")
    hum.WalkSpeed = speedActive and fastSpeed or normalSpeed
end)

-- TP para outro jogador
TeleportToPlayerButton.MouseButton1Click:Connect(function()
    local targetUsername = TargetBox.Text
    if targetUsername ~= nil and targetUsername ~= "" then
        local targetPlayer = Players:FindFirstChild(targetUsername)
        local localChar = player.Character or player.CharacterAdded:Wait()
        
        if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") and localChar and localChar:FindFirstChild("HumanoidRootPart") then
            localChar.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
        else
            TargetBox.Text = "Jogador não encontrado!"
            wait(1.2)
            TargetBox.Text = ""
        end
    end
end)

-- Click Teleport
if _G.WRDClickTeleport == nil then
    _G.WRDClickTeleport = false
end

local function setClickTeleport(state)
    clickTeleportActive = state
    ClickTeleportButton.Text = clickTeleportActive and "Click Teleport: ON (CTRL+Clique)" or "Click Teleport: OFF (CTRL+Clique)"
    _G.WRDClickTeleport = clickTeleportActive
    
    if clickTeleportActive then
        if clickTeleportConn then
            clickTeleportConn:Disconnect()
        end
        local mouse = player:GetMouse()
        clickTeleportConn = UserInputService.InputBegan:Connect(function(input, gameProcessed)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                if _G.WRDClickTeleport and UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then
                    local char = player.Character or player.CharacterAdded:Wait()
                    if char then
                        local hrp = char:FindFirstChild("HumanoidRootPart") or char:FindFirstChild("Torso")
                        if hrp then
                            char:MoveTo(Vector3.new(mouse.Hit.x, mouse.Hit.y, mouse.Hit.z))
                        end
                    end
                end
            end
        end)
        game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Click teleport ativado (CTRL+Clique)"; Duration=5;})
    else
        if clickTeleportConn then
            clickTeleportConn:Disconnect()
            clickTeleportConn = nil
        end
        game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Click teleport desativado"; Duration=5;})
    end
end

ClickTeleportButton.MouseButton1Click:Connect(function()
    setClickTeleport(not clickTeleportActive)
end)

-- NOVAS FUNÇÕES PARA STEAL A BRAINROT

-- Auto Steal Brainrot
local function autoStealBrainrot()
    autoStealLoop = task.spawn(function()
        while autoStealActive do
            for _, targetPlayer in pairs(Players:GetPlayers()) do
                if targetPlayer ~= player and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    local distance = (player.Character.HumanoidRootPart.Position - targetPlayer.Character.HumanoidRootPart.Position).Magnitude
                    
                    if distance < 30 then
                        -- Tenta diferentes métodos de roubo
                        pcall(function()
                            -- Método 1: ClickDetector
                            if targetPlayer.Character:FindFirstChild("ClickDetector") then
                                fireclickdetector(targetPlayer.Character.ClickDetector)
                            end
                            
                            -- Método 2: Parte específica com ClickDetector
                            for _, part in pairs(targetPlayer.Character:GetChildren()) do
                                if part:FindFirstChild("ClickDetector") then
                                    fireclickdetector(part.ClickDetector)
                                end
                            end
                            
                            -- Método 3: RemoteEvent (ajustar nome conforme necessário)
                            if ReplicatedStorage:FindFirstChild("StealBrainrot") then
                                ReplicatedStorage.StealBrainrot:FireServer(targetPlayer)
                            elseif ReplicatedStorage:FindFirstChild("RemoteEvents") and ReplicatedStorage.RemoteEvents:FindFirstChild("Steal") then
                                ReplicatedStorage.RemoteEvents.Steal:FireServer(targetPlayer)
                            end
                        end)
                    end
                end
            end
            task.wait(0.5)
        end
    end)
end

AutoStealButton.MouseButton1Click:Connect(function()
    autoStealActive = not autoStealActive
    AutoStealButton.Text = autoStealActive and "Auto Steal Brainrot: ON" or "Auto Steal Brainrot: OFF"
    
    if autoStealActive then
        autoStealBrainrot()
        game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Auto Steal ativado!"; Duration=3;})
    else
        if autoStealLoop then
            task.cancel(autoStealLoop)
            autoStealLoop = nil
        end
        game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Auto Steal desativado!"; Duration=3;})
    end
end)

-- Auto Collect Cash
local function autoCollectCash()
    autoCollectLoop = task.spawn(function()
        while autoCollectActive do
            pcall(function()
                -- Procura por diferentes nomes de itens coletáveis
                local itemNames = {"Cash", "Coin", "Money", "Brainrot", "Item", "Collectible"}
                
                for _, itemName in pairs(itemNames) do
                    if workspace:FindFirstChild(itemName) then
                        for _, item in pairs(workspace[itemName]:GetChildren()) do
                            if item:FindFirstChild("ClickDetector") then
                                fireclickdetector(item.ClickDetector)
                            end
                        end
                    end
                end
                
                -- Procura diretamente no workspace
                for _, item in pairs(workspace:GetChildren()) do
                    if table.find(itemNames, item.Name) and item:FindFirstChild("ClickDetector") then
                        fireclickdetector(item.ClickDetector)
                    end
                end
            end)
            task.wait(0.1)
        end
    end)
end

AutoCollectButton.MouseButton1Click:Connect(function()
    autoCollectActive = not autoCollectActive
    AutoCollectButton.Text = autoCollectActive and "Auto Collect Cash: ON" or "Auto Collect Cash: OFF"
    
    if autoCollectActive then
        autoCollectCash()
        game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Auto Collect ativado!"; Duration=3;})
    else
        if autoCollectLoop then
            task.cancel(autoCollectLoop)
            autoCollectLoop = nil
        end
        game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Auto Collect desativado!"; Duration=3;})
    end
end)

-- Auto Lock Target
local function autoLock()
    autoLockLoop = task.spawn(function()
        while autoLockActive do
            local nearestPlayer = nil
            local nearestDistance = math.huge
            
            for _, targetPlayer in pairs(Players:GetPlayers()) do
                if targetPlayer ~= player and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    local distance = (player.Character.HumanoidRootPart.Position - targetPlayer.Character.HumanoidRootPart.Position).Magnitude
                    if distance < nearestDistance and distance < 50 then
                        nearestDistance = distance
                        nearestPlayer = targetPlayer
                    end
                end
            end
            
            if nearestPlayer and nearestPlayer.Character and player.Character then
                local targetPos = nearestPlayer.Character.HumanoidRootPart.Position
                local myPos = player.Character.HumanoidRootPart.Position
                local lookDirection = (targetPos - myPos).Unit
                
                player.Character.HumanoidRootPart.CFrame = CFrame.lookAt(myPos, myPos + lookDirection)
            end
            
            task.wait(0.1)
        end
    end)
end

AutoLockButton.MouseButton1Click:Connect(function()
    autoLockActive = not autoLockActive
    AutoLockButton.Text = autoLockActive and "Auto Lock Target: ON" or "Auto Lock Target: OFF"
    
    if autoLockActive then
        autoLock()
        game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Auto Lock ativado!"; Duration=3;})
    else
        if autoLockLoop then
            task.cancel(autoLockLoop)
            autoLockLoop = nil
        end
        game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Auto Lock desativado!"; Duration=3;})
    end
end)

-- Brainrot ESP
local function createESP(targetPlayer)
    if not targetPlayer.Character then return end
    
    local highlight = Instance.new("Highlight")
    highlight.Name = "BrainrotESP"
    highlight.Parent = targetPlayer.Character
    highlight.FillColor = Color3.fromRGB(255, 100, 255)
    highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
    highlight.FillTransparency = 0.5
    highlight.OutlineTransparency = 0
    
    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Name = "BrainrotNameESP"
    billboardGui.Parent = targetPlayer.Character:FindFirstChild("Head")
    billboardGui.Size = UDim2.new(0, 200, 0, 50)
    billboardGui.StudsOffset = Vector3.new(0, 2, 0)
    
    local nameLabel = Instance.new("TextLabel")
    nameLabel.Parent = billboardGui
    nameLabel.Size = UDim2.new(1, 0, 1, 0)
    nameLabel.BackgroundTransparency = 1
    nameLabel.Text = targetPlayer.Name
    nameLabel.TextColor3 = Color3.fromRGB(255, 100, 255)
    nameLabel.TextStrokeTransparency = 0
    nameLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
    nameLabel.Font = Enum.Font.GothamBold
    nameLabel.TextSize = 16
    
    espConnections[targetPlayer] = targetPlayer.CharacterRemoving:Connect(function()
        if highlight then highlight:Destroy() end
        if billboardGui then billboardGui:Destroy() end
    end)
end

local function toggleBrainrotESP()
    if brainrotESPActive then
        -- Ativar ESP
        for _, targetPlayer in pairs(Players:GetPlayers()) do
            if targetPlayer ~= player and targetPlayer.Character then
                createESP(targetPlayer)
            end
        end
        
        -- ESP para novos jogadores
        Players.PlayerAdded:Connect(function(newPlayer)
            if brainrotESPActive then
                newPlayer.CharacterAdded:Connect(function()
                    if brainrotESPActive then
                        createESP(newPlayer)
                    end
                end)
            end
        end)
    else
        -- Desativar ESP
        for _, targetPlayer in pairs(Players:GetPlayers()) do
            if targetPlayer.Character then
                local highlight = targetPlayer.Character:FindFirstChild("BrainrotESP")
                local billboard = targetPlayer.Character:FindFirstChild("Head") and targetPlayer.Character.Head:FindFirstChild("BrainrotNameESP")
                
                if highlight then highlight:Destroy() end
                if billboard then billboard:Destroy() end
            end
            
            if espConnections[targetPlayer] then
                espConnections[targetPlayer]:Disconnect()
                espConnections[targetPlayer] = nil
            end
        end
    end
end

BrainrotESPButton.MouseButton1Click:Connect(function()
    brainrotESPActive = not brainrotESPActive
    BrainrotESPButton.Text = brainrotESPActive and "Brainrot ESP: ON" or "Brainrot ESP: OFF"
    
    toggleBrainrotESP()
    
    local status = brainrotESPActive and "ativado" or "desativado"
    game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Brainrot ESP " .. status .. "!"; Duration=3;})
end)

-- TP para Jogador com Brainrot
TpBrainrotButton.MouseButton1Click:Connect(function()
    local bestTarget = nil
    local highestBrainrot = 0
    
    for _, targetPlayer in pairs(Players:GetPlayers()) do
        if targetPlayer ~= player and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
            -- Verifica leaderstats
            if targetPlayer:FindFirstChild("leaderstats") then
                for _, stat in pairs(targetPlayer.leaderstats:GetChildren()) do
                    if stat.Name:lower():find("brainrot") or stat.Name:lower():find("brain") then
                        if stat.Value > highestBrainrot then
                            highestBrainrot = stat.Value
                            bestTarget = targetPlayer
                        end
                    end
                end
            end
        end
    end
    
    if bestTarget then
        player.Character.HumanoidRootPart.CFrame = bestTarget.Character.HumanoidRootPart.CFrame
        game.StarterGui:SetCore("SendNotification", {
            Title="KamuiHub"; 
            Text="Teleportado para " .. bestTarget.Name .. " (" .. highestBrainrot .. " brainrots)"; 
            Duration=4;
        })
    else
        game.StarterGui:SetCore("SendNotification", {
            Title="KamuiHub"; 
            Text="Nenhum jogador com brainrot encontrado!"; 
            Duration=3;
        })
    end
end)

-- Minimizar e reabrir GUI
MinButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    OpenIcon.Visible = true
end)

OpenIcon.MouseButton1Click:Connect(function()
    MainFrame.Visible = true
    OpenIcon.Visible = false
end)

CloseButton.MouseButton1Click:Connect(function()
    MainFrame
-- KamuiHub - GUI arrastável e com rolagem para "Steal a Brainrot"
-- Inclui: Inf Jump, GodMode, Teleporte, NoClip, Velocidade, Teleporte para Jogador, Click Teleport, Anti-AFK
-- NOVAS FUNÇÕES: Auto Steal, Auto Collect, Auto Lock, TP para Brainrot Players, Brainrot ESP

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer

-- CRIE UMA SCREENGUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "KamuiHub"
ScreenGui.Parent = player:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- Frame principal (arrastável)
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 360, 0, 540) -- Aumentado para mais funções
MainFrame.Position = UDim2.new(0.5, -180, 0.12, 0)
MainFrame.BackgroundTransparency = 1
MainFrame.Parent = ScreenGui
MainFrame.Active = true
MainFrame.Draggable = true

-- Barra superior
local TopBar = Instance.new("Frame")
TopBar.Size = UDim2.new(1, 0, 0, 36)
TopBar.Position = UDim2.new(0, 0, 0, 0)
TopBar.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
TopBar.BackgroundTransparency = 0.1
TopBar.Parent = MainFrame
local corner = Instance.new("UICorner", TopBar)
corner.CornerRadius = UDim.new(0, 18)
TopBar.Active = false

-- Título
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -70, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "KamuiHub - Brainrot"
Title.Font = Enum.Font.GothamBold
Title.TextColor3 = Color3.fromRGB(230, 230, 255)
Title.TextStrokeTransparency = 0.7
Title.TextSize = 20
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = TopBar

-- Botão minimizar
local MinButton = Instance.new("TextButton")
MinButton.Size = UDim2.new(0, 34, 0, 28)
MinButton.Position = UDim2.new(1, -68, 0.5, -14)
MinButton.Text = "-"
MinButton.Font = Enum.Font.GothamBold
MinButton.TextSize = 26
MinButton.BackgroundColor3 = Color3.fromRGB(85, 85, 120)
MinButton.TextColor3 = Color3.new(1, 1, 1)
MinButton.Parent = TopBar
local corner2 = Instance.new("UICorner", MinButton)
corner2.CornerRadius = UDim.new(0, 10)

MinButton.MouseEnter:Connect(function()
    MinButton.BackgroundColor3 = Color3.fromRGB(110,110,160)
end)
MinButton.MouseLeave:Connect(function()
    MinButton.BackgroundColor3 = Color3.fromRGB(85,85,120)
end)

-- Botão fechar
local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0, 34, 0, 28)
CloseButton.Position = UDim2.new(1, -34, 0.5, -14)
CloseButton.Text = "X"
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextSize = 22
CloseButton.BackgroundColor3 = Color3.fromRGB(140, 40, 60)
CloseButton.TextColor3 = Color3.new(1, 1, 1)
CloseButton.Parent = TopBar
local corner3 = Instance.new("UICorner", CloseButton)
corner3.CornerRadius = UDim.new(0, 10)

CloseButton.MouseEnter:Connect(function()
    CloseButton.BackgroundColor3 = Color3.fromRGB(190,70,90)
end)
CloseButton.MouseLeave:Connect(function()
    CloseButton.BackgroundColor3 = Color3.fromRGB(140,40,60)
end)

-- Sombra para TopBar
local function addShadow(obj)
    local shadow = Instance.new("ImageLabel")
    shadow.Name = "Shadow"
    shadow.Image = "rbxassetid://1316045217"
    shadow.ImageTransparency = 0.25
    shadow.ScaleType = Enum.ScaleType.Slice
    shadow.SliceCenter = Rect.new(10,10,118,118)
    shadow.Size = UDim2.new(1, 24, 1, 24)
    shadow.Position = UDim2.new(0, -12, 0, -12)
    shadow.BackgroundTransparency = 1
    shadow.ZIndex = obj.ZIndex - 1
    shadow.Parent = obj
end
addShadow(TopBar)

-- ScrollFrame para os botões (rolagem)
local BtnScroll = Instance.new("ScrollingFrame")
BtnScroll.Size = UDim2.new(1, 0, 1, -46)
BtnScroll.Position = UDim2.new(0, 0, 0, 46)
BtnScroll.BackgroundColor3 = Color3.fromRGB(20, 20, 40)
BtnScroll.BackgroundTransparency = 0.15
BtnScroll.BorderSizePixel = 0
BtnScroll.Parent = MainFrame
BtnScroll.CanvasSize = UDim2.new(0, 0, 0, 700) -- Aumentado para mais botões
BtnScroll.ScrollBarThickness = 8
local corner4 = Instance.new("UICorner", BtnScroll)
corner4.CornerRadius = UDim.new(0, 18)
addShadow(BtnScroll)

-- Template botão estilizado
local function createButton(txt, posY)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.94, 0, 0, 38)
    btn.Position = UDim2.new(0.03, 0, 0, posY)
    btn.Text = txt
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 16 -- Diminuído para caber mais texto
    btn.BackgroundColor3 = Color3.fromRGB(60, 85, 135)
    btn.TextColor3 = Color3.fromRGB(240, 240, 255)
    btn.AutoButtonColor = false
    btn.Parent = BtnScroll
    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 10)
    
    btn.MouseEnter:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(100,130,200)
    end)
    btn.MouseLeave:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(60,85,135)
    end)
    
    return btn
end

-- Botões existentes e novos
local posY = 10

-- FUNÇÕES ORIGINAIS
local InfJumpButton = createButton("Inf Jump: OFF", posY); posY = posY + 48
local GodModeButton = createButton("Invencibilidade: OFF", posY); posY = posY + 48
local TeleportButton = createButton("Teleporte para Base", posY); posY = posY + 48
local NoClipButton = createButton("NoClip: OFF", posY); posY = posY + 48
local SpeedButton = createButton("Velocidade: NORMAL", posY); posY = posY + 48
local TeleportToPlayerButton = createButton("Teleportar para Jogador", posY); posY = posY + 48
local ClickTeleportButton = createButton("Click Teleport: OFF (CTRL+Clique)", posY); posY = posY + 48

-- NOVAS FUNÇÕES PARA STEAL A BRAINROT
local AutoStealButton = createButton("Auto Steal Brainrot: OFF", posY); posY = posY + 48
local AutoCollectButton = createButton("Auto Collect Cash: OFF", posY); posY = posY + 48
local AutoLockButton = createButton("Auto Lock Target: OFF", posY); posY = posY + 48
local BrainrotESPButton = createButton("Brainrot ESP: OFF", posY); posY = posY + 48
local TpBrainrotButton = createButton("TP para Player c/ Brainrot", posY); posY = posY + 48

-- TextBox para nome do jogador
local TargetBox = Instance.new("TextBox")
TargetBox.Size = UDim2.new(0.94, 0, 0, 30)
TargetBox.Position = UDim2.new(0.03, 0, 0, posY)
TargetBox.BackgroundColor3 = Color3.fromRGB(40, 60, 110)
TargetBox.TextColor3 = Color3.new(1,1,1)
TargetBox.PlaceholderText = "Nome do Jogador para TP"
TargetBox.Font = Enum.Font.Gotham
TargetBox.TextSize = 16
TargetBox.Parent = BtnScroll
local corner5 = Instance.new("UICorner", TargetBox)
corner5.CornerRadius = UDim.new(0, 10)

posY = posY + 38
BtnScroll.CanvasSize = UDim2.new(0, 0, 0, posY + 20)

-- Ícone reabrir
local OpenIcon = Instance.new("ImageButton")
OpenIcon.Size = UDim2.new(0, 48, 0, 48)
OpenIcon.Position = UDim2.new(0, 20, 0.92, 0)
OpenIcon.Image = "rbxassetid://6031091002"
OpenIcon.BackgroundTransparency = 1
OpenIcon.Visible = false
OpenIcon.Parent = ScreenGui

-- VARIÁVEIS DE CONTROLE
local infJump = false
local godMode = false
local godModeLoop = nil
local noclipActive = false
local noclipConnection = nil
local speedActive = false
local normalSpeed = 16
local fastSpeed = 100
local clickTeleportActive = false
local clickTeleportConn

-- NOVAS VARIÁVEIS PARA STEAL A BRAINROT
local autoStealActive = false
local autoStealLoop = nil
local autoCollectActive = false
local autoCollectLoop = nil
local autoLockActive = false
local autoLockLoop = nil
local brainrotESPActive = false
local espConnections = {}

-- FUNÇÕES ORIGINAIS

-- Inf Jump
InfJumpButton.MouseButton1Click:Connect(function()
    infJump = not infJump
    InfJumpButton.Text = infJump and "Inf Jump: ON" or "Inf Jump: OFF"
end)

UserInputService.JumpRequest:Connect(function()
    if infJump then
        local char = player.Character or player.CharacterAdded:Wait()
        local hum = char:FindFirstChildOfClass("Humanoid")
        if hum then
            hum:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

-- GodMode
GodModeButton.MouseButton1Click:Connect(function()
    godMode = not godMode
    GodModeButton.Text = godMode and "Invencibilidade: ON" or "Invencibilidade: OFF"
    
    if godMode and not godModeLoop then
        godModeLoop = task.spawn(function()
            while godMode do
                local char = player.Character or player.CharacterAdded:Wait()
                local hum = char:FindFirstChildOfClass("Humanoid")
                if hum then
                    hum.Health = hum.MaxHealth
                    hum.MaxHealth = math.huge
                    hum.Health = math.huge
                    hum:SetStateEnabled(Enum.HumanoidStateType.Dead, false)
                end
                task.wait(0.05)
            end
            godModeLoop = nil
        end)
    elseif not godMode and godModeLoop then
        godModeLoop = nil
    end
end)

-- Teleporte (ajuste basePosition para sua base)
local basePosition = Vector3.new(100, 5, 200)
TeleportButton.MouseButton1Click:Connect(function()
    local char = player.Character or player.CharacterAdded:Wait()
    if char and char:FindFirstChild("HumanoidRootPart") then
        char.HumanoidRootPart.CFrame = CFrame.new(basePosition)
    end
end)

-- NoClip
local function onTouched(part)
    if not noclipActive then return end
    if not part:IsA("BasePart") then return end
    if not part.Anchored then return end
    if not part.CanCollide then return end
    
    local character = player.Character
    if not character then return end
    
    local hrp = character:FindFirstChild("HumanoidRootPart")
    if not hrp then return end
    
    if part.Position.Y < (hrp.Position.Y - hrp.Size.Y) then return end
    
    part.CanCollide = false
end

local function enableNoClip()
    if noclipConnection then
        noclipConnection:Disconnect()
    end
    local character = player.Character
    if not character then return end
    
    local hrp = character:FindFirstChild("HumanoidRootPart")
    if not hrp then return end
    
    noclipConnection = hrp.Touched:Connect(onTouched)
end

local function disableNoClip()
    if noclipConnection then
        noclipConnection:Disconnect()
        noclipConnection = nil
    end
end

NoClipButton.MouseButton1Click:Connect(function()
    noclipActive = not noclipActive
    NoClipButton.Text = noclipActive and "NoClip: ON" or "NoClip: OFF"
    
    disableNoClip()
    if noclipActive then
        enableNoClip()
    end
end)

player.CharacterAdded:Connect(function()
    if noclipActive then
        enableNoClip()
    end
end)

-- Velocidade
SpeedButton.MouseButton1Click:Connect(function()
    speedActive = not speedActive
    local char = player.Character or player.CharacterAdded:Wait()
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    
    if hum then
        if speedActive then
            hum.WalkSpeed = fastSpeed
            SpeedButton.Text = "Velocidade: RÁPIDA"
        else
            hum.WalkSpeed = normalSpeed
            SpeedButton.Text = "Velocidade: NORMAL"
        end
    end
end)

player.CharacterAdded:Connect(function(char)
    local hum = char:WaitForChild("Humanoid")
    hum.WalkSpeed = speedActive and fastSpeed or normalSpeed
end)

-- TP para outro jogador
TeleportToPlayerButton.MouseButton1Click:Connect(function()
    local targetUsername = TargetBox.Text
    if targetUsername ~= nil and targetUsername ~= "" then
        local targetPlayer = Players:FindFirstChild(targetUsername)
        local localChar = player.Character or player.CharacterAdded:Wait()
        
        if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") and localChar and localChar:FindFirstChild("HumanoidRootPart") then
            localChar.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
        else
            TargetBox.Text = "Jogador não encontrado!"
            wait(1.2)
            TargetBox.Text = ""
        end
    end
end)

-- Click Teleport
if _G.WRDClickTeleport == nil then
    _G.WRDClickTeleport = false
end

local function setClickTeleport(state)
    clickTeleportActive = state
    ClickTeleportButton.Text = clickTeleportActive and "Click Teleport: ON (CTRL+Clique)" or "Click Teleport: OFF (CTRL+Clique)"
    _G.WRDClickTeleport = clickTeleportActive
    
    if clickTeleportActive then
        if clickTeleportConn then
            clickTeleportConn:Disconnect()
        end
        local mouse = player:GetMouse()
        clickTeleportConn = UserInputService.InputBegan:Connect(function(input, gameProcessed)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                if _G.WRDClickTeleport and UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then
                    local char = player.Character or player.CharacterAdded:Wait()
                    if char then
                        local hrp = char:FindFirstChild("HumanoidRootPart") or char:FindFirstChild("Torso")
                        if hrp then
                            char:MoveTo(Vector3.new(mouse.Hit.x, mouse.Hit.y, mouse.Hit.z))
                        end
                    end
                end
            end
        end)
        game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Click teleport ativado (CTRL+Clique)"; Duration=5;})
    else
        if clickTeleportConn then
            clickTeleportConn:Disconnect()
            clickTeleportConn = nil
        end
        game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Click teleport desativado"; Duration=5;})
    end
end

ClickTeleportButton.MouseButton1Click:Connect(function()
    setClickTeleport(not clickTeleportActive)
end)

-- NOVAS FUNÇÕES PARA STEAL A BRAINROT

-- Auto Steal Brainrot
local function autoStealBrainrot()
    autoStealLoop = task.spawn(function()
        while autoStealActive do
            for _, targetPlayer in pairs(Players:GetPlayers()) do
                if targetPlayer ~= player and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    local distance = (player.Character.HumanoidRootPart.Position - targetPlayer.Character.HumanoidRootPart.Position).Magnitude
                    
                    if distance < 30 then
                        -- Tenta diferentes métodos de roubo
                        pcall(function()
                            -- Método 1: ClickDetector
                            if targetPlayer.Character:FindFirstChild("ClickDetector") then
                                fireclickdetector(targetPlayer.Character.ClickDetector)
                            end
                            
                            -- Método 2: Parte específica com ClickDetector
                            for _, part in pairs(targetPlayer.Character:GetChildren()) do
                                if part:FindFirstChild("ClickDetector") then
                                    fireclickdetector(part.ClickDetector)
                                end
                            end
                            
                            -- Método 3: RemoteEvent (ajustar nome conforme necessário)
                            if ReplicatedStorage:FindFirstChild("StealBrainrot") then
                                ReplicatedStorage.StealBrainrot:FireServer(targetPlayer)
                            elseif ReplicatedStorage:FindFirstChild("RemoteEvents") and ReplicatedStorage.RemoteEvents:FindFirstChild("Steal") then
                                ReplicatedStorage.RemoteEvents.Steal:FireServer(targetPlayer)
                            end
                        end)
                    end
                end
            end
            task.wait(0.5)
        end
    end)
end

AutoStealButton.MouseButton1Click:Connect(function()
    autoStealActive = not autoStealActive
    AutoStealButton.Text = autoStealActive and "Auto Steal Brainrot: ON" or "Auto Steal Brainrot: OFF"
    
    if autoStealActive then
        autoStealBrainrot()
        game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Auto Steal ativado!"; Duration=3;})
    else
        if autoStealLoop then
            task.cancel(autoStealLoop)
            autoStealLoop = nil
        end
        game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Auto Steal desativado!"; Duration=3;})
    end
end)

-- Auto Collect Cash
local function autoCollectCash()
    autoCollectLoop = task.spawn(function()
        while autoCollectActive do
            pcall(function()
                -- Procura por diferentes nomes de itens coletáveis
                local itemNames = {"Cash", "Coin", "Money", "Brainrot", "Item", "Collectible"}
                
                for _, itemName in pairs(itemNames) do
                    if workspace:FindFirstChild(itemName) then
                        for _, item in pairs(workspace[itemName]:GetChildren()) do
                            if item:FindFirstChild("ClickDetector") then
                                fireclickdetector(item.ClickDetector)
                            end
                        end
                    end
                end
                
                -- Procura diretamente no workspace
                for _, item in pairs(workspace:GetChildren()) do
                    if table.find(itemNames, item.Name) and item:FindFirstChild("ClickDetector") then
                        fireclickdetector(item.ClickDetector)
                    end
                end
            end)
            task.wait(0.1)
        end
    end)
end

AutoCollectButton.MouseButton1Click:Connect(function()
    autoCollectActive = not autoCollectActive
    AutoCollectButton.Text = autoCollectActive and "Auto Collect Cash: ON" or "Auto Collect Cash: OFF"
    
    if autoCollectActive then
        autoCollectCash()
        game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Auto Collect ativado!"; Duration=3;})
    else
        if autoCollectLoop then
            task.cancel(autoCollectLoop)
            autoCollectLoop = nil
        end
        game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Auto Collect desativado!"; Duration=3;})
    end
end)

-- Auto Lock Target
local function autoLock()
    autoLockLoop = task.spawn(function()
        while autoLockActive do
            local nearestPlayer = nil
            local nearestDistance = math.huge
            
            for _, targetPlayer in pairs(Players:GetPlayers()) do
                if targetPlayer ~= player and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    local distance = (player.Character.HumanoidRootPart.Position - targetPlayer.Character.HumanoidRootPart.Position).Magnitude
                    if distance < nearestDistance and distance < 50 then
                        nearestDistance = distance
                        nearestPlayer = targetPlayer
                    end
                end
            end
            
            if nearestPlayer and nearestPlayer.Character and player.Character then
                local targetPos = nearestPlayer.Character.HumanoidRootPart.Position
                local myPos = player.Character.HumanoidRootPart.Position
                local lookDirection = (targetPos - myPos).Unit
                
                player.Character.HumanoidRootPart.CFrame = CFrame.lookAt(myPos, myPos + lookDirection)
            end
            
            task.wait(0.1)
        end
    end)
end

AutoLockButton.MouseButton1Click:Connect(function()
    autoLockActive = not autoLockActive
    AutoLockButton.Text = autoLockActive and "Auto Lock Target: ON" or "Auto Lock Target: OFF"
    
    if autoLockActive then
        autoLock()
        game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Auto Lock ativado!"; Duration=3;})
    else
        if autoLockLoop then
            task.cancel(autoLockLoop)
            autoLockLoop = nil
        end
        game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Auto Lock desativado!"; Duration=3;})
    end
end)

-- Brainrot ESP
local function createESP(targetPlayer)
    if not targetPlayer.Character then return end
    
    local highlight = Instance.new("Highlight")
    highlight.Name = "BrainrotESP"
    highlight.Parent = targetPlayer.Character
    highlight.FillColor = Color3.fromRGB(255, 100, 255)
    highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
    highlight.FillTransparency = 0.5
    highlight.OutlineTransparency = 0
    
    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Name = "BrainrotNameESP"
    billboardGui.Parent = targetPlayer.Character:FindFirstChild("Head")
    billboardGui.Size = UDim2.new(0, 200, 0, 50)
    billboardGui.StudsOffset = Vector3.new(0, 2, 0)
    
    local nameLabel = Instance.new("TextLabel")
    nameLabel.Parent = billboardGui
    nameLabel.Size = UDim2.new(1, 0, 1, 0)
    nameLabel.BackgroundTransparency = 1
    nameLabel.Text = targetPlayer.Name
    nameLabel.TextColor3 = Color3.fromRGB(255, 100, 255)
    nameLabel.TextStrokeTransparency = 0
    nameLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
    nameLabel.Font = Enum.Font.GothamBold
    nameLabel.TextSize = 16
    
    espConnections[targetPlayer] = targetPlayer.CharacterRemoving:Connect(function()
        if highlight then highlight:Destroy() end
        if billboardGui then billboardGui:Destroy() end
    end)
end

local function toggleBrainrotESP()
    if brainrotESPActive then
        -- Ativar ESP
        for _, targetPlayer in pairs(Players:GetPlayers()) do
            if targetPlayer ~= player and targetPlayer.Character then
                createESP(targetPlayer)
            end
        end
        
        -- ESP para novos jogadores
        Players.PlayerAdded:Connect(function(newPlayer)
            if brainrotESPActive then
                newPlayer.CharacterAdded:Connect(function()
                    if brainrotESPActive then
                        createESP(newPlayer)
                    end
                end)
            end
        end)
    else
        -- Desativar ESP
        for _, targetPlayer in pairs(Players:GetPlayers()) do
            if targetPlayer.Character then
                local highlight = targetPlayer.Character:FindFirstChild("BrainrotESP")
                local billboard = targetPlayer.Character:FindFirstChild("Head") and targetPlayer.Character.Head:FindFirstChild("BrainrotNameESP")
                
                if highlight then highlight:Destroy() end
                if billboard then billboard:Destroy() end
            end
            
            if espConnections[targetPlayer] then
                espConnections[targetPlayer]:Disconnect()
                espConnections[targetPlayer] = nil
            end
        end
    end
end

BrainrotESPButton.MouseButton1Click:Connect(function()
    brainrotESPActive = not brainrotESPActive
    BrainrotESPButton.Text = brainrotESPActive and "Brainrot ESP: ON" or "Brainrot ESP: OFF"
    
    toggleBrainrotESP()
    
    local status = brainrotESPActive and "ativado" or "desativado"
    game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Brainrot ESP " .. status .. "!"; Duration=3;})
end)

-- TP para Jogador com Brainrot
TpBrainrotButton.MouseButton1Click:Connect(function()
    local bestTarget = nil
    local highestBrainrot = 0
    
    for _, targetPlayer in pairs(Players:GetPlayers()) do
        if targetPlayer ~= player and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
            -- Verifica leaderstats
            if targetPlayer:FindFirstChild("leaderstats") then
                for _, stat in pairs(targetPlayer.leaderstats:GetChildren()) do
                    if stat.Name:lower():find("brainrot") or stat.Name:lower():find("brain") then
                        if stat.Value > highestBrainrot then
                            highestBrainrot = stat.Value
                            bestTarget = targetPlayer
                        end
                    end
                end
            end
        end
    end
    
    if bestTarget then
        player.Character.HumanoidRootPart.CFrame = bestTarget.Character.HumanoidRootPart.CFrame
        game.StarterGui:SetCore("SendNotification", {
            Title="KamuiHub"; 
            Text="Teleportado para " .. bestTarget.Name .. " (" .. highestBrainrot .. " brainrots)"; 
            Duration=4;
        })
    else
        game.StarterGui:SetCore("SendNotification", {
            Title="KamuiHub"; 
            Text="Nenhum jogador com brainrot encontrado!"; 
            Duration=3;
        })
    end
end)

-- Minimizar e reabrir GUI
MinButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    OpenIcon.Visible = true
end)

OpenIcon.MouseButton1Click:Connect(function()
    MainFrame.Visible = true
    OpenIcon.Visible = false
end)

CloseButton.MouseButton1Click:Connect(function()
    -- Limpa todas as conexões antes de fechar
    if autoStealLoop then task.cancel(autoStealLoop) end
    if autoCollectLoop then task.cancel(autoCollectLoop) end
    if autoLockLoop then task.cancel(autoLockLoop) end
    if godModeLoop then task.cancel(godModeLoop) end
    if noclipConnection then noclipConnection:Disconnect() end
    if clickTeleportConn then clickTeleportConn:Disconnect() end
    
    -- Remove ESP de todos os jogadores
    for _, targetPlayer in pairs(Players:GetPlayers()) do
        if targetPlayer.Character then
            local highlight = targetPlayer.Character:FindFirstChild("BrainrotESP")
            local billboard = targetPlayer.Character:FindFirstChild("Head") and targetPlayer.Character.Head:FindFirstChild("BrainrotNameESP")
            if highlight then highlight:Destroy() end
            if billboard then billboard:Destroy() end
        end
        if espConnections[targetPlayer] then
            espConnections[targetPlayer]:Disconnect()
        end
    end
    
    MainFrame:Destroy()
    game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Hub fechado com segurança!"; Duration=3;})
end)

-------------------
-- ANTI-AFK SCRIPT
-------------------
wait(0.5)
local antiAFKGui = Instance.new("ScreenGui")
antiAFKGui.Name = "AntiAFKGui"
antiAFKGui.Parent = game:GetService("CoreGui")
antiAFKGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local mainLabel = Instance.new("TextLabel")
mainLabel.Parent = antiAFKGui
mainLabel.Active = true
mainLabel.BackgroundColor3 = Color3.fromRGB(0, 176, 176)
mainLabel.Draggable = true
mainLabel.Position = UDim2.new(0, 500, 0, 100)
mainLabel.Size = UDim2.new(0, 370, 0, 52)
mainLabel.Font = Enum.Font.SourceSansSemibold
mainLabel.Text = "Script Anti AFK"
mainLabel.TextColor3 = Color3.fromRGB(0, 255, 255)
mainLabel.TextSize = 22

local statusFrame = Instance.new("Frame")
statusFrame.Parent = mainLabel
statusFrame.BackgroundColor3 = Color3.fromRGB(0, 196, 196)
statusFrame.Position = UDim2.new(0, 0, 1, 2)
statusFrame.Size = UDim2.new(0, 370, 0, 107)

local autorLabel = Instance.new("TextLabel")
autorLabel.Parent = statusFrame
autorLabel.BackgroundColor3 = Color3.fromRGB(0, 176, 176)
autorLabel.Position = UDim2.new(0, 0, 0, 8)
autorLabel.Size = UDim2.new(0, 370, 0, 21)
autorLabel.Font = Enum.Font.Arial
autorLabel.Text = "Derrapou por Mattie/Parasita"
autorLabel.TextColor3 = Color3.fromRGB(0, 255, 255)
autorLabel.TextSize = 20

local statusLabel = Instance.new("TextLabel")
statusLabel.Parent = statusFrame
statusLabel.BackgroundColor3 = Color3.fromRGB(0, 176, 176)
statusLabel.Position = UDim2.new(0, 0, 0, 40)
statusLabel.Size = UDim2.new(0, 370, 0, 44)
statusLabel.Font = Enum.Font.ArialBold
statusLabel.Text = "Status: Ativo"
statusLabel.TextColor3 = Color3.fromRGB(0, 255, 255)
statusLabel.TextSize = 20

local VirtualUser = game:GetService("VirtualUser")
Players.LocalPlayer.Idled:Connect(function()
    VirtualUser:CaptureController()
    VirtualUser:ClickButton2(Vector2.new())
    statusLabel.Text = "Você foi salvo do kick AFK! KamuiHub funcionando"
    wait(2)
    statusLabel.Text = "Status: Ativo"
end)

-- NOTIFICAÇÃO DE CARREGAMENTO COMPLETO
game.StarterGui:SetCore("SendNotification", {
    Title="🧠 KamuiHub - Steal a Brainrot"; 
    Text="Carregado com sucesso! Todas as funções ativas."; 
    Duration=6;
})

print("🚀 KamuiHub para Steal a Brainrot carregado!")
print("📋 Funções disponíveis:")
print("   • Auto Steal Brainrot")
print("   • Auto Collect Cash") 
print("   • Auto Lock Target")
print("   • Brainrot ESP")
print("   • TP para Player c/ Brainrot")
print("   • + Todas as funções originais")
print("💡 Dica: Teste cada função individualmente primeiro!")
