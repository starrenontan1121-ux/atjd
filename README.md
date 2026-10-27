-- Zerohub AI Assistant Chat UI
local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")
local player = Players.LocalPlayer

-- 既存のGUIがあれば削除
if CoreGui:FindFirstChild("ZeroHubAIChat") then
    CoreGui.ZeroHubAIChat:Destroy()
end

-- ScreenGuiの作成
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ZeroHubAIChat"
screenGui.Parent = CoreGui

-- メインフレーム
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 350, 0, 450)
mainFrame.Position = UDim2.new(0.5, -175, 0.5, -225)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true -- ドラッグ可能にする
mainFrame.Parent = screenGui

-- 角丸
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 10)
corner.Parent = mainFrame

-- タイトルバー
local titleBar = Instance.new("TextLabel")
titleBar.Size = UDim2.new(1, 0, 0, 40)
titleBar.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
titleBar.Text = "  🤖 zerohub AI Assistant"
titleBar.TextColor3 = Color3.fromRGB(255, 255, 255)
titleBar.TextSize = 16
titleBar.Font = Enum.Font.SourceSansBold
titleBar.TextXAlignment = Enum.TextXAlignment.Left
titleBar.Parent = mainFrame

local titleCorner = Instance.new("UICorner")
titleCorner.CornerRadius = UDim.new(0, 10)
titleCorner.Parent = titleBar

-- 閉じるボタン
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 40, 0, 40)
closeBtn.Position = UDim2.new(1, -40, 0, 0)
closeBtn.BackgroundTransparency = 1
closeBtn.Text = "X"
closeBtn.TextColor3 = Color3.fromRGB(200, 50, 50)
closeBtn.TextSize = 18
closeBtn.Font = Enum.Font.SourceSansBold
closeBtn.Parent = titleBar

closeBtn.MouseButton1Click:Connect(function()
    screenGui:Destroy()
end)

-- チャット履歴表示エリア
local chatScroll = Instance.new("ScrollingFrame")
chatScroll.Size = UDim2.new(1, -20, 1, -110)
chatScroll.Position = UDim2.new(0, 10, 0, 50)
chatScroll.BackgroundTransparency = 1
chatScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
chatScroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
chatScroll.ScrollBarThickness = 6
chatScroll.Parent = mainFrame

local uiListLayout = Instance.new("UIListLayout")
uiListLayout.SortOrder = Enum.SortOrder.LayoutOrder
uiListLayout.Padding = UDim.new(0, 8)
uiListLayout.Parent = chatScroll

-- メッセージを追加する関数
local function addMessage(sender, text, isAI)
    local msgLabel = Instance.new("TextLabel")
    msgLabel.Size = UDim2.new(1, 0, 0, 0)
    msgLabel.AutomaticSize = Enum.AutomaticSize.Y
    msgLabel.BackgroundTransparency = 1
    msgLabel.Text = sender .. ": " .. text
    msgLabel.TextColor3 = isAI and Color3.fromRGB(100, 200, 255) or Color3.fromRGB(255, 255, 255)
    msgLabel.TextSize = 14
    msgLabel.Font = Enum.Font.SourceSans
    msgLabel.TextXAlignment = Enum.TextXAlignment.Left
    msgLabel.TextWrapped = true
    msgLabel.Parent = chatScroll
end

-- 初期メッセージ
addMessage("zerohub", "こんにちは！何かお手伝いできることはありますか？", true)

-- 入力欄の背景
local inputFrame = Instance.new("Frame")
inputFrame.Size = UDim2.new(1, -20, 0, 40)
inputFrame.Position = UDim2.new(0, 10, 1, -50)
inputFrame.BackgroundColor3 = Color3.fromRGB(45, 45, 60)
inputFrame.Parent = mainFrame

local inputCorner = Instance.new("UICorner")
inputCorner.CornerRadius = UDim.new(0, 8)
inputCorner.Parent = inputFrame

-- テキストボックス
local textBox = Instance.new("TextBox")
textBox.Size = UDim2.new(1, -15, 1, 0)
textBox.Position = UDim2.new(0, 10, 0, 0)
textBox.BackgroundTransparency = 1
textBox.PlaceholderText = "メッセージを入力..."
textBox.PlaceholderColor3 = Color3.fromRGB(150, 150, 150)
textBox.Text = ""
textBox.TextColor3 = Color3.fromRGB(255, 255, 255)
textBox.TextSize = 14
textBox.Font = Enum.Font.SourceSans
textBox.TextXAlignment = Enum.TextXAlignment.Left
textBox.Parent = inputFrame

-- 送信時の処理
textBox.FocusLost:Connect(function(enterPressed)
    if enterPressed and textBox.Text ~= "" then
        local userText = textBox.Text
        addMessage("あなた", userText, false)
        textBox.Text = ""
        
        -- AIの簡易返答シミュレーション（ここを外部API連携に書き換え可能）
        task.wait(0.5)
        addMessage("zerohub", "「" .. userText .. "」ですね。承知いたしました！", true)
    end
end)
