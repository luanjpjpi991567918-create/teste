local p,rs,ts,vu = game.Players.LocalPlayer, game:GetService("RunService"), game:GetService("TweenService"), game:GetService("VirtualUser")
local pg, fAtivo, alt, cfK, vel = p:WaitForChild("PlayerGui"), false, 2000, nil, 950

-- ANTI-AFK (Não cai do jogo)
p.Idled:Connect(function() vu:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame) task.wait(1) vu:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame) end)

if pg:FindFirstChild("LuazinMenu") then pg.LuazinMenu:Destroy() end

local sg = Instance.new("ScreenGui", pg)
sg.Name = "LuazinMenu"
sg.ResetOnSpawn = false

-- --- TELA DE LOADING --- --
local ld = Instance.new("Frame", sg)
ld.Size, ld.BackgroundColor3, ld.ZIndex = UDim2.new(1, 0, 1, 0), Color3.fromRGB(10, 10, 10), 10

local loadTitle = Instance.new("TextLabel", ld)
loadTitle.Size = UDim2.new(0, 300, 0, 30)
loadTitle.Position = UDim2.new(0.5, -150, 0.48, -15)
loadTitle.Text = "o mais brabo" -- O nome que você pediu
loadTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
loadTitle.Font = Enum.Font.GothamBold
loadTitle.TextSize = 16 
loadTitle.BackgroundTransparency = 1

local barBg = Instance.new("Frame", ld)
barBg.Size, barBg.Position, barBg.BackgroundColor3 = UDim2.new(0, 200, 0, 4), UDim2.new(0.5, -100, 0.53, 0), Color3.fromRGB(30, 30, 30)
barBg.BorderSizePixel = 0
Instance.new("UICorner", barBg)

local barFill = Instance.new("Frame", barBg)
barFill.Size, barFill.BackgroundColor3 = UDim2.new(0, 0, 1, 0), Color3.fromRGB(0, 255, 120)
Instance.new("UICorner", barFill)

-- --- MENU PRINCIPAL (TAMANHO NORMAL) --- --
local main = Instance.new("Frame", sg)
main.Size = UDim2.new(0, 180, 0, 100)
main.Position = UDim2.new(0.05, 0, 0.4, 0)
main.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
main.Visible = false
main.Active = true
main.Draggable = true 
Instance.new("UICorner", main).CornerRadius = UDim.new(0, 10)

local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1, 0, 0, 35)
title.Text = "LUAZIN AUTO FARM"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.GothamBold
title.TextSize = 14
title.BackgroundTransparency = 1

local btn = Instance.new("TextButton", main)
btn.Size = UDim2.new(0.85, 0, 0, 45)
btn.Position = UDim2.new(0.075, 0, 0.45, 0)
btn.Text = "FARM: OFF"
btn.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
btn.TextColor3 = Color3.new(1, 1, 1)
btn.Font = Enum.Font.GothamSemibold
btn.TextSize = 13
Instance.new("UICorner", btn)

-- Animação
task.spawn(function()
    ts:Create(barFill, TweenInfo.new(2, Enum.EasingStyle.Quart), {Size = UDim2.new(1, 0, 1, 0)}):Play()
    task.wait(2.2)
    ld:Destroy()
    main.Visible = true
end)

-- Lógica
btn.MouseButton1Click:Connect(function()
    fAtivo = not fAtivo
    local s = p.Character and p.Character:FindFirstChildWhichIsA("Humanoid") and p.Character.Humanoid.SeatPart
    if fAtivo and s and s:IsA("VehicleSeat") then
        btn.Text, btn.BackgroundColor3 = "FARM: ON", Color3.fromRGB(0, 180, 100)
        cfK = CFrame.new(s.Position.X, alt, s.Position.Z) * s.CFrame.Rotation
    else
        fAtivo, btn.Text, btn.BackgroundColor3, cfK = false, "FARM: OFF", Color3.fromRGB(180, 0, 0), nil
    end
end)

rs.Heartbeat:Connect(function()
    if not fAtivo or not cfK then return end
    local s = p.Character and p.Character:FindFirstChildWhichIsA("Humanoid") and p.Character.Humanoid.SeatPart
    if s and s:IsA("VehicleSeat") then
        s.CFrame, s.Velocity, s.RotVelocity, s.Throttle = cfK, s.CFrame.LookVector * vel, Vector3.new(0,0,0), 1
        for _,v in pairs(s.Parent:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide = false end end
    else fAtivo, btn.Text, btn.BackgroundColor3 = false, "FARM: OFF", Color3.fromRGB(180, 0, 0) end
end)
