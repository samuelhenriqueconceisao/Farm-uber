-- CLOUD UBER
-- Interface roxa/branca animada com Farm uber + Renomear carro


local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")


local LocalPlayer = Players.LocalPlayer


-- GUI principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "CLOUD_UBER_GUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = game.CoreGui


-- Main frame
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 420, 0, 220)
mainFrame.Position = UDim2.new(0.5, -210, 0.5, -110)
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.BackgroundTransparency = 1
mainFrame.Parent = screenGui


-- Faixas de fundo animadas
local stripe1 = Instance.new("Frame", mainFrame)
stripe1.Size = UDim2.new(1.5, 0, 1.2, 0)
stripe1.Position = UDim2.new(-0.5, 0, -0.1, 0)
stripe1.BackgroundColor3 = Color3.fromRGB(115, 45, 190)


local stripe2 = Instance.new("Frame", mainFrame)
stripe2.Size = UDim2.new(1.5, 0, 1.2, 0)
stripe2.Position = UDim2.new(0.6, 0, -0.1, 0)
stripe2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
stripe2.BackgroundTransparency = 0.05


local stripe3 = Instance.new("Frame", mainFrame)
stripe3.Size = UDim2.new(1.8, 0, 1.4, 0)
stripe3.Position = UDim2.new(-0.9, 0, -0.2, 0)
stripe3.BackgroundColor3 = Color3.fromRGB(175, 120, 230)
stripe3.BackgroundTransparency = 0.25


-- Painel principal
local overlay = Instance.new("Frame", mainFrame)
overlay.Size = UDim2.new(1, 0, 1, 0)
overlay.BackgroundColor3 = Color3.fromRGB(18, 18, 20)
overlay.BackgroundTransparency = 0.25
overlay.BorderSizePixel = 0
Instance.new("UICorner", overlay).CornerRadius = UDim.new(0, 12)


-- TÃ­tulo
local titleBar = Instance.new("Frame", overlay)
titleBar.Size = UDim2.new(1, 0, 0, 42)
titleBar.BackgroundTransparency = 1


local titleLabel = Instance.new("TextLabel", titleBar)
titleLabel.Size = UDim2.new(1, -12, 1, 0)
titleLabel.Position = UDim2.new(0, 8, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "CLOUD UBER"
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextSize = 20
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextXAlignment = Enum.TextXAlignment.Left


local closeBtn = Instance.new("TextButton", titleBar)
closeBtn.Size = UDim2.new(0, 34, 0, 34)
closeBtn.Position = UDim2.new(1, -44, 0.5, -17)
closeBtn.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
closeBtn.Text = "â¨¯"
closeBtn.TextColor3 = Color3.fromRGB(255,255,255)
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 18
closeBtn.BorderSizePixel = 0
Instance.new("UICorner", closeBtn).CornerRadius = UDim.new(1, 0)


-- Ãrea de botÃµes
local content = Instance.new("Frame", overlay)
content.Size = UDim2.new(1, -24, 1, -62)
content.Position = UDim2.new(0, 12, 0, 50)
content.BackgroundTransparency = 1


local function createButton(name, text, y)
    local btn = Instance.new("TextButton", content)
    btn.Name = name
    btn.Size = UDim2.new(1, 0, 0, 42)
    btn.Position = UDim2.new(0, 0, 0, y)
    btn.BackgroundColor3 = Color3.fromRGB(0,0,0)
    btn.BackgroundTransparency = 0.12
    btn.Text = text
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 16
    btn.BorderSizePixel = 0
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)
    return btn
end


local farmBtn = createButton("FarmUberBtn", "Farm uber (OFF)", 0)
local renameBtn = createButton("RenameCarBtn", "Renomear carro", 52)


-- Sistema arrastar
local dragging, dragInput, mousePos, framePos
mainFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        mousePos = input.Position
        framePos = mainFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)
mainFrame.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)
RunService.RenderStepped:Connect(function()
    if dragging and dragInput then
        local delta = UserInputService:GetMouseLocation() - mousePos
        mainFrame.Position = UDim2.new(framePos.X.Scale, framePos.X.Offset + delta.X, framePos.Y.Scale, framePos.Y.Offset + delta.Y)
    end
end)


-- Fundo animado
task.spawn(function()
    local dir = 1
    while screenGui.Parent do
        TweenService:Create(stripe1, TweenInfo.new(6, Enum.EasingStyle.Linear), {Position = UDim2.new(-0.8 * dir, 0, -0.1, 0)}):Play()
        TweenService:Create(stripe2, TweenInfo.new(8, Enum.EasingStyle.Linear), {Position = UDim2.new(0.9 * dir, 0, -0.1, 0)}):Play()
        TweenService:Create(stripe3, TweenInfo.new(10, Enum.EasingStyle.Linear), {Position = UDim2.new(-1.2 * dir, 0, -0.2, 0)}):Play()
        dir *= -1
        task.wait(5)
    end
end)


-- FunÃ§Ã£o Renomear carro (seu cÃ³digo)
local function renameNearestCar()
    local player = Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    local hrp = character:WaitForChild("HumanoidRootPart")


    local carros = workspace.CarrosSpawnados:GetChildren()
    local carroPerto, menorDist = nil, math.huge


    for _, c in pairs(carros) do
        if c:IsA("Model") then
            local ref = c:FindFirstChild("DriveSeat") or c:FindFirstChildWhichIsA("BasePart")
            if ref then
                local dist = (ref.Position - hrp.Position).Magnitude
                if dist < menorDist then
                    menorDist = dist
                    carroPerto = c
                end
            end
        end
    end


    if carroPerto then
        carroPerto.Name = "ok"
        print("Carro perto renomeado pra ok")
    else
        warn("NÃ£o achei carro")
    end
end


-- FunÃ§Ã£o Farm uber (seu cÃ³digo)
local farmRunning = false
local function farmUber()
    while farmRunning do
        local carro = workspace.CarrosSpawnados:FindFirstChild("ok")
        local destino = workspace:FindFirstChild("LocalMarcado")


        if carro and destino and carro:FindFirstChild("DriveSeat") then
            if not carro.PrimaryPart then
                carro.PrimaryPart = carro:FindFirstChild("DriveSeat")
            end
            carro:SetPrimaryPartCFrame(destino.CFrame)
        else
            warn("Carro, destino ou DriveSeat nÃ£o encontrados.")
        end
        task.wait(1)
    end
end


-- Eventos dos botÃµes
farmBtn.MouseButton1Click:Connect(function()
    farmRunning = not farmRunning
    farmBtn.Text = "Farm uber (" .. (farmRunning and "ON" or "OFF") .. ")"
    if farmRunning then
        task.spawn(farmUber)
    end
end)


renameBtn.MouseButton1Click:Connect(renameNearestCar)
closeBtn.MouseButton1Click:Connect(function() screenGui:Destroy() end)
