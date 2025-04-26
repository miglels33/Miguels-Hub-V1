local CoastingLibrary = loadstring(game:HttpGet("https://raw.githubusercontent.com/GhostDuckyy/UI-Libraries/main/Coasting%20Ui%20Lib/source.lua"))()

local PlayerTab = CoastingLibrary:CreateTab("Player")
local MusicTab = CoastingLibrary:CreateTab("Music")
local JumpSection = PlayerTab:CreateSection("Jump")
local SpeedSection = PlayerTab:CreateSection("Speed")
local MusicSection = MusicTab:CreateSection("Musicas")

MusicSection:CreateButton("Tocar Raind tacos", function()
    -- Cria um Sound na workspace ou no player
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://142376088" -- Substitua pelo ID da música
    sound.Looped = true -- Se desejar que a música repita
    sound.Volume = 0.5 -- Volume (0 a 1)
    sound.Parent = game:GetService("SoundService") -- Toca globalmente
    -- sound.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui") -- Toca apenas para o jogador
    
    sound:Play()
    print("Música tocando...")
end)

JumpSection:CreateToggle("Fly Mode", function(boolean)
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoid = character:WaitForChild("Humanoid")
    local rootPart = character:WaitForChild("HumanoidRootPart")
    local camera = workspace.CurrentCamera

    -- Variáveis para armazenar as instâncias
    local flyComponents = {
        bodyVelocity = nil,
        flyConnection = nil,
        bodyGyro = nil
    }

    local function cleanUpFly()
        -- Desconecta e destrói todos os componentes
        if flyComponents.flyConnection then
            flyComponents.flyConnection:Disconnect()
            flyComponents.flyConnection = nil
        end
        
        if flyComponents.bodyVelocity then
            flyComponents.bodyVelocity:Destroy()
            flyComponents.bodyVelocity = nil
        end
        
        if flyComponents.bodyGyro then
            flyComponents.bodyGyro:Destroy()
            flyComponents.bodyGyro = nil
        end

        -- Reset completo das propriedades
        humanoid.PlatformStand = false
        humanoid.AutoRotate = true
        humanoid:ChangeState(Enum.HumanoidStateType.FallingDown)
        
        -- Força a física a agir
        rootPart.Anchored = false
        rootPart.AssemblyLinearVelocity = Vector3.new(0, -100, 0) -- Queda rápida
        rootPart.AssemblyAngularVelocity = Vector3.new(0, 0, 0)
        
        -- Garante que nenhum componente residual fique
        for _, child in pairs(rootPart:GetChildren()) do
            if child:IsA("BodyVelocity") or child:IsA("BodyGyro") then
                child:Destroy()
            end
        end
    end

    if boolean then
        -- Limpa qualquer resíduo antes de ativar
        cleanUpFly()

        -- Ativa o Fly Mode
        humanoid:ChangeState(Enum.HumanoidStateType.Physics)
        humanoid.PlatformStand = true
        
        -- Cria BodyVelocity para movimento
        flyComponents.bodyVelocity = Instance.new("BodyVelocity")
        flyComponents.bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        flyComponents.bodyVelocity.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
        flyComponents.bodyVelocity.P = 10000
        flyComponents.bodyVelocity.Parent = rootPart

        -- Cria BodyGyro para rotacionar o jogador
        flyComponents.bodyGyro = Instance.new("BodyGyro")
        flyComponents.bodyGyro.MaxTorque = Vector3.new(4000, 4000, 4000)
        flyComponents.bodyGyro.CFrame = CFrame.new(rootPart.Position, rootPart.Position + camera.CFrame.LookVector)
        flyComponents.bodyGyro.Parent = rootPart

        -- Controles de voo
        flyComponents.flyConnection = game:GetService("RunService").Heartbeat:Connect(function()
            local camCFrame = camera.CFrame
            local moveDirection = Vector3.new(0, 0, 0)

            -- Atualiza a direção do BodyGyro
            if flyComponents.bodyGyro then
                flyComponents.bodyGyro.CFrame = CFrame.new(rootPart.Position, rootPart.Position + camCFrame.LookVector)
            end

            -- Movimento WASD
            if game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.W) then
                moveDirection = moveDirection + camCFrame.LookVector
            end
            if game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.S) then
                moveDirection = moveDirection - camCFrame.LookVector
            end
            if game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.A) then
                moveDirection = moveDirection - camCFrame.RightVector
            end
            if game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.D) then
                moveDirection = moveDirection + camCFrame.RightVector
            end

            -- Controles de altitude
            if game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.Space) then
                moveDirection = moveDirection + Vector3.new(0, 1, 0)
            elseif game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.LeftShift) then
                moveDirection = moveDirection + Vector3.new(0, -1, 0)
            end

            -- Aplica velocidade
            if moveDirection.Magnitude > 0 and flyComponents.bodyVelocity then
                moveDirection = moveDirection.Unit * 50
                flyComponents.bodyVelocity.Velocity = moveDirection
            elseif flyComponents.bodyVelocity then
                flyComponents.bodyVelocity.Velocity = Vector3.new(0, 0, 0)
            end
        end)

        print("Fly Mode: Ativado")
    else
        -- Desativa completamente o Fly Mode
        cleanUpFly()
        
        -- Reset adicional após 0.1 segundos
        delay(0.1, function()
            if humanoid and humanoid.Parent then
                humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
            end
            if rootPart and rootPart.Parent then
                rootPart.AssemblyLinearVelocity = Vector3.new(0, -10, 0)
            end
        end)

        print("Fly Mode: Desativado")
    end
