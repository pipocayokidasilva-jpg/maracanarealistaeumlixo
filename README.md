local Players = game:GetService("Players")

local player = Players.LocalPlayer

--// GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "TeleportGUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = player:WaitForChild("PlayerGui")

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0,220,0,180)
Frame.Position = UDim2.new(0.5,-110,0.5,-90)
Frame.BackgroundColor3 = Color3.fromRGB(35,35,35)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true
Frame.Parent = ScreenGui

Instance.new("UICorner", Frame)

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1,0,0,35)
Title.BackgroundTransparency = 1
Title.Text = "Teleport GUI"
Title.TextScaled = true
Title.Font = Enum.Font.GothamBold
Title.TextColor3 = Color3.new(1,1,1)
Title.Parent = Frame

--// Botão TP Fixo
local TPToggle = Instance.new("TextButton")
TPToggle.Size = UDim2.new(0.8,0,0,45)
TPToggle.Position = UDim2.new(0.1,0,0,45)
TPToggle.BackgroundColor3 = Color3.fromRGB(170,0,0)
TPToggle.Text = "TP Fixo: OFF"
TPToggle.TextScaled = true
TPToggle.Font = Enum.Font.GothamBold
TPToggle.TextColor3 = Color3.new(1,1,1)
TPToggle.Parent = Frame

Instance.new("UICorner", TPToggle)

--// Botão Farm Coins
local CoinToggle = Instance.new("TextButton")
CoinToggle.Size = UDim2.new(0.8,0,0,45)
CoinToggle.Position = UDim2.new(0.1,0,0,100)
CoinToggle.BackgroundColor3 = Color3.fromRGB(170,0,0)
CoinToggle.Text = "Farm Coins: OFF"
CoinToggle.TextScaled = true
CoinToggle.Font = Enum.Font.GothamBold
CoinToggle.TextColor3 = Color3.new(1,1,1)
CoinToggle.Parent = Frame

Instance.new("UICorner", CoinToggle)

--// Variáveis
local tpEnabled = false
local coinEnabled = false

local TPPosition = Vector3.new(-195,569,-88)

--// Funções

local function getHRP()
    local character = player.Character or player.CharacterAdded:Wait()
    return character:WaitForChild("HumanoidRootPart")
end

local function FixedTPLoop()
    while tpEnabled do
        local hrp = getHRP()
        hrp.CFrame = CFrame.new(TPPosition)
        task.wait(2)
    end
end

local function getCoinPart(model)
    if model.PrimaryPart then
        return model.PrimaryPart
    end

    return model:FindFirstChildWhichIsA("BasePart", true)
end

local function CoinLoop()
    while coinEnabled do
        local hrp = getHRP()

        for _,obj in ipairs(workspace:GetDescendants()) do
            if not coinEnabled then
                break
            end

            if obj:IsA("Model") and obj.Name == "CoinModel" then
                local part = getCoinPart(obj)

                if part then
                    hrp.CFrame = part.CFrame + Vector3.new(0,3,0)
                    task.wait(0.2)
                end
            end
        end

        task.wait(1)
    end
end

--// Eventos

TPToggle.MouseButton1Click:Connect(function()
    tpEnabled = not tpEnabled

    if tpEnabled then
        TPToggle.Text = "TP Fixo: ON"
        TPToggle.BackgroundColor3 = Color3.fromRGB(0,170,0)
        task.spawn(FixedTPLoop)
    else
        TPToggle.Text = "TP Fixo: OFF"
        TPToggle.BackgroundColor3 = Color3.fromRGB(170,0,0)
    end
end)

CoinToggle.MouseButton1Click:Connect(function()
    coinEnabled = not coinEnabled

    if coinEnabled then
        CoinToggle.Text = "Farm Coins: ON"
        CoinToggle.BackgroundColor3 = Color3.fromRGB(0,170,0)
        task.spawn(CoinLoop)
    else
        CoinToggle.Text = "Farm Coins: OFF"
        CoinToggle.BackgroundColor3 = Color3.fromRGB(170,0,0)
    end
end)
