-- Muscle Legends
-- Auto Train + Speed Controller + Auto Rebirth
-- Auto Barbell + Auto Press E

repeat task.wait() until game:IsLoaded()

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local VirtualInputManager =
    game:GetService("VirtualInputManager")

local player = Players.LocalPlayer

-- Координаты цели
local TARGET = Vector3.new(-2800.58, 174.86, 5473.22)

-- Настройки
local running = true
local autoTrain = true
local autoRebirth = true
local autoBarbell = false
local autoPressE = false

local trainInterval = 0.30
local rebirthInterval = 1

-- Удаление старого меню
local playerGui = player:WaitForChild("PlayerGui")
local oldGui = playerGui:FindFirstChild("MuscleLegendsMenu")
if oldGui then
    oldGui:Destroy()
end

-- GUI
local gui = Instance.new("ScreenGui")
gui.Name = "MuscleLegendsMenu"
gui.ResetOnSpawn = false
gui.Parent = playerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 280, 0, 370)
frame.Position = UDim2.new(0, 25, 0.25, 0)
frame.BackgroundColor3 = Color3.fromRGB(28, 28, 36)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
frame.Parent = gui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 10)
corner.Parent = frame

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -20, 0, 32)
title.Position = UDim2.new(0, 10, 0, 5)
title.BackgroundTransparency = 1
title.Text = "MUSCLE LEGENDS"
title.TextColor3 = Color3.fromRGB(255, 210, 70)
title.Font = Enum.Font.GothamBold
title.TextSize = 17
title.Parent = frame

local status = Instance.new("TextLabel")
status.Size = UDim2.new(1, -20, 0, 34)
status.Position = UDim2.new(0, 10, 1, -38)
status.BackgroundTransparency = 1
status.Text = "Статус: запущен"
status.TextColor3 = Color3.fromRGB(220, 220, 220)
status.Font = Enum.Font.Gotham
status.TextSize = 11
status.TextWrapped = true
status.Parent = frame

local ON_COLOR = Color3.fromRGB(40, 145, 80)
local OFF_COLOR = Color3.fromRGB(100, 100, 110)

local function makeButton(text, y, color)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -24, 0, 32)
    button.Position = UDim2.new(0, 12, 0, y)
    button.BackgroundColor3 = color
    button.BorderSizePixel = 0
    button.Text = text
    button.TextColor3 = Color3.new(1, 1, 1)
    button.Font = Enum.Font.GothamBold
    button.TextSize = 12
    button.Parent = frame

    local c = Instance.new("UICorner")
    c.CornerRadius = UDim.new(0, 7)
    c.Parent = button

    return button
end

local function updateButton(button, name, enabled)
    button.Text = name .. ": " .. (enabled and "ON" or "OFF")
    button.BackgroundColor3 = enabled and ON_COLOR or OFF_COLOR
end

-- Кнопки меню
local trainButton = makeButton("Auto Train: ON", 42, ON_COLOR)
local rebirthButton = makeButton("Auto Rebirth: ON", 80, ON_COLOR)
local barbellButton = makeButton("Auto Barbell: OFF", 118, OFF_COLOR)
local eButton = makeButton("Auto Press E: OFF", 156, OFF_COLOR)

-- Регулятор скорости
local speedLabel = Instance.new("TextLabel")
speedLabel.Size = UDim2.new(1, -24, 0, 24)
speedLabel.Position = UDim2.new(0, 12, 0, 194)
speedLabel.BackgroundTransparency = 1
speedLabel.TextColor3 = Color3.new(1, 1, 1)
speedLabel.Font = Enum.Font.GothamBold
speedLabel.TextSize = 12
speedLabel.Parent = frame

local slowerButton = makeButton(
    "- Медленнее", 224, Color3.fromRGB(70, 90, 140)
)
slowerButton.Size = UDim2.new(0, 122, 0, 30)

