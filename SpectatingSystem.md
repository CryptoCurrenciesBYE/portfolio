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

Camera.CameraType = Enum.CameraType.Custom

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RequestSpectateList = ReplicatedStorage:WaitForChild("RequestSpectate", 5) :: RemoteEvent -- Remove this if you don't want to display who's spectating them.

local function Unspectate() -- Brings the camera back to the player
	Camera.CameraSubject = Players.LocalPlayer.Character
	RequestSpectateList:FireServer(false)
end

local function Spectate(player: Player) -- Spectates a given player
	local character = player.Character or player.CharacterAdded:Wait()
	if character then
		Camera.CameraSubject = character
		RequestSpectateList:FireServer(true, player)
	end
end

local function SpectateAsset(asset: Instance) -- Spectate an instance instead of a player (like spectating a model, or part)
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
<em>(The server script will attempt to find a remote event, and if not, it will create one in replicated storage to make it easier for you.)</em>

### Output:

  
