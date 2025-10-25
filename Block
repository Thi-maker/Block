local Players = game:GetService("Players")
local player = Players.LocalPlayer

local screenGui
local blockButton
local BlockEvent
local isBlocking = false -- estado do bloqueio

-- Criar botão
local function createButton()
    if screenGui then
        screenGui:Destroy()
    end

    screenGui = Instance.new("ScreenGui")
    screenGui.Name = "BlockGUI"
    screenGui.Parent = player:WaitForChild("PlayerGui")

    blockButton = Instance.new("TextButton")
    blockButton.Size = UDim2.new(0, 180, 0, 70)
    blockButton.Position = UDim2.new(0.55, -5, 0.0, 0)
    blockButton.BackgroundColor3 = isBlocking and Color3.fromRGB(0,170,0) or Color3.fromRGB(60,60,60)
    blockButton.TextColor3 = Color3.fromRGB(255,255,255)
    blockButton.Text = "🛡️ BLOQUEAR"
    blockButton.TextScaled = true
    blockButton.AutoButtonColor = false
    blockButton.Parent = screenGui
    blockButton.Draggable = false

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0,20)
    corner.Parent = blockButton

    -- Alterna o bloqueio ao clicar
    blockButton.MouseButton1Click:Connect(function()
        if not BlockEvent then return end
        isBlocking = not isBlocking

        if isBlocking then
            BlockEvent:FireServer(true)
            blockButton.BackgroundColor3 = Color3.fromRGB(0,170,0)
        else
            BlockEvent:FireServer(false)
            blockButton.BackgroundColor3 = Color3.fromRGB(60,60,60)
        end
    end)
end

-- Atualiza BlockEvent do Character
local function updateCharacter(char)
    BlockEvent = char:WaitForChild("BlockScript"):WaitForChild("BlockEvent")
    local humanoid = char:WaitForChild("Humanoid")

    -- Quando morrer → resetar GUI e bloqueio
    humanoid.Died:Connect(function()
        if screenGui then
            screenGui:Destroy()
        end
        isBlocking = false
        if BlockEvent then
            BlockEvent:FireServer(false)
        end
    end)

    -- Recria botão no Character novo
    createButton()
end

-- Sempre que spawnar
player.CharacterAdded:Connect(function(char)
    task.wait(0.5)
    updateCharacter(char)
end)

-- Cria botão no lobby
createButton()# Block
