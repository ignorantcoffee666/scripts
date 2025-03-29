-- Auto Collect Chests
local function autoCollectChests(state)
    getfenv().chests = state
    local lastCheckTime = tick()
    local allChestsCollected = false
    local originalPosition = nil

    while getfenv().chests do
        -- Check if it's time to verify if all chests are collected
        if tick() - lastCheckTime > 30 or allChestsCollected then
            lastCheckTime = tick()
            allChestsCollected = false
            local chestsAvailable = false
            for _, v in pairs(workspace:GetDescendants()) do
                if v.ClassName == "ProximityPrompt" and v.Parent.Name == "HumanoidRootPart" and not v.Parent.Parent:GetAttribute("OpenParticlesType") and v.Enabled and v.ActionText == "Open" then
                    chestsAvailable = true
                    break
                end
            end
            if not chestsAvailable then
                print("No chests available. Waiting for 30sec.")
                allChestsCollected = true
                wait(30) -- Wait for 30sec before checking again
                lastCheckTime = tick()
            end
        end

        -- Check for available chests
        local chestsAvailable = false
        for _, v in pairs(workspace:GetDescendants()) do
            if v.ClassName == "ProximityPrompt" and v.Parent.Name == "HumanoidRootPart" and not v.Parent.Parent:GetAttribute("OpenParticlesType") and v.Enabled and v.ActionText == "Open" then
                chestsAvailable = true
                break
            end
        end

        if not chestsAvailable then
            print("No chests available. Continuing to wait.")
            wait(1) -- Wait 1 second before checking again
            continue
        end

        -- Collect chests
        local character = game.Players.LocalPlayer.Character
        if character and character:FindFirstChild("HumanoidRootPart") then
            -- Store the original position
            if not originalPosition then
                originalPosition = character.HumanoidRootPart.CFrame
            end

            for _, v in pairs(workspace:GetDescendants()) do
                if v.ClassName == "ProximityPrompt" and v.Parent.Name == "HumanoidRootPart" and not v.Parent.Parent:GetAttribute("OpenParticlesType") and v.Enabled and v.ActionText == "Open" and getfenv().chests then
                    print("Found a chest. Teleporting and opening it.")
                    repeat task.wait()
                        -- Teleport a little above the chest
                        character.HumanoidRootPart.CFrame = v.Parent.CFrame + Vector3.new(0, 4, 0)
                        fireproximityprompt(v)
                    until v.Enabled == false or getfenv().chests == false
                    if not v.Enabled then
                        print("Chest opened successfully.")
                        allChestsCollected = true
                        for _, chest in pairs(workspace:GetDescendants()) do
                            if chest.ClassName == "ProximityPrompt" and chest.Parent.Name == "HumanoidRootPart" and not chest.Parent.Parent:GetAttribute("OpenParticlesType") and chest.Enabled and chest.ActionText == "Open" then
                                allChestsCollected = false
                                break
                            end
                        end
                    end
                end
            end

            -- Return to the original position after collecting all chests
            if allChestsCollected and originalPosition then
                character.HumanoidRootPart.CFrame = originalPosition
                originalPosition = nil
                print("Returned to original position.")
            end
        else
            wait(1) -- Wait for the character to spawn if it doesn't exist yet
        end
    end
end

-- Auto Collect Loot
local function autoCollectLoot(state)
    _G.loot = (state and true or false)
    local character = game.Players.LocalPlayer.Character
    local humanoidRootPart = character and character:FindFirstChild("HumanoidRootPart")
    local collectInterval = 240 -- Seconds
    local collectDuration = 20 -- Seconds
    local lastCollectTime = tick()

    while _G.loot do
        -- Check if it's time to collect loot
        if tick() - lastCollectTime > collectInterval then
            lastCollectTime = tick()
            print("Auto Collect Loot activated for 20 seconds.")
            local startTime = tick()
            while tick() - startTime < collectDuration do
                pcall(function()
                    for i, v in pairs(game:GetService("Workspace").DroppedItems:GetChildren()) do
                        if v.ClassName == "Model" and v.PrimaryPart ~= nil and v.PrimaryPart.Transparency ~= 1 then
                            if humanoidRootPart then
                                -- Teleportar al jugador solo si el item existe y no ha sido recogido
                                if v.Parent and v.PrimaryPart then
                                    humanoidRootPart.CFrame = v.PrimaryPart.CFrame
                                    -- Esperar un breve momento para evitar teleportaciones rápidas
                                    task.wait(0.01)
                                end
                            end
                            -- Chequear si el item ya no existe o si la función debe detenerse
                            if not v.Parent or not v.PrimaryPart or _G.loot == false then
                                break
                            end
                        end
                    end
                end)
                task.wait(0.1) -- Esperar un breve momento entre iteraciones
            end
            print("Auto Collect Loot paused for 240 seconds.")
        else
            task.wait(1) -- Esperar un segundo antes de chequear de nuevo
        end
    end
