local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer

-- Criando GUI principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "YZHUB"
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Frame principal
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 250, 0, 360)
mainFrame.Position = UDim2.new(0, 20, 0, 20)
mainFrame.BackgroundColor3 = Color3.fromRGB(120, 120, 120)
mainFrame.BackgroundTransparency = 0.7
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui
mainFrame.Active = true

-- Drag support (touch and mouse)
local dragging = false
local dragInput, dragStart, startPos

local function updatePosition(input)
    local delta = input.Position - dragStart
    mainFrame.Position = UDim2.new(
        startPos.X.Scale,
        startPos.X.Offset + delta.X,
        startPos.Y.Scale,
        startPos.Y.Offset + delta.Y
    )
end

-- Título
local titleBar = Instance.new("TextLabel")
titleBar.Size = UDim2.new(1, 0, 0, 30)
titleBar.BackgroundColor3 = Color3.fromRGB(90, 90, 90)
titleBar.BackgroundTransparency = 0
titleBar.Text = "YZHUB"
titleBar.TextColor3 = Color3.new(1, 1, 1)
titleBar.TextScaled = true
titleBar.Font = Enum.Font.SourceSansBold
titleBar.Parent = mainFrame
titleBar.Active = true

-- Drag handlers (funciona pra mouse e toque)
titleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

titleBar.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and input == dragInput then
        updatePosition(input)
    end
end)

-- Botões + e - para redimensionar
local increaseBtn = Instance.new("TextButton")
increaseBtn.Size = UDim2.new(0, 30, 0, 25)
increaseBtn.Position = UDim2.new(1, -70, 0, 2)
increaseBtn.Text = "+"
increaseBtn.Font = Enum.Font.SourceSansBold
increaseBtn.TextScaled = true
increaseBtn.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
increaseBtn.TextColor3 = Color3.new(1, 1, 1)
increaseBtn.Parent = mainFrame

local decreaseBtn = Instance.new("TextButton")
decreaseBtn.Size = UDim2.new(0, 30, 0, 25)
decreaseBtn.Position = UDim2.new(1, -35, 0, 2)
decreaseBtn.Text = "-"
decreaseBtn.Font = Enum.Font.SourceSansBold
decreaseBtn.TextScaled = true
decreaseBtn.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
decreaseBtn.TextColor3 = Color3.new(1, 1, 1)
decreaseBtn.Parent = mainFrame

local function resizeFrame(amount)
    local newWidth = math.clamp(mainFrame.Size.X.Offset + amount, 200, 500)
    local newHeight = math.clamp(mainFrame.Size.Y.Offset + amount * 1.5, 200, 600)
    mainFrame.Size = UDim2.new(0, newWidth, 0, newHeight)
end

increaseBtn.MouseButton1Click:Connect(function()
    resizeFrame(20)
end)

decreaseBtn.MouseButton1Click:Connect(function()
    resizeFrame(-20)
end)

-- Criar botão com checkbox
local function createHackOption(name, posY)
    local container = Instance.new("Frame")
    container.Size = UDim2.new(1, -20, 0, 40)
    container.Position = UDim2.new(0, 10, 0, posY)
    container.BackgroundTransparency = 1
    container.Parent = mainFrame

    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0, 170, 1, 0)
    button.Position = UDim2.new(0, 0, 0, 0)
    button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    button.BackgroundTransparency = 0.5
    button.TextColor3 = Color3.new(1, 1, 1)
    button.TextScaled = true
    button.Text = "Ativar " .. name
    button.Font = Enum.Font.SourceSansBold
    button.Parent = container

    local checkbox = Instance.new("ImageButton")
    checkbox.Size = UDim2.new(0, 30, 0, 30)
    checkbox.Position = UDim2.new(1, -35, 0, 5)
    checkbox.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    checkbox.BackgroundTransparency = 0.5
    checkbox.AutoButtonColor = false
    checkbox.Image = "rbxassetid://3926305904" -- checkbox vazio
    checkbox.Parent = container

    local enabled = false

    local function updateVisual()
        if enabled then
            checkbox.Image = "rbxassetid://3926307971" -- checkbox marcado
            button.Text = "Desativar " .. name
            button.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
        else
            checkbox.Image = "rbxassetid://3926305904"
            button.Text = "Ativar " .. name
            button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
        end
    end

    button.MouseButton1Click:Connect(function()
        enabled = not enabled
        updateVisual()
        toggleHacks[name] = enabled
    end)

    checkbox.MouseButton1Click:Connect(function()
        enabled = not enabled
        updateVisual()
        toggleHacks[name] = enabled
    end)

    updateVisual()
    return function() return enabled end
end

local toggleHacks = {}

local hackOptions = {
    "Aimbot",
    "Teleport Kill",
    "Wallhack (Noclip)",
    "Speed Hack",
    "Fly Hack",
    "ESP",
    "Silent Aim"
}

