-- Rayfield UI 初期化
local Rayfield = loadstring(game:HttpGet('https://raw.githubusercontent.com/UI-Interface/CustomFIeld/main/RayField.lua'))()

-- GUI ウィンドウ作成
local Window = Rayfield:CreateWindow({
    Name = "じんじんgui",
    LoadingTitle = "ずぃんずぃん",
    ConfigurationSaving = { Enabled = true, FolderName = "zinzin", FileName = "zinzin" },
    KeySystem = false
})
local Tab = Window:CreateTab("メイン機能", 4483362458)

-- ESP 設定
local ESPEnabled, ESPColor = false, Color3.new(1, 0, 0)
local ESPInstances = {}
local function ToggleESP(state)
    ESPEnabled = state
    for _, e in ipairs(ESPInstances) do e:Destroy() end
    table.clear(ESPInstances)
    if ESPEnabled then
        for _, p in ipairs(game.Players:GetPlayers()) do
            if p ~= game.Players.LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                local gui = Instance.new("BillboardGui", p.Character.HumanoidRootPart)
                gui.Size = UDim2.new(0,100,0,40)
                gui.Adornee = p.Character.HumanoidRootPart
                gui.AlwaysOnTop = true
                local txt = Instance.new("TextLabel", gui)
                txt.Size = UDim2.new(1,0,1,0)
                txt.BackgroundTransparency = 1
                txt.Text = p.Name
                txt.TextColor3 = ESPColor
                table.insert(ESPInstances, gui)
            end
        end
    end
end
Tab:CreateToggle({ Name = "ESP", Info = "なぜかミエール魔法", CurrentValue = false, Callback = ToggleESP })
Tab:CreateColorPicker({ Name = "ESPの色", Color = ESPColor, Callback = function(c)
    ESPColor = c if ESPEnabled then ToggleESP(false) ToggleESP(true) end
end })

-- Fly 機能
Tab:CreateButton({ Name = "Flyを起動", Callback = function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/XNEOFF/FlyGuiV3/main/FlyGuiV3.txt"))()
end })

-- JumpPower スライダー
Tab:CreateSlider({ Name = "カンガルー", Range = {1,1000000}, Increment = 1, CurrentValue = 50, Suffix = "戦闘力", Callback = function(v)
    local pl = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid")
    if pl then pl.JumpPower = v end
end })

-- WalkSpeed スライダー
Tab:CreateSlider({ Name = "50m走が早くなる魔法", Range = {1,10000000}, Increment = 1, CurrentValue = 16, Suffix = "FXに投資", Callback = function(v)
    local pl = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid")
    if pl then pl.WalkSpeed = v end
end })

-- Aimbot機能
local AimbotRunning = false
local RunService = game:GetService("RunService")
local function GetClosestPlayerInSight()
    local cam = workspace.CurrentCamera
    local me = game.Players.LocalPlayer
    local closest, dist = nil, math.huge
    for _, p in ipairs(game.Players:GetPlayers()) do
        if p~=me and p.Character and p.Character:FindFirstChild("Head") then
            local pos, on = cam:WorldToScreenPoint(p.Character.Head.Position)
            local dir = (p.Character.Head.Position - cam.CFrame.Position).Unit
            if on and dir:Dot(cam.CFrame.LookVector)>0.95 then
                local d = (p.Character.Head.Position - cam.CFrame.Position).Magnitude
                if d<dist then dist, closest = d, p end
            end
        end
    end
    return closest
end
local function UpdateAimbot()
    if AimbotRunning then
        RunService:BindToRenderStep("Aimbot", Enum.RenderPriority.Camera.Value+1, function()
            local t = GetClosestPlayerInSight()
            if t and t.Character and t.Character:FindFirstChild("Head") then
                workspace.CurrentCamera.CFrame = CFrame.new(workspace.CurrentCamera.CFrame.Position, t.Character.Head.Position)
            end
        end)
    else
        RunService:UnbindFromRenderStep("Aimbot")
    end
end
Tab:CreateToggle({ Name = "目線固定の魔法", Info = "近くの敵の頭を狙う", CurrentValue = false, Callback = function(v)
    AimbotRunning = v UpdateAimbot()
end })

-- NoClip 機能
Tab:CreateButton({ Name = "NoClip起動", Callback = function()
    local p = game.Players.LocalPlayer; local c = p.Character or p.CharacterAdded:Wait()
    local enabled = false
    local function toggle()
        enabled = not enabled
        for _, part in ipairs(c:GetDescendants()) do
            if part:IsA("BasePart") then part.CanCollide = not enabled end
        end
    end
    toggle()
end })

-- Infinite Yield
Tab:CreateButton({ Name = "Infinite Yield起動", Callback = function()
    loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()
end })

-- DeadRails
Tab:CreateButton({ Name = "DeadRails起動", Callback = function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/gumanba/Scripts/main/DeadRails"))()
end })