end

-- Auto Sell Loot
local function autoSellLoot(state)
    _G.sell = (state and true or false)
    while _G.sell do
        game:GetService("ReplicatedStorage").CloudFrameShared.DataStreams.processGameItemSold:InvokeServer("SellEverything")
        wait(1)
    end
end

-- Auto Farm Mobs
local function autoFarmMobs(state)
    _G.test = (state and true or false)
    while _G.test do
        task.wait()
        pcall(function()
            local plr = game.Players.LocalPlayer.UserId
            for i, v in pairs(game:GetService("ReplicatedStorage").ToolsCache[plr]:GetChildren()) do
                if v:GetAttribute("type") == "Spears" then
                    spear = v.Name
                end
            end

            for i, v in pairs(workspace:GetChildren()) do
                if v.ClassName == "Model" and v:FindFirstChild("Hitbox") and _G.test then
                    repeat
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.PrimaryPart.CFrame * CFrame.new(0, 0, 20)
                        game:GetService("ReplicatedStorage").CloudFrameShared.DataStreams.SpearThrown:FireServer(spear, v.PrimaryPart.CFrame, v.PrimaryPart.Position, tonumber("1696341607.0"..math.random(100000,1000000)))
                        game:GetService("ReplicatedStorage").CloudFrameShared.DataStreams.MonsterHit:FireServer(v, spear, true)
                        task.wait(0.1) -- Add a small delay to avoid excessive server calls
                    until v.Health.Value == 0 or _G.test == false or v.Parent == nil
                end
            end
        end)
    end
end

-- Anti Mob Damage
local function antiMobDamage(state)
    _G.damage = (state and true or false)
    while _G.damage do
        wait()
        pcall(function()
            for i, v in pairs(workspace:GetChildren()) do
                if v.ClassName == "Model" and v:FindFirstChild("Hitbox") and v.Hitbox:FindFirstChild("TouchInterest") then
                    v.Hitbox:FindFirstChild("TouchInterest"):Destroy()
                    task.wait()
                end
            end
        end)
    end
end


-- Teleport on CTRL+CLICK
local function teleportOnCtrlClick(state)
    _G.ctrlClickTP = (state and true or false)
    local UserInputService = game:GetService("UserInputService")
    local character = game.Players.LocalPlayer.Character
    local humanoidRootPart = character and character:FindFirstChild("HumanoidRootPart")
    local ctrlPressed = false

    UserInputService.InputBegan:Connect(function(input)
        if input.KeyCode == Enum.KeyCode.LeftControl and input.UserInputType == Enum.UserInputType.Keyboard then
            ctrlPressed = true
        end
    end)

    UserInputService.InputEnded:Connect(function(input)
        if input.KeyCode == Enum.KeyCode.LeftControl and input.UserInputType == Enum.UserInputType.Keyboard then
            ctrlPressed = false
        end
    end)

    UserInputService.InputBegan:Connect(function(input)
        if _G.ctrlClickTP and ctrlPressed and input.UserInputType == Enum.UserInputType.MouseButton1 then
            local mouse = game:GetService("Players").LocalPlayer:GetMouse()
            local target = mouse.Hit
            if humanoidRootPart then
                humanoidRootPart.CFrame = CFrame.new(target.Position)
                print("Teleported to clicked position.")
            end
        end
    end)

    -- Desconectar el evento cuando el toggle se desactive
    if not state then
        UserInputService.InputBegan:Disconnect()
        UserInputService.InputEnded:Disconnect()
    end
end


