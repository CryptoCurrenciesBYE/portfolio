# Portfolio
### Sprinting
Sprinting with stamina. I know, this is a horrible way to do it but I was doing this at 2:31am and I wanna go to bed, so here we go.

## Script Info
```lua
-- Server Script -- ServerScriptService
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local sprinters = {}

Players.PlayerAdded:Connect(function(player)
	sprinters[player] = {
		sprinting = false,
		sprintinterval = 0
	}
	player.CharacterAdded:Connect(function(character)
		local humanoid = character:FindFirstChildOfClass("Humanoid")
		if humanoid then
			humanoid:SetAttribute("Stamina", 100)
		end
	end)
end)

Players.PlayerRemoving:Connect(function(player)
	if player and sprinters[player] then sprinters[player] = nil end
end)

RunService.Heartbeat:Connect(function(delta)
	for player, stats in sprinters do
		local currenttime = os.clock()
		if currenttime >= stats.sprintinterval then
			local character = player.Character
			if character then
				local humanoid = character:FindFirstChildOfClass("Humanoid")
				if humanoid then
					local stamina = humanoid:GetAttribute("Stamina") or 100
					if stats.sprinting then
						humanoid:SetAttribute("Stamina", math.max(0, stamina - 1))
						stats.sprintinterval = currenttime + 0.08
						humanoid.WalkSpeed = 26
						if stamina == 0 then
							stats.sprintinterval = currenttime + 1.3
							stats.sprinting = false
							humanoid.WalkSpeed = 10
							return
						end
					else
						humanoid:SetAttribute("Stamina", math.min(100, stamina + 1))
						stats.sprintinterval = currenttime + 0.06
						humanoid.WalkSpeed = 16
					end
				end
			end
		end
	end
end)

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local sprintRemote = ReplicatedStorage:WaitForChild("sprint")

sprintRemote.OnServerEvent:Connect(function(player, sprintbool)
	sprinters[player].sprinting = sprintbool
end)
```

```lua
-- Local Script - StartPlayerScript
local UIS = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local sprintEvent = ReplicatedStorage:WaitForChild("sprint")

UIS.InputBegan:Connect(function(Input, GP)
	if GP then return end
	
	if Input.KeyCode == Enum.KeyCode.LeftShift then
		sprintEvent:FireServer(true)
	end
end)

UIS.InputBegan:Connect(function(Input)
	if Input.KeyCode == Enum.KeyCode.LeftShift then
		sprintEvent:FireServer(false)
	end
end)
```

### Output:
Play will hold shift and start sprinting and when stamina reaches 0, the player's speed will reach 10 for exhaustion and stamina will slowly start climbing back to 100. Stamina variable was turned into a humanoid attribute for the client's convinence.

  