end)

MusicSection:CreateButton("Tocar Lalalava", function()
    -- Cria um Sound na workspace ou no player
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://111172243066964" -- Substitua pelo ID da música
    sound.Looped = true -- Se desejar que a música repita
    sound.Volume = 0.5 -- Volume (0 a 1)
    sound.Parent = game:GetService("SoundService") -- Toca globalmente
    -- sound.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui") -- Toca apenas para o jogador
    
    sound:Play()
    print("Música tocando...")
end)MusicSection:CreateButton("Tocar funk", function()
    -- Cria um Sound na workspace ou no player
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://1836262425" -- Substitua pelo ID da música
    sound.Looped = true -- Se desejar que a música repita
    sound.Volume = 0.5 -- Volume (0 a 1)
    sound.Parent = game:GetService("SoundService") -- Toca globalmente
    -- sound.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui") -- Toca apenas para o jogador
    
    sound:Play()
    print("Música tocando...")
end)

local player = game.Players.LocalPlayer
local connection -- Variável persistente para armazenar a conexão

JumpSection:CreateToggle("Inf jump", function(boolean)
    if boolean then
        -- Função para pulo infinito
        local function infiniteJump()
            local character = player.Character or player.CharacterAdded:Wait()
            local humanoid = character:FindFirstChildOfClass("Humanoid")
            if humanoid and humanoid:GetState() ~= Enum.HumanoidStateType.Jumping then
                humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
            end
        end

        -- Conecta ao evento de pulo (se já não estiver conectado)
        if not connection then
            connection = game:GetService("UserInputService").JumpRequest:Connect(infiniteJump)
        end

        print("Pulo infinito: ativado")
    else
        -- Desativa o pulo infinito (se a conexão existir)
        if connection then
            connection:Disconnect()
            connection = nil
        end

        print("Pulo infinito: desativado")
    end
end)

SpeedSection:CreateSlider("Speed", 0, 200, 50, false, function(value)
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    
    -- Espera até que o Humanoid esteja disponível
    if not character:FindFirstChildOfClass("Humanoid") then
        character:WaitForChild("Humanoid")
    end
    
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    humanoid.WalkSpeed = value
    print("Velocidade do player alterada para: " .. value)
end)


JumpSection:CreateSlider("Jump power", 0, 200, 50, false, function(value)
    local player = game.Players.LocalPlayer
    
    -- Função para aplicar o JumpPower ao Humanoid
    local function applyJumpPower(character)
        if not character:FindFirstChildOfClass("Humanoid") then
            character:WaitForChild("Humanoid")
        end
        
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        humanoid.JumpPower = value -- Altera a altura do pulo
        print("Altura do pulo alterada para: " .. value)
    end
    
    -- Aplica ao personagem atual (se existir)
    if player.Character then
        applyJumpPower(player.Character)
    end
    
    -- Aplica quando o personagem for trocado (ex: morte/resspawn)
    player.CharacterAdded:Connect(applyJumpPower)
end)
