-- Kamuu - GUI com Inf Jump, GodMode, Teleporte e NoClip Otimizado

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer

-- GUI Principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "Kamuu"
ScreenGui.Parent = player:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

-- Barra superior
local TopBar = Instance.new("Frame")
TopBar.Size = UDim2.new(0, 300, 0, 30)
TopBar.Position = UDim2.new(0.5, -150, 0.1, 0)
TopBar.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
TopBar.Parent = ScreenGui

-- Título
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 1, 0)
Title.BackgroundTransparency = 1
Title.Text = "Kamuu"
Title.Font = Enum.Font.SourceSansBold
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 20
Title.Parent = TopBar

-- Botão minimizar
local MinButton = Instance.new("TextButton")
MinButton.Size = UDim2.new(0, 30, 1, 0)
MinButton.Position = UDim2.new(1, -60, 0, 0)
MinButton.Text = "-"
MinButton.Font = Enum.Font.SourceSansBold
MinButton.TextSize = 20
MinButton.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
MinButton.TextColor3 = Color3.new(1, 1, 1)
MinButton.Parent = TopBar

-- Botão fechar
local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0, 30, 1, 0)
CloseButton.Position = UDim2.new(1, -30, 0, 0)
CloseButton.Text = "X"
CloseButton.Font = Enum.Font.SourceSansBold
CloseButton.TextSize = 20
CloseButton.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
CloseButton.TextColor3 = Color3.new(1, 1, 1)
CloseButton.Parent = TopBar

-- Botão Inf Jump
local InfJumpButton = Instance.new("TextButton")
InfJumpButton.Size = UDim2.new(0, 280, 0, 40)
InfJumpButton.Position = UDim2.new(0.5, -140, 0.1, 40)
InfJumpButton.Text = "Inf Jump: OFF"
InfJumpButton.Font = Enum.Font.SourceSans
InfJumpButton.TextSize = 18
InfJumpButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
InfJumpButton.TextColor3 = Color3.new(1, 1, 1)
InfJumpButton.Parent = ScreenGui

-- Botão GodMode
local GodModeButton = Instance.new("TextButton")
GodModeButton.Size = UDim2.new(0, 280, 0, 40)
GodModeButton.Position = UDim2.new(0.5, -140, 0.1, 90)
GodModeButton.Text = "Invencibilidade: OFF"
GodModeButton.Font = Enum.Font.SourceSans
GodModeButton.TextSize = 18
GodModeButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
GodModeButton.TextColor3 = Color3.new(1, 1, 1)
GodModeButton.Parent = ScreenGui

-- Botão Teleporte
local TeleportButton = Instance.new("TextButton")
TeleportButton.Size = UDim2.new(0, 280, 0, 40)
TeleportButton.Position = UDim2.new(0.5, -140, 0.1, 140)
TeleportButton.Text = "Teleporte para Base"
TeleportButton.Font = Enum.Font.SourceSans
TeleportButton.TextSize = 18
TeleportButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
TeleportButton.TextColor3 = Color3.new(1, 1, 1)
TeleportButton.Parent = ScreenGui

-- Botão NoClip
local NoClipButton = Instance.new("TextButton")
NoClipButton.Size = UDim2.new(0, 280, 0, 40)
NoClipButton.Position = UDim2.new(0.5, -140, 0.1, 190)
NoClipButton.Text = "NoClip: OFF"
NoClipButton.Font = Enum.Font.SourceSans
NoClipButton.TextSize = 18
NoClipButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
NoClipButton.TextColor3 = Color3.new(1, 1, 1)
NoClipButton.Parent = ScreenGui

-- Ícone reabrir
local OpenIcon = Instance.new("ImageButton")
OpenIcon.Size = UDim2.new(0, 50, 0, 50)
OpenIcon.Position = UDim2.new(0, 10, 0.9, 0)
OpenIcon.Image = "rbxassetid://3926305904"
OpenIcon.ImageRectOffset = Vector2.new(4, 4)
OpenIcon.ImageRectSize = Vector2.new(36, 36)
OpenIcon.Visible = false
OpenIcon.BackgroundTransparency = 1
OpenIcon.Parent = ScreenGui

-- Variáveis de controle
local infJump = false
local godMode = false
local godModeLoop = nil
local noclipActive = false
local noclipConnection = nil

-- Função Inf Jump
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

-- Função GodMode
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

-- Função Teleporte (troque o Vector3 pela posição real da sua base!)
local basePosition = Vector3.new(100, 5, 200) -- Troque aqui!
TeleportButton.MouseButton1Click:Connect(function()
    local char = player.Character or player.CharacterAdded:Wait()
    if char and char:FindFirstChild("HumanoidRootPart") then
        char.HumanoidRootPart.CFrame = CFrame.new(basePosition)
    end
end)

-- Função NoClip otimizada
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

-- Minimizar e reabrir GUI
MinButton.MouseButton1Click:Connect(function()
    TopBar.Visible = false
    InfJumpButton.Visible = false
    GodModeButton.Visible = false
    TeleportButton.Visible = false
    NoClipButton.Visible = false
    OpenIcon.Visible = true
end)
OpenIcon.MouseButton1Click:Connect(function()
    TopBar.Visible = true
    InfJumpButton.Visible = true
    GodModeButton.Visible = true
    TeleportButton.Visible = true
    NoClipButton.Visible = true
    OpenIcon.Visible = false
end)

-- Fechar GUI
CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)
