local chave = "SilexX_2024_Protegido"
if chave ~= "SilexX_2024_Protegido" then return end

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")

pcall(function() if game.CoreGui:FindFirstChild("DavzScript") then game.CoreGui.DavzScript:Destroy() end end)

local sg = Instance.new("ScreenGui", game.CoreGui)
sg.Name = "DavzScript"
sg.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- Bolinha "S" arrastável manualmente
local btnMini = Instance.new("Frame", sg)
btnMini.Size = UDim2.new(0, 48, 0, 48)
btnMini.Position = UDim2.new(0, 15, 0.5, -24)
btnMini.BackgroundColor3 = Color3.fromRGB(10,10,10)
btnMini.BorderSizePixel = 0
btnMini.Active = true
btnMini.ZIndex = 10
local minicorner = Instance.new("UICorner", btnMini)
minicorner.CornerRadius = UDim.new(1,0)
local sLabel = Instance.new("TextLabel", btnMini)
sLabel.Size = UDim2.new(1,0,1,0)
sLabel.BackgroundTransparency = 1
sLabel.Text = "S"
sLabel.Font = Enum.Font.GothamBlack
sLabel.TextSize = 34
sLabel.TextColor3 = Color3.fromRGB(20,180,255)
sLabel.TextStrokeTransparency = 0.5
sLabel.ZIndex = 11

-- Lógica de arrastar manual da bolinha (mouse e touch)
do
    local dragging, dragInput, dragStart, startPos
    btnMini.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = btnMini.Position
            dragInput = input
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)
    btnMini.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            dragInput = input
        end
    end)
    UIS.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            btnMini.Position = UDim2.new(
                startPos.X.Scale, startPos.X.Offset + delta.X,
                startPos.Y.Scale, startPos.Y.Offset + delta.Y
            )
        end
    end)
end

-- Interface principal quadrada com bordas arredondadas, canto superior direito, maior
local fr = Instance.new("Frame", sg)
fr.Size = UDim2.new(0,260,0,330)
fr.Position = UDim2.new(1, -280, 0, 30)
fr.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
fr.BorderSizePixel = 0
fr.Visible = false
fr.Active = true

-- Borda colorida animada
local sRgbColor = Color3.fromRGB(20,180,255)
local stroke = Instance.new("UIStroke", fr)
stroke.Thickness = 6
stroke.Color = sRgbColor
stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

-- Bordas arredondadas
local frcorner = Instance.new("UICorner", fr)
frcorner.CornerRadius = UDim.new(0.2, 30)

-- S RGB gigante animado no fundo
local sRgb = Instance.new("TextLabel", fr)
sRgb.Size = UDim2.new(1,0,1,0)
sRgb.Position = UDim2.new(0,0,0,0)
sRgb.BackgroundTransparency = 1
sRgb.Text = "S"
sRgb.Font = Enum.Font.GothamBlack
sRgb.TextSize = 260
sRgb.TextStrokeTransparency = 0.7
sRgb.TextTransparency = 0.8
sRgb.ZIndex = 0
task.spawn(function()
    local t = 0
    while true do
        t = t + RunService.RenderStepped:Wait()
        local c = Color3.fromHSV((t*0.2)%1, 0.7, 1)
        sRgb.TextColor3 = c
        sRgbColor = c
        stroke.Color = sRgbColor
        title.TextColor3 = sRgbColor
    end
end)

-- Novo título, cor igual borda colorida
title = Instance.new("TextLabel", fr)
title.Size = UDim2.new(1, 0, 0, 38)
title.Position = UDim2.new(0,0,0,10)
title.BackgroundTransparency = 1
title.Text = "SilexX scripts"
title.Font = Enum.Font.GothamBlack
title.TextSize = 32
title.TextColor3 = sRgbColor
title.TextStrokeTransparency = 0.75
title.ZIndex = 2

