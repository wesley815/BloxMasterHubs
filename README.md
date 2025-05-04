# BloxMasterHubs
-- BloxMaster Hub com GUI + Minimizador + Discord
repeat wait() until game:IsLoaded()

-- GUI principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "BloxMasterHub"
ScreenGui.Parent = game.CoreGui

local Main = Instance.new("Frame")
Main.Parent = ScreenGui
Main.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
Main.Position = UDim2.new(0.3, 0, 0.3, 0)
Main.Size = UDim2.new(0, 300, 0, 240)

local UICorner = Instance.new("UICorner", Main)
UICorner.CornerRadius = UDim.new(0, 12)

-- Ícone no canto
local Icon = Instance.new("ImageLabel")
Icon.Parent = Main
Icon.BackgroundTransparency = 1
Icon.Position = UDim2.new(0, 10, 0, 10)
Icon.Size = UDim2.new(0, 24, 0, 24)
Icon.Image = "content://media/external/downloads/576189" -- Pode trocar esse ID

-- Título
local Title = Instance.new("TextLabel")
Title.Parent = Main
Title.Position = UDim2.new(0, 40, 0, 5)
Title.Size = UDim2.new(0.8, 0, 0.2, 0)
Title.Text = "BloxMaster Hub"
Title.TextColor3 = Color3.fromRGB(200, 50, 200)
Title.BackgroundTransparency = 1
Title.Font = Enum.Font.GothamBold
Title.TextSize = 22
Title.TextXAlignment = Enum.TextXAlignment.Left

-- Minimizar botão
local MinBtn = Instance.new("TextButton")
MinBtn.Parent = Main
MinBtn.Position = UDim2.new(0.87, 0, 0.05, 0)
MinBtn.Size = UDim2.new(0, 25, 0, 25)
MinBtn.Text = "-"
MinBtn.BackgroundColor3 = Color3.fromRGB(50, 0, 50)
MinBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinBtn.Font = Enum.Font.GothamBold
MinBtn.TextSize = 20

local minimized = false
MinBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    for _, obj in ipairs(Main:GetChildren()) do
        if obj:IsA("TextButton") or obj:IsA("TextLabel") and obj ~= Title then
            obj.Visible = not minimized
        end
    end
end)

-- Criador de botão
local function createButton(name, y, callback)
    local btn = Instance.new("TextButton")
    btn.Name = name
    btn.Parent = Main
    btn.BackgroundColor3 = Color3.fromRGB(50, 0, 50)
    btn.Position = UDim2.new(0.1, 0, y, 0)
    btn.Size = UDim2.new(0.8, 0, 0.15, 0)
    btn.Text = name
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 16
    btn.MouseButton1Click:Connect(callback)
end

-- Funcionalidades
createButton("Auto Farm", 0.25, function()
    spawn(function()
        while true do wait()
            pcall(function()
                local enemy = workspace.Enemies:FindFirstChildOfClass("Model")
                if enemy then
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = enemy.HumanoidRootPart.CFrame * CFrame.new(0,10,0)
                end
            end)
        end
    end)
end)

createButton("Auto Raid", 0.45, function()
    spawn(function()
        while true do wait(5)
            pcall(function()
                local raidBtn = workspace:FindFirstChild("StartRaidButton")
                if raidBtn then
                    fireclickdetector(raidBtn.ClickDetector)
                end
            end)
        end
    end)
end)

createButton("Mostrar Frutas", 0.65, function()
    spawn(function()
        while true do wait(10)
            for _, fruit in pairs(workspace:GetChildren()) do
                if fruit:IsA("Tool") and string.find(fruit.Name, "Fruit") then
                    game.StarterGui:SetCore("SendNotification", {
                        Title = "BloxMaster",
                        Text = "Fruta: " .. fruit.Name,
                        Duration = 5
                    })
                end
            end
        end
    end)
end)

-- Discord Button
createButton("Discord", 0.85, function()
    setclipboard("(https://discord.gg/JsyNJWB7)") -- Troque pelo seu link real
    game.StarterGui:SetCore("SendNotification", {
        Title = "BloxMaster",
        Text = "Convite copiado!",
        Duration = 4
    })
end)
