-- Grow Garden Mobile Tools - By Thanh An (FIXED VERSION)
local player = game:GetService("Players").LocalPlayer
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "GardenMobileTools_V2"
gui.ResetOnSpawn = false

-- Cấu hình mobile
local isMobile = game:GetService("UserInputService").TouchEnabled
local buttonSize = isMobile and UDim2.new(0, 80, 0, 80) or UDim2.new(0, 50, 0, 50)

-- Nút toggle (lớn hơn trên mobile)
local toggle = Instance.new("TextButton", gui)
toggle.Size = buttonSize
toggle.Position = UDim2.new(0, 20, 0, 20)
toggle.Text = isMobile and "🌿 MENU" or "🌿"
toggle.TextSize = isMobile and 20 or 16
toggle.BackgroundColor3 = Color3.fromRGB(72, 163, 84)
toggle.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", toggle).CornerRadius = UDim.new(1, 0)

-- Khung chính
local main = Instance.new("Frame", gui)
main.Size = isMobile and UDim2.new(0, 300, 0, 450) or UDim2.new(0, 250, 0, 380)
main.Position = UDim2.new(0, 100, 0, 30)
main.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
main.Visible = false
Instance.new("UICorner", main).CornerRadius = UDim.new(0, 12)

-- Tạo thanh trượt (tối ưu mobile)
local function createSlider(parent, yPos, title, color)
    local slider = Instance.new("Frame", parent)
    slider.Size = UDim2.new(0.9, 0, 0, 70)
    slider.Position = UDim2.new(0.05, 0, yPos, 0)
    slider.BackgroundTransparency = 1

    local label = Instance.new("TextLabel", slider)
    label.Size = UDim2.new(1, 0, 0, 25)
    label.Text = title..": 50"
    label.TextColor3 = Color3.new(1,1,1)
    label.Font = Enum.Font.SourceSansSemibold
    label.TextSize = 16

    local track = Instance.new("Frame", slider)
    track.Size = UDim2.new(1, 0, 0, 12)
    track.Position = UDim2.new(0, 0, 0, 35)
    track.BackgroundColor3 = Color3.fromRGB(70,70,70)
    Instance.new("UICorner", track).CornerRadius = UDim.new(1,0)

    local fill = Instance.new("Frame", track)
    fill.Size = UDim2.new(0.5, 0, 1, 0)
    fill.BackgroundColor3 = color
    Instance.new("UICorner", fill).CornerRadius = UDim.new(1,0)

    local handle = Instance.new("TextButton", track)
    handle.Size = UDim2.new(0, 24, 0, 24)
    handle.Position = UDim2.new(0.5, -12, 0, -6)
    handle.Text = ""
    handle.BackgroundColor3 = Color3.new(1,1,1)
    Instance.new("UICorner", handle).CornerRadius = UDim.new(1,0)

    -- Xử lý cảm ứng
    local function update(input)
        local pos = (input.Position.X - track.AbsolutePosition.X)/track.AbsoluteSize.X
        pos = math.clamp(pos, 0, 1)
        fill.Size = UDim2.new(pos, 0, 1, 0)
        handle.Position = UDim2.new(pos, -12, 0, -6)
        label.Text = title..": "..math.floor(pos*100)
        return pos
    end

    handle.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
            local conn
            update(input)
            conn = game:GetService("UserInputService").InputChanged:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement then
                    update(input)
                end
            end)
            
            game:GetService("UserInputService").InputEnded:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                    conn:Disconnect()
                end
            end)
        end
    end)
    
    return {track = track, fill = fill, handle = handle}
end

-- Tạo các thanh trượt
local speedSlider = createSlider(main, 0.05, "Tốc độ", Color3.fromRGB(100, 200, 255))
local jumpSlider = createSlider(main, 0.2, "Sức nhảy", Color3.fromRGB(255, 150, 100))

-- Nút đóng
local closeBtn = Instance.new("TextButton", main)
closeBtn.Size = UDim2.new(0.9, 0, 0, 40)
closeBtn.Position = UDim2.new(0.05, 0, 0.85, 0)
closeBtn.Text = "ĐÓNG MENU"
closeBtn.TextSize = 16
closeBtn.BackgroundColor3 = Color3.fromRGB(180, 80, 80)
closeBtn.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", closeBtn).CornerRadius = UDim.new(0, 8)

-- Xử lý toggle
toggle.MouseButton1Click:Connect(function()
    main.Visible = not main.Visible
    toggle.Text = main.Visible and "✕" or (isMobile and "🌿 MENU" or "🌿")
end)

closeBtn.MouseButton1Click:Connect(function()
    main.Visible = false
    toggle.Text = isMobile and "🌿 MENU" or "🌿"
end)

-- Cập nhật thuộc tính
game:GetService("RunService").Heartbeat:Connect(function()
    pcall(function()
        local char = player.Character
        if char and char:FindFirstChildOfClass("Humanoid") then
            char:FindFirstChildOfClass("Humanoid").WalkSpeed = 16 + (speedSlider.fill.AbsoluteSize.X / speedSlider.track.AbsoluteSize.X * 84)
            char:FindFirstChildOfClass("Humanoid").JumpPower = 50 + (jumpSlider.fill.AbsoluteSize.X / jumpSlider.track.AbsoluteSize.X * 150)
        end
    end)
end)

-- Auto-close khi chết
player.CharacterAdded:Connect(function()
    main.Visible = false
    toggle.Text = isMobile and "🌿 MENU" or "🌿"
end)