-- Luarmor
Tab:CreateButton({ Name = "ganteng起動", Callback = function()
    loadstring(game:HttpGet("https://api.luarmor.net/files/v3/loaders/a5c3af437cd698d64379cf75cacb9281.lua"))()
end })

-- Infinite Jump 機能
local inf = false
local humanoid = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:WaitForChild("Humanoid")
Tab:CreateButton({ Name = "無限ジャンプ切替", Info = "ON/OFF切替", Callback = function()
    inf = not inf
    humanoid.UseJumpPower = true
    if inf then
        humanoid.JumpPower = humanoid.JumpPower -- 保持
        game:GetService("UserInputService").JumpRequest:Connect(function()
            if inf then humanoid:ChangeState(Enum.HumanoidStateType.Jumping) end
        end)
    end
end })
Tab:CreateButton({ Name = "Hypershot", Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/tbao143/thaibao/refs/heads/main/TbaoHubHypershot"))()
end })
Tab:CreateButton({ Name = "シェーダー影モッド", Callback = function()
        loadstring(game:HttpGet('https://raw.githubusercontent.com/randomstring0/pshade-ultimate/refs/heads/main/src/cd.lua'))()
end })
Tab:CreateButton({ Name = "最強の戦場", Callback = function()
             loadstring(game:HttpGet("https://raw.githubusercontent.com/DiosDi/VexonHub/refs/heads/main/VexonHub"))() 
end })
Tab:CreateButton({ Name = "バレーボールレジェンド", Callback = function()
 loadstring(game:HttpGet("https://rawscripts.net/raw/UPD-Volleyball-Legends-Zeckhubv1-44166"))()
end })
Tab:CreateButton({ Name = "R18＋", Callback = function()
 loadstring(game:HttpGet("https://pastebin.com/raw/FWwdST5Y"))() 
end })
Tab:CreateButton({ Name = "raket rival", Callback = function()
       loadstring(game:HttpGet('https://raw.githubusercontent.com/ScriptsRBXdotCom/scripts/refs/heads/main/racketrivals'))()
end })
Tab:CreateButton({ Name = "最強の戦場 forsaken", Callback = function()
       loadstring(game:HttpGet("https://rawscripts.net/raw/The-Strongest-Battlegrounds-Mafioso-moveset-42115"))()
end })
Tab:CreateButton({ Name = "brainrot 自作", Callback = function()
       loadstring(game:HttpGet("https://raw.githubusercontent.com/jinnpo382/brainrot-script/refs/heads/main/README.md"))()
end })
Tab:CreateButton({ Name = "cut tree key:ArchHub", Callback = function()
       loadstring(game:HttpGet("http://site33927.web1.titanaxe.com/loader.lua"))()
end })
Tab:CreateButton({ Name = "99日生き残る", Callback = function()
       loadstring(game:HttpGet("https://raw.githubusercontent.com/VapeVoidware/VW-Add/main/nightsintheforest.lua", true))()
end })
Tab:CreateButton({ Name = "anti AFK", Callback = function()
       loadstring(game:HttpGet("https://raw.githubusercontent.com/hassanxzayn-lua/Anti-afk/main/antiafkbyhassanxzyn"))()
end })
Tab:CreateButton({ Name = "plant vs brainrot", Callback = function()
       loadstring(game:HttpGet("https://raw.githubusercontent.com/tbao143/thaibao/refs/heads/main/TbaoHubPlantsVsBrainrots"))()
end })
Tab:CreateButton({ Name = "forsaken", Callback = function()
       loadstring(game:HttpGet("https://rawscripts.net/raw/Forsaken-B0bby-hub-30755"))()
end })
Tab:CreateButton({ Name = "bedwars", Callback = function()
      loadstring(game:HttpGet("https://raw.githubusercontent.com/VapeVoidware/VWPacket/main/NewMainScript.lua", true))()
end })

end })
Tab:CreateButton({ Name = "piano ", Callback = function()
       loadstring(game:HttpGet("https://raw.githubusercontent.com/hellohellohell012321/TALENTLESS/main/TALENTLESS", true))()
end })
Tab:CreateButton({ Name = "ビルボ ", Callback = function()
       loadstring(game:HttpGet("https://raw.githubusercontent.com/suntisalts/WeshkyHub/refs/heads/main/MainLoader.lua"))()
end })
Tab:CreateButton({ Name = "grow a gaden ", Callback = function()
       loadstring(game:HttpGet("https://raw.githubusercontent.com/NittarPP/PhotonScript/refs/heads/main/Loading/Loading.lua"))()
end })
Tab:CreateButton({ Name = "裏庭を掘る ", Callback = function()
       loadstring(game:HttpGet("https://raw.githubusercontent.com/Bac0nHck/Scripts/refs/heads/main/DigtheBackyard.lua"))('t.me/arceusxscripts')
end })