local toggles = {}

for i, hackName in ipairs(hackOptions) do
    toggleHacks[hackName] = false
    toggles[hackName] = createHackOption(hackName, 40 + (i - 1) * 45)
end

-- Função que retorna o inimigo mais próximo
local function getClosestEnemy()
    local char = player.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then return nil end

    local closest = nil
    local shortestDist = math.huge

    for _, p in pairs(Players:GetPlayers()) do
        if p ~= player and p.Team ~= player.Team and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
            local dist = (char.HumanoidRootPart.Position - p.Character.HumanoidRootPart.Position).Magnitude
            if dist < shortestDist then
                shortestDist = dist
                closest = p
            end
        end
    end

    return closest
end

-- Hacks baseados no toggle

RunService.RenderStepped:Connect(function()
    if toggleHacks["Aimbot"] then
        local target = getClosestEnemy()
        if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
            local myHRP = player.Character.HumanoidRootPart
            local targetHRP = target.Character.HumanoidRootPart
            local dir = (targetHRP.Position - myHRP.Position).Unit
            player.Character:SetPrimaryPartCFrame(CFrame.new(myHRP.Position, myHRP.Position + dir))
        end
    end
end)

RunService.RenderStepped:Connect(function()
    if toggleHacks["Teleport Kill"] then
        local target = getClosestEnemy()
        if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
            local myHRP = player.Character.HumanoidRootPart
            local targetHRP = target.Character.HumanoidRootPart
            local offset = Vector3.new(0, 3, 0)
            player.Character:SetPrimaryPartCFrame(CFrame.new(targetHRP.Position + offset))
        end
    end
end)

RunService.Stepped:Connect(function()
    if toggleHacks["Wallhack (Noclip)"] then
        if player.Character then
            for _, part in pairs(player.Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        end
    else
        if player.Character then
            for _, part in pairs(player.Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = true
                end
            end
        end
    end
end)

local normalSpeed = 16
RunService.RenderStepped:Connect(function()
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        local humanoid = player.Character.Humanoid
        if toggleHacks["Speed Hack"] then
            humanoid.WalkSpeed = 50
        else
            humanoid.WalkSpeed = normalSpeed
        end
    end
end)

-- Fly Hack: botão pra ativar/desativar
local flying = false
local bodyVelocity
local flySpeed = 50

local flyBtn = Instance.new("TextButton")
flyBtn.Size = UDim2.new(0, 100, 0, 35)
flyBtn.Position = UDim2.new(0, 10, 1, -45)
flyBtn.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
flyBtn.BackgroundTransparency = 0.5
flyBtn.TextColor3 = Color3.new(1, 1, 1)
flyBtn.Text = "Fly Hack OFF"
flyBtn.Font = Enum.Font.SourceSansBold
flyBtn.TextScaled = true
flyBtn.Parent = mainFrame

flyBtn.MouseButton1Click:Connect(function()
    flying = not flying
    toggleHacks["Fly Hack"] = flying
    if flying and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
        bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        bodyVelocity.Parent = player.Character.HumanoidRootPart
        flyBtn.Text = "Fly Hack ON"
    else
        if bodyVelocity then
            bodyVelocity:Destroy()
            bodyVelocity = nil
        end
        flyBtn.Text = "Fly Hack OFF"
    end
end)

RunService.RenderStepped:Connect(function()
    if flying and bodyVelocity and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local cam = workspace.CurrentCamera
        local dir = cam.CFrame.LookVector
        bodyVelocity.Velocity = dir * flySpeed
    end
end)

-- ESP Highlight
local highlights = {}

RunService.RenderStepped:Connect(function()
    if toggleHacks["ESP"] then
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= player and p.Team ~= player.Team and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                if not highlights[p] then
                    local hl = Instance.new("Highlight")
                    hl.FillColor = Color3.fromRGB(255, 0, 0)
                    hl.OutlineColor = Color3.new(1, 1, 1)
                    hl.FillTransparency = 0.5
                    hl.OutlineTransparency = 0
                    hl.Parent = workspace
                    hl.Adornee = p.Character
                    highlights[p] = hl
                end
            end
        end
        for p, hl in pairs(highlights) do
            if not p.Parent or p.Team == player.Team or not p.Character or not p.Character:FindFirstChild("HumanoidRootPart") then
                if hl then hl:Destroy() end
                highlights[p] = nil
            end
        end
    else
        for p, hl in pairs(highlights) do
            if hl then hl:Destroy() end
        end
        highlights = {}
    end
end)

-- Silent Aim só um exemplo para integração, sem keybind (você adapta ao seu tiro)
local function getSilentAimTarget()
    if toggleHacks["Silent Aim"] then
        return getClosestEnemy()
    else
        return nil
    end
end

-- Fim do código
