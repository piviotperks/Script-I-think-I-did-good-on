-- Create GUI
local playerGui = game.Players.LocalPlayer:WaitForChild("PlayerGui")
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AutoActionsGui"
screenGui.Parent = playerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 200, 0, 350)
frame.Position = UDim2.new(0, 10, 0, 10)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.Parent = screenGui

-- Create Buttons
local function createButton(name, position, action)
local button = Instance.new("TextButton")
button.Size = UDim2.new(1, 0, 0, 40)
button.Position = position
button.Text = name
button.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
button.TextColor3 = Color3.fromRGB(255, 255, 255)
button.Parent = frame

button.MouseButton1Click:Connect(action)
end

-- Create RemoteEvents if they don't exist
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local function createRemoteEvent(name)
local remoteEvent = ReplicatedStorage:FindFirstChild(name)
if not remoteEvent then
remoteEvent = Instance.new("RemoteEvent")
remoteEvent.Name = name
remoteEvent.Parent = ReplicatedStorage
end
return remoteEvent
end

local BreakBedEvent = createRemoteEvent("BreakBed")
local KillOthersEvent = createRemoteEvent("KillOthers")

-- Connect GUI buttons to RemoteEvents
createButton("Break All Beds", UDim2.new(0, 0, 0, 0), function()
local beds = workspace:FindFirstChild("Beds")
if beds then
for _, bed in ipairs(beds:GetChildren()) do
bed:Destroy()
end
end
end)

createButton("Infinite Health", UDim2.new(0, 0, 0, 50), function()
local player = game.Players.LocalPlayer
local character = player.Character
if character and character:FindFirstChild("Humanoid") then
character.Humanoid.HealthChanged:Connect(function(health)
if health < 100 then
character.Humanoid.Health = 100
end
end)
end
end)

createButton("Kill Aura", UDim2.new(0, 0, 0, 100), function()
local player = game.Players.LocalPlayer
while true do
for _, otherPlayer in ipairs(game.Players:GetPlayers()) do
if otherPlayer ~= player then
local character = otherPlayer.Character
if character and character:FindFirstChild("Humanoid") then
character.Humanoid:TakeDamage(100)
end
end
end
wait(1) -- Adjust wait time if necessary
end
end)

createButton("Infinite Jump & Fly", UDim2.new(0, 0, 0, 150), function()
local player = game.Players.LocalPlayer
local character = player.Character
if character and character:FindFirstChild("Humanoid") then
local humanoid = character.Humanoid
local function onJumpRequest()
if humanoid then
local hrp = character:FindFirstChild("HumanoidRootPart")
if hrp then
hrp.Velocity = Vector3.new(0, 100, 0) -- Adjust velocity for flying
end
end
end
game:GetService("UserInputService").JumpRequest:Connect(onJumpRequest)
end
end)

createButton("Anti-Cheat Disabler", UDim2.new(0, 0, 0, 200), function()
-- Placeholder: anti-cheat disabler is not implemented
print("Anti-Cheat Disabler is not implemented.")
end)

createButton("Speed Boost", UDim2.new(0, 0, 0, 250), function()
local player = game.Players.LocalPlayer
local character = player.Character
if character and character:FindFirstChild("Humanoid") then
character.Humanoid.WalkSpeed = 25
end
end)

-- Ensure script is active until player leaves
local player = game.Players.LocalPlayer
player.CharacterAdded:Connect(function(character)
-- Ensure GUI is reloaded
screenGui.Parent = playerGui
end)
