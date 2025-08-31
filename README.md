-- KamuiHub - GUI arrastável e com rolagem para mostrar todas as funções.
-- Inclui: Inf Jump, GodMode, Teleporte, NoClip, Velocidade, Teleporte para Jogador, Click Teleport, Anti-AFK.

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer

-- Frame principal (arrastável)
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 360, 0, 440)
MainFrame.Position = UDim2.new(0.5, -180, 0.12, 0)
MainFrame.BackgroundTransparency = 1
MainFrame.Parent = player:WaitForChild("PlayerGui")
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
TopBar.Active = false -- só o MainFrame é arrastável

-- Título
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -70, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "KamuiHub"
Title.Font = Enum.Font.GothamBold
Title.TextColor3 = Color3.fromRGB(230, 230, 255)
Title.TextStrokeTransparency = 0.7
Title.TextSize = 24
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
MinButton.MouseEnter:Connect(function() MinButton.BackgroundColor3 = Color3.fromRGB(110,110,160) end)
MinButton.MouseLeave:Connect(function() MinButton.BackgroundColor3 = Color3.fromRGB(85,85,120) end)

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
CloseButton.MouseEnter:Connect(function() CloseButton.BackgroundColor3 = Color3.fromRGB(190,70,90) end)
CloseButton.MouseLeave:Connect(function() CloseButton.BackgroundColor3 = Color3.fromRGB(140,40,60) end)

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
BtnScroll.CanvasSize = UDim2.new(0, 0, 0, 500) -- aumenta se adicionar mais funções
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
    btn.TextSize = 18
    btn.BackgroundColor3 = Color3.fromRGB(60, 85, 135)
    btn.TextColor3 = Color3.fromRGB(240, 240, 255)
    btn.AutoButtonColor = false
    btn.Parent = BtnScroll
    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 10)
    btn.MouseEnter:Connect(function() btn.BackgroundColor3 = Color3.fromRGB(100,130,200) end)
    btn.MouseLeave:Connect(function() btn.BackgroundColor3 = Color3.fromRGB(60,85,135) end)
    return btn
end

-- Botões e inputs
local posY = 10
local InfJumpButton = createButton("Inf Jump: OFF", posY); posY = posY + 48
local GodModeButton = createButton("Invencibilidade: OFF", posY); posY = posY + 48
local TeleportButton = createButton("Teleporte para Base", posY); posY = posY + 48
local NoClipButton = createButton("NoClip: OFF", posY); posY = posY + 48
local SpeedButton = createButton("Velocidade: NORMAL", posY); posY = posY + 48
local TeleportToPlayerButton = createButton("Teleportar para Jogador", posY); posY = posY + 48
local ClickTeleportButton = createButton("Click Teleport: OFF (CTRL+Clique)", posY); posY = posY + 48

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
OpenIcon.Parent = player:WaitForChild("PlayerGui")

-- Funções dos botões
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
    if noclipConnection then noclipConnection:Disconnect() end
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
    if noclipActive then enableNoClip() end
end)
player.CharacterAdded:Connect(function()
    if noclipActive then enableNoClip() end
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

-- Click Teleport (Ctrl + Clique Esquerdo)
if _G.WRDClickTeleport == nil then
    _G.WRDClickTeleport = false
end
local function setClickTeleport(state)
    clickTeleportActive = state
    ClickTeleportButton.Text = clickTeleportActive and "Click Teleport: ON (CTRL+Clique)" or "Click Teleport: OFF (CTRL+Clique)"
    _G.WRDClickTeleport = clickTeleportActive
    if clickTeleportActive then
        if clickTeleportConn then clickTeleportConn:Disconnect() end
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
        if clickTeleportConn then clickTeleportConn:Disconnect() clickTeleportConn = nil end
        game.StarterGui:SetCore("SendNotification", {Title="KamuiHub"; Text="Click teleport desativado"; Duration=5;})
    end
end
ClickTeleportButton.MouseButton1Click:Connect(function()
    setClickTeleport(not clickTeleportActive)
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
    MainFrame:Destroy()
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
    statusLabel.Text = "Você foi salvo do kick AFK! Discord: wearedevs"
    wait(2)
    statusLabel.Text = "Status: Ativo"
end)
