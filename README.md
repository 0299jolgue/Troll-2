if game.CoreGui:FindFirstChild("JolgueHub") then
    game.CoreGui.JolgueHub:Destroy()
end

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "JolgueHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game.CoreGui

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 300, 0, 380)
Frame.Position = UDim2.new(0.5, -150, 0.5, -190)
Frame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Frame.BorderSizePixel = 0
Frame.Parent = ScreenGui
Instance.new("UICorner", Frame)

-- Título
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -40, 0, 40)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "Jolgue Hub"
Title.TextColor3 = Color3.new(1,1,1)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 22
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Frame

-- Botão de minimizar
local btnMinimizar = Instance.new("TextButton")
btnMinimizar.Size = UDim2.new(0, 30, 0, 30)
btnMinimizar.Position = UDim2.new(1, -35, 0, 5)
btnMinimizar.Text = "-"
btnMinimizar.Font = Enum.Font.GothamBold
btnMinimizar.TextSize = 18
btnMinimizar.TextColor3 = Color3.new(1,1,1)
btnMinimizar.BackgroundColor3 = Color3.fromRGB(60,60,60)
btnMinimizar.Parent = Frame
Instance.new("UICorner", btnMinimizar)

-- Container do conteúdo
local ContentFrame = Instance.new("Frame")
ContentFrame.Size = UDim2.new(1, 0, 1, -40)
ContentFrame.Position = UDim2.new(0, 0, 0, 40)
ContentFrame.BackgroundTransparency = 1
ContentFrame.Parent = Frame

-- Botões de navegação
local NavFrame = Instance.new("Frame")
NavFrame.Size = UDim2.new(1, -20, 0, 40)
NavFrame.Position = UDim2.new(0, 10, 0, 5)
NavFrame.BackgroundTransparency = 1
NavFrame.Parent = ContentFrame

local UIListNav = Instance.new("UIListLayout")
UIListNav.FillDirection = Enum.FillDirection.Horizontal
UIListNav.HorizontalAlignment = Enum.HorizontalAlignment.Left
UIListNav.Padding = UDim.new(0, 10)
UIListNav.Parent = NavFrame

-- Função de botão
local function criarBotao(nome, parent, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 120, 1, 0)
    btn.Text = nome
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 15
    btn.TextColor3 = Color3.new(1,1,1)
    btn.BackgroundColor3 = Color3.fromRGB(70,70,70)
    btn.Parent = parent
    Instance.new("UICorner", btn)
    btn.MouseButton1Click:Connect(callback)
    return btn
end

-- Abas
local MainTab = Instance.new("Frame")
MainTab.Size = UDim2.new(1, -20, 0, 280)
MainTab.Position = UDim2.new(0, 10, 0, 50)
MainTab.BackgroundTransparency = 1
MainTab.Parent = ContentFrame

local ItemsTab = Instance.new("Frame")
ItemsTab.Size = UDim2.new(1, -20, 0, 280)
ItemsTab.Position = UDim2.new(0, 10, 0, 50)
ItemsTab.BackgroundTransparency = 1
ItemsTab.Visible = false
ItemsTab.Parent = ContentFrame

-- Minimizar lógica
local minimizado = false
btnMinimizar.MouseButton1Click:Connect(function()
    minimizado = not minimizado
    ContentFrame.Visible = not minimizado
    btnMinimizar.Text = minimizado and "+" or "-"
end)

-- Navegação
criarBotao("Principal", NavFrame, function()
    MainTab.Visible = true
    ItemsTab.Visible = false
end)

criarBotao("Itens", NavFrame, function()
    MainTab.Visible = false
    ItemsTab.Visible = true
end)

criarBotao("Fechar", NavFrame, function()
    ScreenGui:Destroy()
end)

-- Botão girar
local btnGirar = criarBotao("Girar Player", MainTab, function() end)
btnGirar.Size = UDim2.new(1, 0, 0, 40)
btnGirar.Position = UDim2.new(0, 0, 0, 0)

local girar = false
btnGirar.MouseButton1Click:Connect(function()
    girar = not girar
    btnGirar.Text = girar and "Parar Giro" or "Girar Player"
    spawn(function()
        while girar and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") do
            LocalPlayer.Character:SetPrimaryPartCFrame(LocalPlayer.Character.PrimaryPart.CFrame * CFrame.Angles(0, math.rad(30), 0))
            task.wait(0.05)
        end
    end)
end)

-- Botão Infinite Yield
local btnInfinite = criarBotao("Infinite Yield", MainTab, function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"))()
end)
btnInfinite.Position = UDim2.new(0, 0, 0, 50)
btnInfinite.Size = UDim2.new(1, 0, 0, 40)

-- Botão crash com partículas
local btnParticles = criarBotao("Crash Partículas", MainTab, function()
    for _, plr in pairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer then
            local char = plr.Character
            if char and char:FindFirstChild("HumanoidRootPart") then
                local part = Instance.new("Part")
                part.Size = Vector3.new(1,1,1)
                part.Anchored = true
                part.CanCollide = false
                part.Transparency = 1
                part.CFrame = char.HumanoidRootPart.CFrame
                part.Parent = workspace

                local emitter = Instance.new("ParticleEmitter", part)
                emitter.Rate = 1000
                emitter.Lifetime = NumberRange.new(5)
                emitter.Speed = NumberRange.new(50)

                task.delay(10, function() part:Destroy() end)
            end
        end
    end
end)
btnParticles.Position = UDim2.new(0, 0, 0, 100)
btnParticles.Size = UDim2.new(1, 0, 0, 40)

-- Aba de Itens
local TitleItens = Instance.new("TextLabel")
TitleItens.Size = UDim2.new(1, 0, 0, 30)
TitleItens.BackgroundTransparency = 1
TitleItens.Text = "Itens disponíveis"
TitleItens.Font = Enum.Font.GothamBold
TitleItens.TextSize = 18
TitleItens.TextColor3 = Color3.new(1,1,1)
TitleItens.Parent = ItemsTab

local UIListItens = Instance.new("UIListLayout", ItemsTab)
UIListItens.Padding = UDim.new(0, 8)
UIListItens.SortOrder = Enum.SortOrder.LayoutOrder

local function pegarItem(nomeItem)
    local backpack = LocalPlayer.Backpack
    local item = workspace:FindFirstChild(nomeItem)
    if item and item:IsA("Tool") then
        item.Parent = backpack
    else
        warn("Item não encontrado:", nomeItem)
    end
end

local itensExemplo = {"Sword", "Gun", "Tool"} -- Altere esses nomes conforme seu jogo

for _, nome in ipairs(itensExemplo) do
    criarBotao("Pegar " .. nome, ItemsTab, function()
        pegarItem(nome)
    end)
end
