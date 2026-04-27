
-- pretty messy actually

local introBindable = game.ReplicatedStorage.Intro
game.StarterGui:SetCoreGuiEnabled(Enum.CoreGuiType.Backpack, false)
local userInput = game:GetService("UserInputService")
local TWS = game:GetService("TweenService")
local player = game.Players.LocalPlayer
local backpack = player.Backpack
local char = player.Character or player.CharacterAdded:Wait()
local frame = script.Parent.Main
local camera = workspace.CurrentCamera

local function ZoomIn()
	TWS:Create(camera, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {FieldOfView = 70}):Play()
end
local function ZoomOut()
	TWS:Create(camera, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {FieldOfView = 90}):Play()
end
local equiped = false

local lastPressed = nil
local humanoid = char:WaitForChild("Humanoid")

local function scale(slot, scaleValue)
	local uiScale = slot:FindFirstChild("UIScale")
	if uiScale then
		TWS:Create(uiScale, TweenInfo.new(0.3), {Scale = scaleValue}):Play()
	end
end

local function clearSlot(slot, itemName)
	slot.Used.Value = false
	slot.Label.Visible = false
	slot.Label.Text = "Loading"
	slot.Value.Value = 0
	local model = slot.ViewportFrame:FindFirstChild(itemName)
	if model then
		model:Destroy()
	end
end

local function onSlotClicked(slot)
	if slot.Used.Value then
		if equiped == false then
			for _,obj in pairs(backpack:GetChildren()) do
				if obj:IsA("Tool") and obj.ToolBool.Value == slot.Value.Value then
					humanoid:EquipTool(obj)
					break
				end
			end
			equiped = true
		else
			humanoid:UnequipTools()
			equiped = false
		end
	else
		humanoid:UnequipTools()
		equiped = false
	end
end

userInput.InputBegan:Connect(function(input, processed)
	if input.KeyCode == Enum.KeyCode.One and not processed then
		onSlotClicked(frame.Slot1)
	elseif input.KeyCode == Enum.KeyCode.Two and not processed then
		onSlotClicked(frame.Slot2)
	elseif input.KeyCode == Enum.KeyCode.Three and not processed then
		onSlotClicked(frame.Slot3)
	elseif input.KeyCode == Enum.KeyCode.Four and not processed then
		onSlotClicked(frame.Slot4)
	end
end)

for _, slot in pairs(frame:GetChildren()) do
	if slot:FindFirstChild("Button") then
		slot.Button.MouseButton1Click:Connect(function()
			onSlotClicked(slot)
		end)
	end
end

local function creteSlot(item)
	local slots = {}

	for _, slot in ipairs(frame:GetChildren()) do
		if slot:IsA("Frame") then
			table.insert(slots, slot)
		end
	end

	table.sort(slots, function(a, b)
		return a.Name < b.Name
	end)

	for _, slot in ipairs(slots) do
		if slot.Used.Value == false then
			slot.Used.Value = true

			local visualClone = game.ReplicatedStorage.ToolModels:FindFirstChild(item.Name):Clone()
			visualClone.Parent = slot.ViewportFrame
			if visualClone:IsA("Model") then
	
			else
				visualClone.Position = Vector3.new(0,0,-2)
				visualClone.Orientation = Vector3.new(0,0,0)
			end

			slot.Value.Value = item.ToolBool.Value
			slot.Label.Text = item.Name
			slot.Label.Visible = true
			break
		end
	end
end

local slotsUsed = 1

backpack.ChildAdded:Connect(function(item)

	if item:IsA("Tool") and not item:FindFirstChild("ToolBool") then
		
		local toolBool = Instance.new("IntValue")
		toolBool.Name = "ToolBool"
		toolBool.Value = slotsUsed
		toolBool.Parent = item
		slotsUsed += 1
		creteSlot(item)
	end
end)

backpack.ChildRemoved:Connect(function(item)
	task.wait()
	if not char:FindFirstChild(item.Name) then
		local toolBoolVal = item:FindFirstChild("ToolBool") and item.ToolBool.Value
		for _,slot in pairs(frame:GetChildren()) do
			if slot:IsA("Frame") then
				if toolBoolVal and slot.Value.Value == toolBoolVal then
					clearSlot(slot, item.Name)
					break
				end
			end
		end
	end
end)

char.ChildRemoved:Connect(function(item)
	task.wait(.1)
	equiped = false
	local toolBoolVal = item:FindFirstChild("ToolBool") and item.ToolBool.Value

	if not backpack:FindFirstChild(item.Name) then
		for _,slot in pairs(frame:GetChildren()) do
			if slot:IsA("Frame") then
				if toolBoolVal and slot.Value.Value == toolBoolVal then
					clearSlot(slot, item.Name)
					break
				end
			end
		end
	else
		local matchExists = false
		for _,obj in pairs(backpack:GetChildren()) do
			if obj:IsA("Tool") and obj:FindFirstChild("ToolBool") and obj.ToolBool.Value == toolBoolVal then
				matchExists = true
				break
			end
		end
		if not matchExists then
			for _,slot in pairs(frame:GetChildren()) do
				if slot:IsA("Frame") then
					if toolBoolVal and slot.Value.Value == toolBoolVal then
						clearSlot(slot, item.Name)
						break
					end
				end
			end
		end
	end
end)

game.ReplicatedStorage.InvDebug.OnClientEvent:Connect(function(item)
	equiped = false
	local toolBoolVal = item:FindFirstChild("ToolBool") and item.ToolBool.Value

	if not backpack:FindFirstChild(item.Name) then
		for _,slot in pairs(frame:GetChildren()) do
			if slot:IsA("Frame") then
				if toolBoolVal and slot.Value.Value == toolBoolVal then
					clearSlot(slot, item.Name)
					break
				end
			end
		end
	else
		local matchExists = false
		for _,obj in pairs(backpack:GetChildren()) do
			if obj:IsA("Tool") and obj:FindFirstChild("ToolBool") and obj.ToolBool.Value == toolBoolVal then
				matchExists = true
				break
			end
		end
		if not matchExists then
			for _,slot in pairs(frame:GetChildren()) do
				if slot:IsA("Frame") then
					if toolBoolVal and slot.Value.Value == toolBoolVal then
						clearSlot(slot, item.Name)
						break
					end
				end
			end
		end
	end
end)

introBindable.Event:Connect(function(bool)
	script.Parent.Enabled = bool
end)

game.ReplicatedStorage.DeadUi.Event:Connect(function(bool)
	script.Parent.Enabled = bool
end)
