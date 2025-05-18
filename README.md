-- NATAN DEAD REAILS 1.0 | Script Avançado para Dead Rails
-- Desenvolvido por ChatGPT para uso pessoal
-- Funcionalidades: Teleportes, Auto Farm, Noclip, Menu Bonito e Arrastável

-- Carrega a biblioteca da UI Orion
local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/shlexware/Orion/main/source"))()
local Window = OrionLib:MakeWindow({
    Name = "NATAN DEAD REAILS 1.0",
    HidePremium = false,
    SaveConfig = true,
    ConfigFolder = "NatanDeadRails",
    IntroText = "Natan Script",
    IntroIcon = "",
})

-- Variáveis principais
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Função de teleporte
function teleportTo(position)
    if character and character:FindFirstChild("HumanoidRootPart") then
        character:MoveTo(position)
    end
end

-- Criação das abas do menu
local tpTab = Window:MakeTab({
    Name = "Teleporte",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

local farmTab = Window:MakeTab({
    Name = "Auto Farm",
    Icon = "rbxassetid://6035047409",
    PremiumOnly = false
})

-- Botões de teleporte
tpTab:AddButton({
    Name = "Ir para o Banco do Trem",
    Callback = function()
        teleportTo(Vector3.new(10, 5, -80)) -- Altere conforme necessário
    end
})

tpTab:AddButton({
    Name = "Ir para a Torre Final",
    Callback = function()
        teleportTo(Vector3.new(105, 110, 340)) -- Coordenada da torre final
    end
})

tpTab:AddButton({
    Name = "Ir para a Base Principal",
    Callback = function()
        teleportTo(Vector3.new(-200, 30, 50)) -- Exemplo de base
    end
})

-- Função de Auto Farm
_G.AutoFarm = false
farmTab:AddToggle({
    Name = "Ativar Auto Farm",
    Default = false,
    Callback = function(v)
        _G.AutoFarm = v
        while _G.AutoFarm do
            for _, item in pairs(workspace:GetDescendants()) do
                if item:IsA("TouchTransmitter") and item.Parent then
                    firetouchinterest(character.HumanoidRootPart, item.Parent, 0)
                    firetouchinterest(character.HumanoidRootPart, item.Parent, 1)
                end
            end
            wait(1)
        end
    end
})

-- Função de Noclip
farmTab:AddToggle({
    Name = "Ativar Noclip",
    Default = false,
    Callback = function(state)
        getgenv().noclip = state
        game:GetService("RunService").Stepped:Connect(function()
            if getgenv().noclip and character then
                for _, part in pairs(character:GetDescendants()) do
                    if part:IsA("BasePart") and part.CanCollide == true then
                        part.CanCollide = false
                    end
                end
            end
        end)
    end
})

-- Inicializa o menu
OrionLib:Init()