-- Auto Fish Function
local function autoFish(state)
    getfenv().fish = (state and true or false)
    while getfenv().fish do
        task.wait()
        pcall(function()
            local chr = game.Players.LocalPlayer.Character
            if not chr:FindFirstChildOfClass("Tool") or chr:FindFirstChildOfClass("Tool") and chr:FindFirstChildOfClass("Tool"):GetAttribute("type") ~= "Rods" then
                local plr = game.Players.LocalPlayer.UserId
                for i, v in pairs(game:GetService("ReplicatedStorage").ToolsCache[plr]:GetChildren()) do
                    if v:GetAttribute("type") == "Rods" then
                        rod = v
                    end
                end
                game:GetService("ReplicatedStorage"):WaitForChild("CloudFrameShared"):WaitForChild("DataStreams"):WaitForChild("EquipTool"):FireServer(rod)
            end

            if getfenv().fmethod == "Method 1" then
                game:GetService("ReplicatedStorage"):WaitForChild("CloudFrameShared"):WaitForChild("DataStreams"):WaitForChild("CastFishingLine"):FireServer()
            elseif getfenv().fmethod == "Method 2" then
                local chr = game.Players.LocalPlayer.Character
                chr:FindFirstChildOfClass("Tool"):Activate()
                wait()
                chr:FindFirstChildOfClass("Tool"):Deactivate()
            end

            task.wait(1)
            task.spawn(function()
                game:GetService("ReplicatedStorage"):WaitForChild("CloudFrameShared"):WaitForChild("DataStreams"):WaitForChild("FishBiting"):InvokeServer()
            end)
            task.wait(2.1)
            game:GetService("ReplicatedStorage"):WaitForChild("CloudFrameShared"):WaitForChild("DataStreams"):WaitForChild("FishCaught"):FireServer()
        end)
    end
end


-- Teleport to Islands
local function teleportToIsland(islandName, coordinates)
    local character = game.Players.LocalPlayer.Character
    local humanoidRootPart = character and character:FindFirstChild("HumanoidRootPart")
    if humanoidRootPart then
        humanoidRootPart.CFrame = CFrame.new(coordinates)
        print("Teleported to " .. islandName)
    end
end


-- UI Setup
local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Marco8642/science/main/ui%20libs2", true))()
local example = library:CreateWindow({
    text = "Fishing Simulator",
    visible = true, -- Asegúrate de que la ventana esté visible
})

example:AddLabel("      Select Fishing Method", function(state)
end)

example:AddDropdown({"Method 1", "Method 2"}, function(state)
    getfenv().fmethod = state
end)

example:AddToggle("Auto Fish", autoFish)
example:AddToggle("Auto Collect Chests", autoCollectChests)
example:AddToggle("Auto Collect Loot", autoCollectLoot)
example:AddToggle("Auto Sell Loot", autoSellLoot)
example:AddToggle("Auto Farm Mobs", autoFarmMobs)
example:AddToggle("Anti Mob Damage", antiMobDamage)
example:AddToggle("CTRL+CLICK=TP", teleportOnCtrlClick)

-- Create individual buttons for island teleportation
example:AddButton("Port Jackson", function()
    teleportToIsland("Port Jackson", Vector3.new(14, 53, -39))
end)

example:AddButton("Monster Borough", function()
    teleportToIsland("Monster Borough", Vector3.new(-3253, 45, 2718))
end)

example:AddButton("Dunas Del Faraón", function()
    teleportToIsland("Dunas Del Faraón", Vector3.new(-4281, 96, 125))
end)

example:AddButton("Costas Antiguas", function()
    teleportToIsland("Costas Antiguas", Vector3.new(-2414, 41, -1812))
end)

example:AddButton("Islas Sombreadas", function()
    teleportToIsland("Islas Sombreadas", Vector3.new(2182, 43, -2271))
end)

example:AddButton("Isla Erupción", function()
    teleportToIsland("Isla Erupción", Vector3.new(2803, 50, 1492))
end)


-- Anti-AFK to prevent being kicked for inactivity
game:GetService("Players").LocalPlayer.Idled:Connect(function()
    warn("Anti afk ran")
    game:GetService("VirtualUser"):CaptureController()
    game:GetService("VirtualUser"):ClickButton2(Vector2.new())
end)
warn("Anti afk running")