-- Botões centralizados, faixa preta atrás deles, botões cinza
local function makeButton(name, y)
    local b = Instance.new("TextButton")
    b.Size = UDim2.new(0.85, 0, 0, 38)
    b.Position = UDim2.new(0.075, 0, y, 0)
    b.Text = name
    b.Font = Enum.Font.GothamBold
    b.TextSize = 19
    b.TextColor3 = Color3.fromRGB(255,255,255)
    b.BackgroundColor3 = Color3.fromRGB(40,40,40) -- cinza
    b.BorderSizePixel = 0
    b.ZIndex = 3

    local corner = Instance.new("UICorner", b)
    corner.CornerRadius = UDim.new(0.5, 0)

    local faixa = Instance.new("Frame", fr)
    faixa.Size = b.Size
    faixa.Position = b.Position
    faixa.BackgroundColor3 = Color3.fromRGB(0,0,0) -- fundo preto
    faixa.BorderSizePixel = 0
    faixa.ZIndex = 2
    local fc = Instance.new("UICorner", faixa)
    fc.CornerRadius = UDim.new(0.5, 0)

    b:GetPropertyChangedSignal("Position"):Connect(function()
        faixa.Position = b.Position
    end)
    b:GetPropertyChangedSignal("Size"):Connect(function()
        faixa.Size = b.Size
    end)

    b.MouseEnter:Connect(function() b.BackgroundColor3 = Color3.fromRGB(70,70,70) end)
    b.MouseLeave:Connect(function() b.BackgroundColor3 = Color3.fromRGB(40,40,40) end)

    b.Parent = fr
    return b
end

local btnInfJump = makeButton("Pulo Inf: Desligado", 0.23)
local btnSpeed = makeButton("Boost Speed: Desligado", 0.39)
local btnGod = makeButton("God Mod: Desligado", 0.55)
local btnEsp = makeButton("ESP: Desligado", 0.71)

-- Funções dos botões (igual seu script original)
local espOn = false
local highlights = {}
local function updateESP()
    for _, v in pairs(highlights) do if v and v.Parent then v:Destroy() end end
    table.clear(highlights)
    if espOn then
        for _, plr in ipairs(Players:GetPlayers()) do
            if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                local hl = Instance.new("Highlight")
                hl.Adornee = plr.Character
                hl.FillColor = Color3.fromRGB(255, 0, 0)
                hl.OutlineColor = Color3.fromRGB(255, 255, 0)
                hl.Parent = plr.Character
                table.insert(highlights, hl)
            end
        end
    end
end
btnEsp.MouseButton1Click:Connect(function()
    espOn = not espOn
    btnEsp.Text = "ESP: " .. (espOn and "Ligado" or "Desligado")
    updateESP()
end)
Players.PlayerAdded:Connect(function(plr)
    plr.CharacterAdded:Connect(function() if espOn then wait(1) updateESP() end end)
end)
Players.PlayerRemoving:Connect(function() if espOn then updateESP() end end)
RunService.RenderStepped:Connect(function() if espOn then updateESP() end end)

local infJumpOn = false
btnInfJump.MouseButton1Click:Connect(function()
    infJumpOn = not infJumpOn
    btnInfJump.Text = "Pulo Inf: " .. (infJumpOn and "Ligado" or "Desligado")
end)
UIS.JumpRequest:Connect(function()
    if infJumpOn then
        local char = LocalPlayer.Character
        if char then
            local hum = char:FindFirstChildWhichIsA("Humanoid")
            if hum then
                hum:ChangeState(Enum.HumanoidStateType.Jumping)
            end
        end
    end
end)

local godOn = false
btnGod.MouseButton1Click:Connect(function()
    godOn = not godOn
    btnGod.Text = "God Mod: " .. (godOn and "Ligado" or "Desligado")
end)

local function godAura()
    local char = LocalPlayer.Character
    if not char then return end
    local hum = char:FindFirstChildWhichIsA("Humanoid")
    if not hum then return end
    if hum.Health < hum.MaxHealth then
        hum.Health = hum.MaxHealth
    end
    for _,v in ipairs(char:GetChildren()) do
        if v:IsA("BasePart") and v.Name:find("Effect") then
            v:Destroy()
        elseif v:IsA("ForceField") then
            v:Destroy()
        end
    end
    for _,v in ipairs(char:GetDescendants()) do
        if v.Name:lower():find("ragdoll") or v.Name:lower():find("stun") then
            if v.Destroy then pcall(function() v:Destroy() end) end
        end
    end
