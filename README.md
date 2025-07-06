--[[
    💫METEOR_HUB🌟
    Version 6: Restored Emoji Logo
    Dibuat oleh: rip_kompos
]]

--// Services
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

--// Player & GUI
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

--// Events
local GameEvents = ReplicatedStorage:WaitForChild("GameEvents")

--// Configuration
local seedList = {
    "Apple", "Carrot", "Strawberry", "Blueberry", "Watermelon", "Coconut", "Mushroom", "Mango", "Cocoa", "Tomato",
    "Sugar Apple", "Ember Lily", "Orange Tulip", "Daffodil", "Burning Bud", "Bamboo", "Beanstalk", "Grape", "Cactus", "Pumpkin", "Dragon Fruit"
}

local gearList = {
    "Master Sprinkler", "Harvest Tool", "Favorite Tool", "Watering Can",
    "Advanced Sprinkler", "Basic Sprinkler", "Tanning Mirror", "Lightning Rod",
    "Cleaning Spray", "Trowel", "Recall Wrench", "Magnifying Glass", "godly sprinkler"
}

local collectSeeds = {"Tomato Fruit", "Strawberry Fruit", "blueberry Fruit"}

--// Main UI Creation
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "💫METEOR_HUB🌟"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = PlayerGui

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 550, 0, 420)
MainFrame.Position = UDim2.new(0.5, -275, 0.5, -210)
MainFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
MainFrame.BorderColor3 = Color3.fromRGB(80, 80, 100)
MainFrame.BorderSizePixel = 1
MainFrame.Visible = true
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner", MainFrame)
UICorner.CornerRadius = UDim.new(0, 8)

--// Top Bar
local TopBar = Instance.new("Frame")
TopBar.Name = "TopBar"
TopBar.Size = UDim2.new(1, 0, 0, 35)
TopBar.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
TopBar.BorderSizePixel = 0
TopBar.Parent = MainFrame

local TopBarCorner = Instance.new("UICorner", TopBar)
TopBarCorner.CornerRadius = UDim.new(0, 8)

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Name = "TitleLabel"
TitleLabel.Size = UDim2.new(1, -70, 1, 0)
TitleLabel.Position = UDim2.new(0, 15, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.Text = "💫METEOR_HUB🌟 [v6]" -- [DIUBAH] Versi diperbarui
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.TextSize = 16
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
TitleLabel.Parent = TopBar

local CloseButton = Instance.new("TextButton")
CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, 25, 0, 25)
CloseButton.Position = UDim2.new(1, -35, 0.5, -12.5)
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
CloseButton.Font = Enum.Font.GothamBold
CloseButton.Text = "X"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.TextSize = 14
CloseButton.Parent = TopBar
local CloseButtonCorner = Instance.new("UICorner", CloseButton)
CloseButtonCorner.CornerRadius = UDim.new(0, 6)

--// Navigation Bar
local NavBar = Instance.new("Frame")
NavBar.Name = "NavBar"
NavBar.Size = UDim2.new(0, 150, 1, -40)
NavBar.Position = UDim2.new(0, 0, 0, 40)
NavBar.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
NavBar.BorderSizePixel = 0
NavBar.Parent = MainFrame

local NavLayout = Instance.new("UIListLayout", NavBar)
NavLayout.Padding = UDim.new(0, 10)
NavLayout.SortOrder = Enum.SortOrder.LayoutOrder
NavLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center

--// Pages Container
local PagesContainer = Instance.new("Frame")
PagesContainer.Name = "PagesContainer"
PagesContainer.Size = UDim2.new(1, -160, 1, -45)
PagesContainer.Position = UDim2.new(0, 155, 0, 40)
PagesContainer.BackgroundTransparency = 1
PagesContainer.BorderSizePixel = 0
PagesContainer.Parent = MainFrame

--// Page Creation Logic
local pages = {}
local function createPage(name)
    local page = Instance.new("ScrollingFrame")
    page.Name = name .. "Page"
    page.Size = UDim2.new(1, 0, 1, 0)
    page.BackgroundTransparency = 1
    page.BorderSizePixel = 0
    page.Visible = false
    page.Parent = PagesContainer
    page.CanvasSize = UDim2.new(0, 0, 0, 0)
    page.ScrollBarImageColor3 = Color3.fromRGB(150, 150, 150)
    page.ScrollBarThickness = 5

    local layout = Instance.new("UIListLayout", page)
    layout.Padding = UDim.new(0, 8)
    layout.SortOrder = Enum.SortOrder.LayoutOrder
    layout.HorizontalAlignment = Enum.HorizontalAlignment.Center

    pages[name] = page
    return page
