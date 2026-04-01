# script.lua
--[[
  Developer: Shahd (The Queen) - shhode320
  Final Version: All UI Elements Draggable + Focus Fix
]]

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local UserInputService = game:GetService("UserInputService")
local StarterGui = game:GetService("StarterGui")

local localPlayer = Players.LocalPlayer
local pImageId = "rbxassetid://112445533849036"

-- إنشاء الواجهة
local sg = Instance.new("ScreenGui")
sg.Name = "Shahd_Draggable_Pro"
sg.ResetOnSpawn = false
sg.Parent = localPlayer.PlayerGui

-- دالة لجعل أي عنصر قابل للسحب (عشان يشتغل على الموبايل والكمبيوتر)
local function makeDraggable(frame)
    local dragging, dragInput, dragStart, startPos
    frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = frame.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then dragging = false end
            end)
        end
    end)
    frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
end

-----------------------------------------
-- [ اللوحة الكبيرة - Main Panel ]
-----------------------------------------
local mainFrame = Instance.new("Frame", sg)
mainFrame.Size = UDim2.new(0, 450, 0, 350)
mainFrame.Position = UDim2.new(0.5, -225, 0.5, -175)
mainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
mainFrame.Active = true
mainFrame.Parent = sg
makeDraggable(mainFrame) -- تفعيل السحب للوحة الكبيرة

local mainCorner = Instance.new("UICorner", mainFrame)
local mainStroke = Instance.new("UIStroke", mainFrame)
mainStroke.Color = Color3.fromRGB(255, 105, 180)
mainStroke.Thickness = 3

-- الهيدر
local header = Instance.new("Frame", mainFrame)
header.Size = UDim2.new(1, 0, 0, 60)
header.BackgroundTransparency = 1

local myImg = Instance.new("ImageLabel", header)
myImg.Size = UDim2.new(0, 45, 0, 45)
myImg.Position = UDim2.new(0, 15, 0, 10)
myImg.Image = pImageId
myImg.BackgroundTransparency = 1
Instance.new("UICorner", myImg).CornerRadius = UDim.new(1, 0)

local title = Instance.new("TextLabel", header)
title.Size = UDim2.new(1, -80, 1, 0)
title.Position = UDim2.new(0, 70, 0, 0)
title.Text = "Shahd Queen UI 👑🤘"
title.TextColor3 = Color3.new(1,1,1)
title.TextScaled = true
title.Font = Enum.Font.FredokaOne
title.BackgroundTransparency = 1

-- مربع الكتابة الرئيسي
local mainInput = Instance.new("TextBox", mainFrame)
mainInput.Size = UDim2.new(0.9, 0, 0, 50)
mainInput.Position = UDim2.new(0.05, 0, 0.25, 0)
mainInput.PlaceholderText = "عمو اكتب هنا... 😉"
mainInput.Text = ""
mainInput.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainInput.TextColor3 = Color3.new(1,1,1)
mainInput.TextScaled = true
Instance.new("UICorner", mainInput)

-- خانة الحجم
local sizeInput = Instance.new("TextBox", mainFrame)
sizeInput.Size = UDim2.new(0.2, 0, 0, 40)
sizeInput.Position = UDim2.new(0.05, 0, 0.45, 0)
sizeInput.Text = "60"
sizeInput.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
sizeInput.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", sizeInput)

-- قائمة الألوان
local colorFrame = Instance.new("ScrollingFrame", mainFrame)
colorFrame.Size = UDim2.new(0.65, 0, 0, 50)
colorFrame.Position = UDim2.new(0.3, 0, 0.45, 0)
colorFrame.CanvasSize = UDim2.new(2, 0, 0, 0)
colorFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
colorFrame.ScrollBarThickness = 2
Instance.new("UICorner", colorFrame)

local colors = {
    {C = Color3.fromRGB(255, 105, 180), Tag = "rgb(255,105,180)"},
    {C = Color3.fromRGB(255, 215, 0), Tag = "rgb(255,215,0)"},
    {C = Color3.fromRGB(0, 255, 255), Tag = "rgb(0,255,255)"},
    {C = Color3.fromRGB(0, 255, 0), Tag = "rgb(0,255,0)"},
    {C = Color3.fromRGB(255, 0, 0), Tag = "rgb(255,0,0)"},
    {C = Color3.fromRGB(170, 0, 255), Tag = "rgb(170,0,255)"},
    {C = Color3.fromRGB(255, 165, 0), Tag = "rgb(255,165,0)"},
    {C = Color3.fromRGB(0, 120, 255), Tag = "rgb(0,120,255)"}
}