end

RunService.RenderStepped:Connect(function()
    if godOn then
        pcall(godAura)
    end
end)

local speedOn = false
local boostSpeed = 50

btnSpeed.MouseButton1Click:Connect(function()
    speedOn = not speedOn
    btnSpeed.Text = "Boost Speed: " .. (speedOn and "Ligado" or "Desligado")
end)

local function clearSlows()
    local char = LocalPlayer.Character
    if not char then return end
    for _,v in ipairs(char:GetChildren()) do
        if v:IsA("NumberValue") or v:IsA("IntValue") or v:IsA("StringValue") then
            if v.Name:lower():find("speed") or v.Name:lower():find("slow") then
                v:Destroy()
            end
        end
    end
    local hum = char:FindFirstChildWhichIsA("Humanoid")
    if hum and hum:GetAttribute("WalkSpeed") then
        hum:SetAttribute("WalkSpeed", nil)
    end
    for _,v in ipairs(char:GetDescendants()) do
        if v:IsA("BodyVelocity") or v:IsA("BodyForce") then
            v:Destroy()
        end
    end
end

local function forceSpeed()
    local char = LocalPlayer.Character
    if not char then return end
    local hum = char:FindFirstChildWhichIsA("Humanoid")
    if not hum then return end

    if speedOn then
        clearSlows()
        if hum.WalkSpeed ~= boostSpeed then
            hum.WalkSpeed = boostSpeed
        end
    else
        if hum.WalkSpeed ~= 16 then
            hum.WalkSpeed = 16
        end
    end
end

local function hookWalkSpeed()
    local char = LocalPlayer.Character
    if not char then return end
    local hum = char:FindFirstChildWhichIsA("Humanoid")
    if not hum then return end
    hum:GetPropertyChangedSignal("WalkSpeed"):Connect(function()
        if speedOn and hum.WalkSpeed ~= boostSpeed then
            hum.WalkSpeed = boostSpeed
        end
    end)
end

Players.LocalPlayer.CharacterAdded:Connect(function()
    wait(0.5)
    hookWalkSpeed()
end)
if Players.LocalPlayer.Character then
    hookWalkSpeed()
end

task.spawn(function()
    while true do
        pcall(forceSpeed)
        task.wait(1/60)
    end
end)

-- Clique/tap na bolinha abre/fecha painel (sem bloquear arrasto)
do
    local lastClick = 0
    btnMini.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            -- Espera mouse/touch soltar para considerar clique curto (sem arrastar)
            local dragStart = input.Position
            local released = false
            local moved = false

            -- Detecta movimento
            local moveConn, endConn
            moveConn = UIS.InputChanged:Connect(function(moveInput)
                if (moveInput.UserInputType == Enum.UserInputType.MouseMovement or moveInput.UserInputType == Enum.UserInputType.Touch) and not released then
                    local delta = moveInput.Position - dragStart
                    if delta.Magnitude > 8 then
                        moved = true
                        if moveConn then moveConn:Disconnect() end
                        if endConn then endConn:Disconnect() end
                    end
                end
            end)
            endConn = input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    released = true
                    -- Só considera clique se não arrastou
                    if not moved then
                        -- Previne bug de múltiplos cliques rápidos
                        local now = tick()
                        if now - lastClick > 0.15 then
                            fr.Visible = not fr.Visible
                            lastClick = now
                        end
                    end
                    if moveConn then moveConn:Disconnect() end
                    if endConn then endConn:Disconnect() end
                end
            end)
        end
    end)
end

-- Arrastar manual do painel principal (igual a bolinha)
do
    local dragging, dragInput, dragStart, startPos
    fr.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = fr.Position
            dragInput = input
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)
    fr.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            dragInput = input
        end
    end)
    UIS.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            fr.Position = UDim2.new(
                startPos.X.Scale, startPos.X.Offset + delta.X,
                startPos.Y.Scale, startPos.Y.Offset + delta.Y
            )
        end
    end)
end