local fasterButton = makeButton(
    "+ Быстрее", 224, Color3.fromRGB(70, 90, 140)
)
fasterButton.Size = UDim2.new(0, 122, 0, 30)
fasterButton.Position = UDim2.new(0, 146, 0, 224)

local closeButton = makeButton(
    "CLOSE", 263, Color3.fromRGB(165, 55, 55)
)

local function updateSpeedLabel()
    speedLabel.Text = string.format(
        "Интервал тренировки: %.2f сек", trainInterval
    )
end

updateSpeedLabel()

slowerButton.MouseButton1Click:Connect(function()
    trainInterval = math.min(2, trainInterval + 0.05)
    updateSpeedLabel()
end)

fasterButton.MouseButton1Click:Connect(function()
    trainInterval = math.max(0.05, trainInterval - 0.05)
    updateSpeedLabel()
end)

-- Переключатели
trainButton.MouseButton1Click:Connect(function()
    autoTrain = not autoTrain
    updateButton(trainButton, "Auto Train", autoTrain)
end)

rebirthButton.MouseButton1Click:Connect(function()
    autoRebirth = not autoRebirth
    updateButton(rebirthButton, "Auto Rebirth", autoRebirth)
end)

barbellButton.MouseButton1Click:Connect(function()
    autoBarbell = not autoBarbell
    updateButton(barbellButton, "Auto Barbell", autoBarbell)

    if autoBarbell then
        status.Text = "Телепорт к цели..."
    else
        status.Text = "Auto Barbell выключен"
    end
end)

eButton.MouseButton1Click:Connect(function()
    autoPressE = not autoPressE
    updateButton(eButton, "Auto Press E", autoPressE)
end)

-- Телепорт
local function teleportToTarget()
    local character = player.Character
    if not character then return false end

    local root = character:FindFirstChild("HumanoidRootPart")
    local humanoid = character:FindFirstChildOfClass("Humanoid")

    if not root or not humanoid or humanoid.Health <= 0 then
        return false
    end

    return pcall(function()
        root.CFrame = CFrame.new(TARGET)
    end)
end

-- Auto Train
task.spawn(function()
    while running do
        task.wait(trainInterval)
        if not running then break end

        if autoTrain then
            pcall(function()
                local event = player:FindFirstChild("muscleEvent")
                if event and event:IsA("RemoteEvent") then
                    event:FireServer("rep")
                end
            end)
        end
    end
end)

-- Auto Rebirth
task.spawn(function()
    while running do
        task.wait(rebirthInterval)
        if not running then break end

        if autoRebirth then
            pcall(function()
                local events =
                    ReplicatedStorage:FindFirstChild("rEvents")
                local remote = events
                    and events:FindFirstChild("rebirthRemote")

                if remote and remote:IsA("RemoteFunction") then
                    remote:InvokeServer("rebirthRequest")
                end
            end)
        end
    end
end)

-- Auto Press E
task.spawn(function()
    while running do
        task.wait(0.2)

        if autoPressE then
            pcall(function()
                VirtualInputManager:SendKeyEvent(
                    true, Enum.KeyCode.E, false, game
                )
                task.wait(0.05)
                VirtualInputManager:SendKeyEvent(
                    false, Enum.KeyCode.E, false, game
                )
            end)
        end
    end
end)

-- Телепорт после возрождения
local function onCharacterAdded(character)
    task.spawn(function()
        local root = character:WaitForChild(
            "HumanoidRootPart", 10
        )
        if not root then return end

        task.wait(1.5)

        if running and autoBarbell then
            if teleportToTarget() then
                status.Text = "Телепорт выполнен"
            end
        end
    end)
end

player.CharacterAdded:Connect(onCharacterAdded)

-- Закрытие
closeButton.MouseButton1Click:Connect(function()
    running = false
    autoTrain = false
    autoRebirth = false
    autoBarbell = false
    autoPressE = false
    gui:Destroy()
end)

status.Text = "Скрипт запущен"