end

--// Create Pages
local homePage = createPage("Home")
local mainPage = createPage("Main")
local autoPage = createPage("Otomatis")
local miscPage = createPage("Misc")
local visualPage = createPage("Visual")
local uiSettingsPage = createPage("Pengaturan UI")

--// Navigation Logic
local function switchPage(name)
    for pageName, page in pairs(pages) do
        page.Visible = (pageName == name)
    end
    for _, button in ipairs(NavBar:GetChildren()) do
        if button:IsA("TextButton") then
            local targetColor = (button.Name == name) and Color3.fromRGB(80, 80, 255) or Color3.fromRGB(50, 50, 60)
            TweenService:Create(button, TweenInfo.new(0.2), {BackgroundColor3 = targetColor}):Play()
        end
    end
end

--// Navigation Button Creation
local function createNavButton(name, order)
    local button = Instance.new("TextButton")
    button.Name = name
    button.LayoutOrder = order
    button.Size = UDim2.new(1, -10, 0, 30)
    button.BackgroundColor3 = (name == "Home") and Color3.fromRGB(80, 80, 255) or Color3.fromRGB(50, 50, 60)
    button.Font = Enum.Font.Gotham
    button.Text = name
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.TextSize = 14
    button.Parent = NavBar

    local corner = Instance.new("UICorner", button)
    corner.CornerRadius = UDim.new(0, 6)

    button.MouseButton1Click:Connect(function()
        switchPage(name)
    end)
    return button
end

createNavButton("Home", 0)
createNavButton("Main", 1)
createNavButton("Otomatis", 2)
createNavButton("Misc", 3)
createNavButton("Visual", 4)
createNavButton("Pengaturan UI", 5)

--// Function to dynamically update ScrollingFrame CanvasSize
local function updateCanvasSize(page)
    task.wait() 
    local layout = page:FindFirstChildOfClass("UIListLayout")
    if not layout then return end

    local absoluteContentSize = layout.AbsoluteContentSize
    page.CanvasSize = UDim2.new(0, 0, 0, absoluteContentSize.Y + layout.Padding.Offset * 2)
end

--// Element Creation Functions
local function createLabel(parentPage, text)
    local label = Instance.new("TextLabel")
    label.Name = "InfoLabel"
    label.Size = UDim2.new(1, -10, 0, 25)
    label.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
    label.Font = Enum.Font.Gotham
    label.Text = text
    label.TextColor3 = Color3.fromRGB(220, 220, 220)
    label.TextSize = 14
    label.Parent = parentPage
    
    local corner = Instance.new("UICorner", label)
    corner.CornerRadius = UDim.new(0, 6)
    
    updateCanvasSize(parentPage)
    return label
end

local function createToggle(parentPage, labelText, callback)
    local enabled = false

    local button = Instance.new("TextButton")
    button.Name = labelText
    button.Size = UDim2.new(1, -10, 0, 35)
    button.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
    button.Font = Enum.Font.Gotham
    button.Text = ""
    button.Parent = parentPage
    
    local corner = Instance.new("UICorner", button)
    corner.CornerRadius = UDim.new(0, 6)

    local label = Instance.new("TextLabel", button)
    label.Size = UDim2.new(0.7, 0, 1, 0)
    label.Position = UDim2.new(0, 10, 0, 0)
    label.BackgroundTransparency = 1
    label.Font = Enum.Font.Gotham
    label.Text = labelText
    label.TextColor3 = Color3.fromRGB(220, 220, 220)
    label.TextSize = 14
    label.TextXAlignment = Enum.TextXAlignment.Left

    local status = Instance.new("TextLabel", button)
    status.Size = UDim2.new(0.25, 0, 1, 0)
    status.Position = UDim2.new(0.75, -10, 0, 0)
    status.BackgroundTransparency = 1
    status.Font = Enum.Font.GothamBold
    status.Text = "OFF"
    status.TextColor3 = Color3.fromRGB(255, 80, 80)
    status.TextSize = 14
    status.TextXAlignment = Enum.TextXAlignment.Right
    
    button.MouseButton1Click:Connect(function()
        enabled = not enabled
        status.Text = enabled and "ON" or "OFF"
        status.TextColor3 = enabled and Color3.fromRGB(80, 255, 80) or Color3.fromRGB(255, 80, 80)
        pcall(callback, enabled)
    end)
    
    updateCanvasSize(parentPage)
end

