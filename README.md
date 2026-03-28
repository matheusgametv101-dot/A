# A
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- CONFIG
local allowedPlayers = {
	["Player1"] = true,
	["Player2"] = true
}

local targetName = "Player1" -- nome do player para morph

-- GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player:WaitForChild("PlayerGui")

local button = Instance.new("TextButton")
button.Size = UDim2.new(0, 150, 0, 50)
button.Position = UDim2.new(0, 20, 0, 100)
button.Text = "Morph OFF"
button.Parent = screenGui

local toggled = false

-- Função de morph
local function morph(target)
	local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
	if not humanoid then return end
	
	local targetPlayer = Players:FindFirstChild(target)
	if not targetPlayer then return end
	
	local success, desc = pcall(function()
		return Players:GetHumanoidDescriptionFromUserId(targetPlayer.UserId)
	end)
	
	if success and desc then
		humanoid:ApplyDescription(desc)
	end
end

-- Reset (respawn)
local function reset()
	player:LoadCharacter()
end

-- Toggle
button.MouseButton1Click:Connect(function()
	toggled = not toggled
	
	if toggled then
		button.Text = "Morph ON"
		
		if allowedPlayers[targetName] then
			morph(targetName)
		end
	else
		button.Text = "Morph OFF"
		reset()
	end
end)