local selectedColor = colors[1].Tag

for i, v in ipairs(colors) do
    local cBtn = Instance.new("TextButton", colorFrame)
    cBtn.Size = UDim2.new(0, 35, 0, 35)
    cBtn.Position = UDim2.new(0, (i-1)*45 + 5, 0, 5)
    cBtn.BackgroundColor3 = v.C
    cBtn.Text = ""
    Instance.new("UICorner", cBtn).CornerRadius = UDim.new(0.3, 0)
    cBtn.MouseButton1Click:Connect(function() 
        selectedColor = v.Tag 
        mainStroke.Color = v.C
        if sg:FindFirstChild("MiniInputFrame") then
            sg.MiniInputFrame.UIStroke.Color = v.C
        end
    end)
end

-----------------------------------------
-- [ المربع الميني القابل للتحريك - Draggable Mini Bar ]
-----------------------------------------
local miniFrame = Instance.new("Frame", sg)
miniFrame.Name = "MiniInputFrame"
miniFrame.Size = UDim2.new(0, 150, 0, 45)
miniFrame.Position = UDim2.new(0, 10, 0.7, 0) -- مكان البداية
miniFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
miniFrame.Active = true
Instance.new("UICorner", miniFrame)
local mStroke = Instance.new("UIStroke", miniFrame)
mStroke.Color = Color3.fromRGB(255, 105, 180)
mStroke.Thickness = 2
makeDraggable(miniFrame) -- 🔥 الحين صار يتحرك يا شهد!

local miniInput = Instance.new("TextBox", miniFrame)
miniInput.Size = UDim2.new(1, 0, 1, 0)
miniInput.BackgroundTransparency = 1
miniInput.PlaceholderText = "اكتب هنا سريع..."
miniInput.Text = ""
miniInput.TextColor3 = Color3.new(1,1,1)
miniInput.TextScaled = true
miniInput.Parent = miniFrame

-----------------------------------------
-- [ دالة الإرسال وحل الكيبورد ]
-----------------------------------------
local function sendMessage(inputBox)
    local text = inputBox.Text
    if text == "" then return end
    
    local size = sizeInput.Text
    local formatted = '<font color="'..selectedColor..'" size="'..size..'"><b>'..text..'</b></font>'
    
    local remote = ReplicatedStorage:FindFirstChild("RemoteEvents") and ReplicatedStorage.RemoteEvents:FindFirstChild("ChatEvent")
    
    if remote then
        remote:FireServer(formatted)
        inputBox.Text = "" 
        inputBox:ReleaseFocus() -- يقفل الكيبورد فوراً
    end
end

mainInput.FocusLost:Connect(function(enterPressed)
    if enterPressed then sendMessage(mainInput) end
end)

miniInput.FocusLost:Connect(function(enterPressed)
    if enterPressed then sendMessage(miniInput) end
end)

-- أزرار التحكم
local closeBtn = Instance.new("TextButton", mainFrame)
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -35, 0, 5)
closeBtn.Text = "X"
closeBtn.BackgroundColor3 = Color3.new(0.8, 0, 0)
closeBtn.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", closeBtn)
closeBtn.MouseButton1Click:Connect(function() sg:Destroy() end)

local minBtn = Instance.new("TextButton", mainFrame)
minBtn.Size = UDim2.new(0, 30, 0, 30)
minBtn.Position = UDim2.new(1, -70, 0, 5)
minBtn.Text = "-"
minBtn.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
minBtn.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", minBtn)

local icon = Instance.new("ImageButton", sg)
icon.Size = UDim2.new(0, 60, 0, 60)
icon.Position = UDim2.new(0.9, 0, 0.1, 0)
icon.Image = pImageId
icon.Visible = false
Instance.new("UICorner", icon).CornerRadius = UDim.new(1, 0)

minBtn.MouseButton1Click:Connect(function() mainFrame.Visible = false; icon.Visible = true end)
icon.MouseButton1Click:Connect(function() mainFrame.Visible = true; icon.Visible = false end)

StarterGui:SetCore("SendNotification", {
    Title = "Shahd Queen Final",
    Text = "كل شي يتحرك الحين يا ملكة! 👸💎",
    Icon = pImageId,
    Duration = 5
})

