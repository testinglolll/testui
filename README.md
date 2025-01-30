local ArisHub = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/UI-Libs/main/discord%20lib.txt"))()

-- Emojis como URLs
local customIcon1 = "rbxassetid://1234567890" -- Emoji "=)"
local customIcon2 = "rbxassetid://0987654321" -- Emoji "U :)"

-- Janela principal
local win = ArisHub:Window("Aris-HUB")

-- Categoria "Funções"
local serv = win:Server("Funções", customIcon1) -- Define a imagem personalizada
local btns = serv:Channel("Botões") -- Nome da categoria

-- Botão "Se Curar"
btns:Button("Se Curar", function()
    local player = game.Players.LocalPlayer -- Obtém o jogador local
    if player and player.Character then
        local humanoid = player.Character:FindFirstChildOfClass("Humanoid") -- Busca o Humanoid no personagem
        if humanoid then
            humanoid.Health = humanoid.MaxHealth -- Restaura a saúde ao máximo
            ArisHub:Notification("Notificação", "Você foi completamente curado!", "Entendido!")
        else
            ArisHub:Notification("Erro", "Não foi possível encontrar o Humanoid!", "Ok")
        end
    else
        ArisHub:Notification("Erro", "Jogador ou personagem inválido!", "Ok")
    end
end)

-- Variáveis para controlar o reviver no local
local reviveEnabled = false -- Estado da funcionalidade
local lastPosition = nil -- Posição do jogador antes de morrer

-- Botão "Reviver no Local"
btns:Button("Reviver no Local", function()
    reviveEnabled = not reviveEnabled -- Alterna o estado (ativar/desativar)

    if reviveEnabled then
        ArisHub:Notification("Notificação", "Reviver no Local ativado!", "Ok")

        local player = game.Players.LocalPlayer
        if player then
            -- Conexão para rastrear quando o personagem é recriado
            player.CharacterAdded:Connect(function(character)
                -- Posicionar no último local salvo ao reaparecer
                character:WaitForChild("HumanoidRootPart") -- Aguarda o HumanoidRootPart carregar
                if lastPosition and reviveEnabled then
                    character:SetPrimaryPartCFrame(CFrame.new(lastPosition))
                end

                -- Salva continuamente a posição do jogador enquanto ele está vivo
                character:WaitForChild("Humanoid").Died:Connect(function()
                    if character.PrimaryPart then
                        lastPosition = character.PrimaryPart.Position
                    end
                end)
            end)

            -- Salva a posição inicial do personagem
            if player.Character and player.Character.PrimaryPart then
                lastPosition = player.Character.PrimaryPart.Position
            end
        else
            ArisHub:Notification("Erro", "Jogador não encontrado!", "Ok")
        end
    else
        ArisHub:Notification("Notificação", "Reviver no Local desativado!", "Ok")
        lastPosition = nil -- Reseta a posição salva
    end
end)

-- Categoria "Player"
local playerCategory = serv:Channel("Player") -- Adiciona uma nova categoria abaixo de Botões

-- Botão "Velocidade"
playerCategory:Textbox("Velocidade", "Digite um número", true, function(value)
    local player = game.Players.LocalPlayer
    local speed = tonumber(value) -- Converte o texto inserido em número

    if speed and speed > 0 then
        if player and player.Character then
            local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid.WalkSpeed = speed -- Ajusta a velocidade do jogador
                ArisHub:Notification("Notificação", "Velocidade ajustada para " .. speed, "Entendido!")
            else
                ArisHub:Notification("Erro", "Humanoid não encontrado!", "Ok")
            end
        else
            ArisHub:Notification("Erro", "Jogador ou personagem inválido!", "Ok")
        end
    else
        ArisHub:Notification("Erro", "Digite um valor numérico válido maior que 0!", "Ok")
    end
end)

-- Botão "Infinite Yield"
playerCategory:Button("Infinite Yield", function()
    loadstring(game:HttpGet(('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'), true))()
    ArisHub:Notification("Notificação", "Infinite Yield carregado com sucesso!", "Entendido!")
end)

-- Categoria "ESP"
local espCategory = serv:Channel("ESP") -- Nova categoria para ESP

-- Botão "Ativar ESP"
espCategory:Button("Ativar ESP", function()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local espBox = Instance.new("BoxHandleAdornment")
            espBox.Size = Vector3.new(4, 6, 4)
            espBox.Adornee = player.Character:FindFirstChild("HumanoidRootPart")
            espBox.Color3 = Color3.new(1, 0, 0)
            espBox.AlwaysOnTop = true
            espBox.ZIndex = 5
            espBox.Transparency = 0.5
            espBox.Parent = player.Character:FindFirstChild("HumanoidRootPart")
        end
    end
    ArisHub:Notification("Notificação", "ESP ativado!", "Entendido!")
end)

-- Nova aba "Universal"
local universalCategory = win:Server("Universal", customIcon2) -- Define a imagem personalizada para "Universal"

-- Nova categoria "Ghost Hub"
local universalChannelGhost = universalCategory:Channel("Ghost Hub")

-- Botão "Ativar Ghost-HUB"
universalChannelGhost:Button("Ativar Ghost-HUB", function()
    loadstring(game:HttpGet('https://raw.githubusercontent.com/GhostPlayer352/Test4/main/GhostHub'))()
end)

-- Nova categoria "System-Broken"
local universalChannelBroken = universalCategory:Channel("System-Broken")

-- Botão "Ativar System-Broken"
universalChannelBroken:Button("Ativar System-Broken", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/H20CalibreYT/SystemBroken/main/script"))()
end)

-- Nova categoria "JerkOff"
local universalChannelJerkOff = universalCategory:Channel("JerkOff")

-- Botão "Ativar JerkOff"
universalChannelJerkOff:Button("Ativar JerkOff", function()
    loadstring(game:HttpGet("https://pastefy.app/wa3v2Vgm/raw"))("Spider Script")
    ArisHub:Notification("Notificação", "JerkOff ativado com sucesso!", "Entendido!")
end)

-- Nova categoria "Kick"
local universalChannelKick = universalCategory:Channel("Kick")

-- Botão "Kickar Jogador"
universalChannelKick:Button("Kickar Jogador", function()
    local playerName = ArisHub:Textbox("Digite o nome do jogador", "Nome do jogador", true, function(value)
        -- Script de kick para jogadores
        game.Players.PlayerAdded:Connect(function(player)
            -- Condição para kicker (exemplo: se o nome do jogador for igual ao especificado)
            if player.Name == value then
                -- Executa o kick no jogador
                player:Kick("Você foi expulso do servidor por Ari'sHub") 
                ArisHub:Notification("Notificação", "Jogador " .. player.Name .. " foi kickado com sucesso!", "Entendido!")
            end
        end)
    end)
end)

-- Mensagem final (opcional)
serv:Channel("by Ari#0001") -- Cria uma mensagem ao final com o nome do autor