local function createButton(parentPage, labelText, callback)
    local button = Instance.new("TextButton")
    button.Name = labelText
    button.Size = UDim2.new(1, -10, 0, 35)
    button.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
    button.Font = Enum.Font.Gotham
    button.Text = labelText
    button.TextColor3 = Color3.fromRGB(220, 220, 220)
    button.TextSize = 14
    button.Parent = parentPage
    
    local corner = Instance.new("UICorner", button)
    corner.CornerRadius = UDim.new(0, 6)
    
    button.MouseButton1Click:Connect(function()
		pcall(callback, button)
	end)
    
    updateCanvasSize(parentPage)
    return button 
end

--// Player Feature Variables
local noclipConnection, infJumpConnection, flyConnection = nil, nil, nil
local flyBodyVelocity, flyBodyGyro = nil, nil
local originalGravity = workspace.Gravity

--// Populate Pages with Features

--## HOME PAGE ##--
createLabel(homePage, "Dibuat oleh: rip_kompos")
local playerCountLabel = createLabel(homePage, "Pemain di Server: " .. #Players:GetPlayers())
Players.PlayerAdded:Connect(function() playerCountLabel.Text = "Pemain di Server: " .. #Players:GetPlayers() end)
Players.PlayerRemoving:Connect(function() playerCountLabel.Text = "Pemain di Server: " .. #Players:GetPlayers() end)

local speedFrame = Instance.new("Frame", homePage)
speedFrame.Size, speedFrame.BackgroundTransparency = UDim2.new(1, -10, 0, 45), 1
local speedLayout = Instance.new("UIListLayout", speedFrame)
speedLayout.FillDirection, speedLayout.Padding = Enum.FillDirection.Horizontal, UDim.new(0, 5)
local speedInput = Instance.new("TextBox", speedFrame)
speedInput.Size, speedInput.BackgroundColor3, speedInput.TextColor3, speedInput.PlaceholderText, speedInput.Font, speedInput.TextSize = UDim2.new(0.7, -5, 1, 0), Color3.fromRGB(35, 35, 45), Color3.fromRGB(255, 255, 255), "Kecepatan Lari (Default: 16)", Enum.Font.Gotham, 14
local speedInputCorner = Instance.new("UICorner", speedInput)
speedInputCorner.CornerRadius = UDim.new(0, 4)
local speedButton = Instance.new("TextButton", speedFrame)
speedButton.Size, speedButton.BackgroundColor3, speedButton.Font, speedButton.Text, speedButton.TextColor3, speedButton.TextSize = UDim2.new(0.3, 0, 1, 0), Color3.fromRGB(80, 80, 255), Enum.Font.GothamBold, "Atur", Color3.fromRGB(255, 255, 255), 14
local speedButtonCorner = Instance.new("UICorner", speedButton)
speedButtonCorner.CornerRadius = UDim.new(0, 6)
speedButton.MouseButton1Click:Connect(function()
    local speed = tonumber(speedInput.Text)
    if speed and LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = speed
    end
end)
updateCanvasSize(homePage)

createToggle(homePage, "Terbang (Fly)", function(enabled)
    local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    local rootPart = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if not humanoid or not rootPart then return end
    if enabled then
        flyBodyGyro = Instance.new("BodyGyro", rootPart)
        flyBodyGyro.MaxTorque, flyBodyGyro.CFrame = Vector3.new(9e9, 9e9, 9e9), rootPart.CFrame
        flyBodyVelocity = Instance.new("BodyVelocity", rootPart)
        flyBodyVelocity.MaxForce, flyBodyVelocity.Velocity = Vector3.new(9e9, 9e9, 9e9), Vector3.new(0, 0, 0)
        flyConnection = RunService.RenderStepped:Connect(function()
            local speed, velocity = 50, Vector3.new(0,0,0)
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then velocity += (rootPart.CFrame.LookVector * speed) end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then velocity -= (rootPart.CFrame.LookVector * speed) end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then velocity += (rootPart.CFrame.RightVector * speed) end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then velocity -= (rootPart.CFrame.RightVector * speed) end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then velocity += Vector3.new(0, speed, 0) end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then velocity -= Vector3.new(0, speed, 0) end
            flyBodyGyro.CFrame = rootPart.CFrame
            flyBodyVelocity.Velocity = velocity
        end)
    else
        if flyConnection then flyConnection:Disconnect() end
        if flyBodyGyro then flyBodyGyro:Destroy() end
        if flyBodyVelocity then flyBodyVelocity:Destroy() end
        flyConnection, flyBodyGyro, flyBodyVelocity = nil, nil, nil
    end
end)

createToggle(homePage, "Noclip", function(enabled)
    if enabled then
        noclipConnection = RunService.Stepped:Connect(function() if LocalPlayer.Character then for _, part in ipairs(LocalPlayer.Character:GetDescendants()) do if part:IsA("BasePart") then part.CanCollide = false end end end end)
    else
        if noclipConnection then noclipConnection:Disconnect(); noclipConnection = nil end
    end
end)

createToggle(homePage, "Lompat Tanpa Batas", function(enabled)
    local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if not humanoid then return end
    if enabled then
        infJumpConnection = UserInputService.JumpRequest:Connect(function() humanoid:ChangeState(Enum.HumanoidStateType.Jumping) end)
    else
        if infJumpConnection then infJumpConnection:Disconnect(); infJumpConnection = nil end
    end
end)

createToggle(homePage, "Anti Gravitasi", function(enabled) workspace.Gravity = enabled and 0 or originalGravity end)

--## MAIN PAGE ##--
createToggle(mainPage, "Auto Harvest", function() for _,p in ipairs(workspace:GetDescendants()) do if p:IsA("Model") and table.find(seedList, p.Name) then pcall(GameEvents.HarvestRemote.InvokeServer, GameEvents.HarvestRemote, p) end end end)
createToggle(mainPage, "Auto Trowel", function() local pos = LocalPlayer.Character and LocalPlayer.Character.HumanoidRootPart.Position if pos then for _,p in ipairs(workspace:GetDescendants()) do if p:IsA("Model") and table.find(seedList, p.Name) then pcall(GameEvents.TrowelRemote.InvokeServer, GameEvents.TrowelRemote, p, pos) end end end end)
createToggle(mainPage, "Auto Collect", function() for _,s in ipairs(collectSeeds) do GameEvents.SpawnCollectableSeed:FireServer(s) end end)
createToggle(mainPage, "Claim Dino Quest", function() GameEvents.ClaimDinoQuest:InvokeServer() end)
createButton(mainPage, "❤️ Like Semua Garden", function(b) local f=GameEvents.LikeGarden for _,p in ipairs(Players:GetPlayers()) do if p~=LocalPlayer then pcall(f.InvokeServer,f,p) task.wait() end end b.Text="✅ Sudah Like Semua!"; task.wait(2); b.Text="❤️ Like Semua Garden" end)

--## OTOMATIS PAGE ##--
createToggle(autoPage, "Auto Buy All Seeds", function() for _,s in ipairs(seedList) do GameEvents.BuySeedStock:FireServer(s, 1) end end)
createToggle(autoPage, "Auto Buy All Gear", function() for _,g in ipairs(gearList) do GameEvents.BuyGearStock:FireServer(g, 1) end end)
createToggle(autoPage, "Auto Claim Fossil", function() pcall(function() fireproximityprompt(workspace.Interaction.UpdateItems.DinoEvent.DNAmachine.SideTable:GetChildren()[4].ProxPromptPart.FossilClaimProximityPrompt) end) end)

--## MISC PAGE ##--
createButton(miscPage, "Meteor Shower", function() GameEvents.MeteorShower:FireServer() end)
createButton(miscPage, "Meteor Strike", function() GameEvents.MeteorStrike:FireServer() end)
createButton(miscPage, "Blackhole Weather", function() GameEvents.SendClientWeatherEvents:FireServer("blackhole", "send") end)
local likeChangerFrame = Instance.new("Frame", miscPage)
likeChangerFrame.Name, likeChangerFrame.Size, likeChangerFrame.BackgroundColor3, likeChangerFrame.BorderSizePixel, likeChangerFrame.LayoutOrder = "LikeChangerContainer", UDim2.new(1, -10, 0, 105), Color3.fromRGB(45, 45, 55), 0, 4
local likeChangerCorner = Instance.new("UICorner", likeChangerFrame)
likeChangerCorner.CornerRadius = UDim.new(0, 6)
local likeLabel = Instance.new("TextLabel", likeChangerFrame)
likeLabel.Size, likeLabel.Position, likeLabel.BackgroundTransparency, likeLabel.Font, likeLabel.Text, likeLabel.TextColor3, likeLabel.TextSize, likeLabel.TextXAlignment = UDim2.new(1, -20, 0, 20), UDim2.new(0, 10, 0, 5), 1, Enum.Font.Gotham, "Ganti Jumlah Like Papan", Color3.fromRGB(220, 220, 220), 14, Enum.TextXAlignment.Left
local likeTextBox = Instance.new("TextBox", likeChangerFrame)
likeTextBox.Name, likeTextBox.Size, likeTextBox.Position, likeTextBox.BackgroundColor3, likeTextBox.TextColor3, likeTextBox.PlaceholderText, likeTextBox.ClearTextOnFocus, likeTextBox.Font, likeTextBox.TextSize = "LikeInput", UDim2.new(1, -20, 0, 30), UDim2.new(0, 10, 0, 30), Color3.fromRGB(35, 35, 45), Color3.fromRGB(255, 255, 255), "Contoh: 99999", false, Enum.Font.Gotham, 14
local likeTextBoxCorner = Instance.new("UICorner", likeTextBox)
likeTextBoxCorner.CornerRadius = UDim.new(0, 4)
local likeButton = Instance.new("TextButton", likeChangerFrame)
likeButton.Name, likeButton.Size, likeButton.Position, likeButton.BackgroundColor3, likeButton.Font, likeButton.Text, likeButton.TextColor3, likeButton.TextSize = "ApplyLikes", UDim2.new(1, -20, 0, 30), UDim2.new(0, 10, 0, 65), Color3.fromRGB(0, 170, 0), Enum.Font.GothamBold, "Ganti Like!", Color3.fromRGB(255, 255, 255), 14
local likeButtonCorner = Instance.new("UICorner", likeButton)
likeButtonCorner.CornerRadius = UDim.new(0, 6)
likeButton.MouseButton1Click:Connect(function()
	if tonumber(likeTextBox.Text) == nil then return end
	pcall(function() workspace.Farm.Farm.Sign.Core_Part.SurfaceGui.Player.Likes.Text = (workspace.Farm.Farm.Sign.Core_Part.SurfaceGui.Player.Likes.Text:match("^([^%d%s]+)") or "👍") .. " " .. likeTextBox.Text end)
end)
updateCanvasSize(miscPage)

--## VISUAL PAGE ##--
createButton(visualPage, "Dupe Item Visual", function(b) local t = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Tool") or LocalPlayer.Backpack:FindFirstChildOfClass("Tool") if t then for i=1,5 do t:Clone().Parent=LocalPlayer.Backpack end b.Text="✅ Berhasil" else b.Text="❌ Tidak Ada Item" end task.wait(2) b.Text="Dupe Item Visual" end)

--## PENGATURAN UI PAGE ##--
createButton(uiSettingsPage, "Toggle Dino UI", function() local ui=PlayerGui:FindFirstChild("DinoQuests_UI") if ui then ui.Enabled=not ui.Enabled end end)
createButton(uiSettingsPage, "Toggle Gear Shop", function() local ui=PlayerGui:FindFirstChild("Gear_Shop") if ui then ui.Enabled=not ui.Enabled end end)
createButton(uiSettingsPage, "Toggle Seed Shop", function() local ui=PlayerGui:FindFirstChild("Seed_Shop") if ui then ui.Enabled=not ui.Enabled end end)
createButton(uiSettingsPage, "Toggle Cosmetic Shop", function() local ui=PlayerGui:FindFirstChild("CosmeticShop_UI") if ui then ui.Enabled=not ui.Enabled end end)

--// Final Setup
switchPage("Home")
CloseButton.MouseButton1Click:Connect(function() ScreenGui:Destroy(); workspace.Gravity = originalGravity end)

--// Toggle Button to Show/Hide MainFrame
local ToggleHubButton = Instance.new("TextButton")
ToggleHubButton.Name = "ToggleHubButton"
ToggleHubButton.Size = UDim2.new(0, 40, 0, 40)
ToggleHubButton.Position = UDim2.new(0, 10, 0.5, -20)
ToggleHubButton.Text = "💫"--[DIPERBAIKI] Emoji

logo dikembalikan

ToggleHubButton.BackgroundColor3 =

Color3.fromRGB(25, 25, 35)

ToggleHubButton.TextColor3 = Color3.new(1, 1, 1)

ToggleHubButton.Font Font = Enum.Font.GothamBold

ToggleHubButton.TextSize = 24

ToggleHubButton. Active = true

ToggleHubButton.Draggable = true

ToggleHubButton.Parent = ScreenGui

local ToggleHubButtonCorner =

Instance.new("UICorner", ToggleHubButton)

ToggleHubButtonCorner.CornerRadius = UDim.new(0,8)

ToggleHubButton. MouseButton1Click: Connect(function()

MainFrame.Visible = not MainFrame. Visible end)
