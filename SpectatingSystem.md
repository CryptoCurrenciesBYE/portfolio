# Portfolio
### Spectating System
The spectating system below is very useful for players to see what's going on in-game and displays who's spectating them.

## Script Info
<em>(If you don't want the script where the players can see who's spectating them, don't put the server script in)</em>
```lua
-- Local Script - StarterCharacterScripts
-- Partial Code: The code below are local functions and important stuff needed.
local Workspace = game:GetService("Workspace")
local Camera = Workspace.CurrentCamera
local Players = game:GetService("Players")

local client = Players.LocalPlayer

Camera.CameraType = Enum.CameraType.Custom

local ReplicatedStorage = game:GetService("ReplicatedStorage")

local function Unspectate()
	local character = client.Character
	if character then
		local humanoid = character:FindFirstChildOfClass("Humanoid")
		if humanoid then
			Camera.CameraSubject = humanoid
		end
	end
end

local function Spectate(player: Player)
	local character = player.Character or player.CharacterAdded:Wait()
	if character then
		local humanoid = character:FindFirstChildOfClass("Humanoid")
		if humanoid then
			Camera.CameraSubject = humanoid
		end
	end
end

local function SpectateAsset(asset: Instance)
	Camera.CameraSubject = asset
end


--[[ Example usage:
local AllPlayers = Players:GetPlayers()
local player = AllPlayers[math.random(1, #AllPlayers)]
Spectate(player)

task.wait(1)

Unspectate()
--]]
```

### Output:
When the <em><b>Spectate</em></b> function is called, the spectator's camera is positioned on the player's humanoid and follows the player using <em><b>CameraSubject</em></b>. And when the <em><b>Unspectate</em></b> function is called, the spectator's camera is position back to their humanoid.

  
