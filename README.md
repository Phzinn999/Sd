-- Interface do Script
local ScreenGui = Instance.new("ScreenGui")
local MinimizeButton = Instance.new("TextButton")
local Frame = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local ESPButton = Instance.new("TextButton")
local SpeedHackButton = Instance.new("TextButton")
local CollectRareItemsButton = Instance.new("TextButton")
local SpeedSlider = Instance.new("TextBox")
local InstantKillButton = Instance.new("TextButton")

-- Propriedades da Interface
ScreenGui.Parent = game.CoreGui

MinimizeButton.Parent = ScreenGui
MinimizeButton.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
MinimizeButton.Position = UDim2.new(0.05, 0, 0.05, 0)
MinimizeButton.Size = UDim2.new(0, 50, 0, 50)
MinimizeButton.Font = Enum.Font.SourceSans
MinimizeButton.Text = "O"
MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeButton.TextSize = 24

Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Frame.Position = UDim2.new(0.1, 0, 0.1, 0)
Frame.Size = UDim2.new(0, 300, 0, 400)
Frame.Visible = false

Title.Parent = Frame
Title.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Title.Size = UDim2.new(1, 0, 0, 50)
Title.Font = Enum.Font.SourceSans
Title.Text = "Script Menu"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 24

ESPButton.Parent = Frame
ESPButton.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
ESPButton.Position = UDim2.new(0, 0, 0.2, 0)
ESPButton.Size = UDim2.new(1, 0, 0, 50)
ESPButton.Font = Enum.Font.SourceSans
ESPButton.Text = "ESP (Ver Jogadores)"
ESPButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ESPButton.TextSize = 20

SpeedHackButton.Parent = Frame
SpeedHackButton.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
SpeedHackButton.Position = UDim2.new(0, 0, 0.35, 0)
SpeedHackButton.Size = UDim2.new(1, 0, 0, 50)
SpeedHackButton.Font = Enum.Font.SourceSans
SpeedHackButton.Text = "Speed Hack"
SpeedHackButton.TextColor3 = Color3.fromRGB(255, 255, 255)
SpeedHackButton.TextSize = 20

SpeedSlider.Parent = Frame
SpeedSlider.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
SpeedSlider.Position = UDim2.new(0, 0, 0.45, 0)
SpeedSlider.Size = UDim2.new(1, 0, 0, 50)
SpeedSlider.Font = Enum.Font.SourceSans
SpeedSlider.Text = "Digite a Velocidade"
SpeedSlider.TextColor3 = Color3.fromRGB(255, 255, 255)
SpeedSlider.TextSize = 20

CollectRareItemsButton.Parent = Frame
CollectRareItemsButton.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
CollectRareItemsButton.Position = UDim2.new(0, 0, 0.6, 0)
CollectRareItemsButton.Size = UDim2.new(1, 0, 0, 50)
CollectRareItemsButton.Font = Enum.Font.SourceSans
CollectRareItemsButton.Text = "Coletar Itens Raros"
CollectRareItemsButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CollectRareItemsButton.TextSize = 20

InstantKillButton.Parent = Frame
InstantKillButton.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
InstantKillButton.Position = UDim2.new(0, 0, 0.75, 0)
InstantKillButton.Size = UDim2.new(1, 0, 0, 50)
InstantKillButton.Font = Enum.Font.SourceSans
InstantKillButton.Text = "Morte Instantânea"
InstantKillButton.TextColor3 = Color3.fromRGB(255, 255, 255)
InstantKillButton.TextSize = 20

-- Função para minimizar e maximizar a interface
MinimizeButton.MouseButton1Click:Connect(function()
    Frame.Visible = not Frame.Visible
    MinimizeButton.Text = Frame.Visible and "-" or "O"
end)

-- Funções dos Botões
ESPButton.MouseButton1Click:Connect(function()
    local assassinName = "NomeDoAssassino" -- Substitua pelo nome real do assassino
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            local esp = Instance.new("BillboardGui", player.Character.Head)
            esp.Size = UDim2.new(3, 0, 3, 0) -- Aumentado para cobrir o corpo inteiro
            esp.AlwaysOnTop = true
            local frame = Instance.new("Frame", esp)
            frame.Size = UDim2.new(1, 0, 1, 0)
            frame.BackgroundTransparency = 0.5
            if player.Name == assassinName then
                frame.BackgroundColor3 = Color3.fromRGB(255, 0, 0) -- Vermelho para o assassino
            else
                frame.BackgroundColor3 = Color3.fromRGB(0, 255, 0) -- Verde para jogadores normais
            end
        end
    end
end)

SpeedHackButton.MouseButton1Click:Connect(function()
    local speed = tonumber(SpeedSlider.Text)
    if speed then
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = speed
    else
        print("Por favor, insira um valor numérico válido.")
    end
end)

CollectRareItemsButton.MouseButton1Click:Connect(function()
    for _, item in pairs(game.Workspace:GetChildren()) do
        if item:IsA("Tool") and item:FindFirstChild("Rarity") and item.Rarity.Value == "Rare" then
            item.Handle.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
        end
    end
end)

InstantKillButton.MouseButton1Click:Connect(function()
    local player = game.Players.LocalPlayer
    if player.Backpack:FindFirstChild("Knife") then
        player.Backpack.Knife.Activated:Connect(function()
            local target = player:GetMouse().Target.Parent
            if target and target:FindFirstChild("Humanoid") then
                target.Humanoid.Health = 0
            end
        end)
    else
        print("Faca não encontrada no inventário.")
    end
end)

-- Função para tornar o jogador imortal
game.Players.LocalPlayer.CharacterAdded:Connect(function(character)
    local humanoid = character:WaitForChild("Humanoid")
    humanoid:GetPropertyChangedSignal("Health"):Connect(function()
        if humanoid.Health < humanoid.MaxHealth then
            humanoid.Health = humanoid.MaxHealth
        end
    end)
end)
