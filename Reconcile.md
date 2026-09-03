# Portfolio
### Reconcile
Reconcile is replacing, removing, or adding any extra or missing information from a table using a template. This is useful for data store modules or scripts if you plan to use long term. I will not be posting a full version of my data store module since profile store is almost exactly similar to what I did with mine <em>(using the jobid and preventing data loss through fast rejoins)</em>

## Script Info
```lua
-- Reconcile code
local function reconile(data, template)
	if typeof(data) == "table" and typeof(template) == "table" then
		for key, tablevalue in template do
			if data[key] == nil or not data[key] then
				if typeof(data[key]) == "table" then
					data[key] = reconile({}, tablevalue)
				else
					data[key] = tablevalue
				end
			elseif typeof(data[key]) == "table" and typeof(tablevalue) == "table" then
				reconile(data[key], tablevalue)
			end
		end
		
		for key, tablevalue in data do
			if template[key] == nil or not template[key] then
				data[key] = nil
			end
		end
	end
	
	return data
end
```

```lua
-- Example usage
local template = {
  Coins = 125,
	Classes = {
		killer = {
			"killertemplatename"
		},
		humans = {
			"survivor",
      "survivor2"
		}
	}
}

local playerdata = {
  weirdvalue = 0,
  Two = 2
  }

local function reconile(data, template)
	if typeof(data) == "table" and typeof(template) == "table" then
		for key, tablevalue in template do
			if data[key] == nil or not data[key] then
				if typeof(data[key]) == "table" then
					data[key] = reconile({}, tablevalue)
				else
					data[key] = tablevalue
				end
			elseif typeof(data[key]) == "table" and typeof(tablevalue) == "table" then
				reconile(data[key], tablevalue)
			end
		end
		
		for key, tablevalue in data do
			if template[key] == nil or not template[key] then
				data[key] = nil
			end
		end
	end
	
	return data
end

local filteredTable = reconile(playerdata, template)
print(filteredTable)
```
### Output:
```lua
-- The output will show a mixed order. Using reconcile should be fine if order isn't important. You can always call table[keyname] or any other table tool.
{
  ["Classes"] =  {
    ["humans"] = {
      [1] = "survivor",
      [2] = "survivor2"
    },
    ["killer"] = {
      [1] = "killertemplatename"
    }
  },
  ["Coins"] = 125
}
```
  
