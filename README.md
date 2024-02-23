# BasicAI
OMG POG AI FOR DUMMIES!!!!!1!!11!  


This code defines several functions:

GetRandomPointInRadius: Generates a random point within a specified radius around the dummy for roaming.
LookAtTarget: Makes the dummy look at a specific target (e.g., player).
MoveToTarget: Moves the dummy towards a target using Roblox's built-in Humanoid movement system.
ChaseTarget: Chases a target if it gets close enough (within ChaseDistance).
HandleDamage: Triggers the "Chase" state when the dummy takes damage.
The RunService.Heartbeat event triggers every frame and updates the dummy's behavior based on its current state.  
  

```
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")

local Dummy = script.Parent -- Replace with your actual dummy object

local WalkSpeed = 16 -- Adjust as needed
local RunSpeed = 25 -- Adjust as needed
local ChaseSpeed = 30 -- Adjust as needed
local RoamRadius = 20 -- Adjust as needed
local LookRange = 20 -- Adjust as needed
local ChaseDistance = 15 -- Adjust as needed

local CurrentState = "Roam" -- Initial state

local function GetRandomPointInRadius()
  local x = math.random(-RoamRadius, RoamRadius)
  local y = math.random(-RoamRadius, RoamRadius)
  local z = 0 -- Assuming flat terrain
  return Dummy.Position + Vector3.new(x, y, z)
end

local function LookAtTarget(target)
  if target then
    local lookDir = (target.Position - Dummy.Position).unit
    Dummy.Humanoid:LookAt(lookDir)
  end
end

local function MoveToTarget(target)
  if target then
    local dir = (target.Position - Dummy.Position).unit
    Dummy.Humanoid:Move(dir * WalkSpeed)
  end
end

local function ChaseTarget(target)
  if target and (target.Position - Dummy.Position).magnitude < ChaseDistance then
    MoveToTarget(target)
    Dummy.Humanoid:Move(Dummy.Humanoid.MoveDirection * RunSpeed)
  else
    CurrentState = "Roam"
  end
end

local function HandleDamage(humanoid, damage)
  if humanoid == Dummy.Humanoid then
    CurrentState = "Chase"
    ChaseTarget(humanoid.Parent) -- Assuming damage source is a character
  end
end

RunService.Heartbeat:Connect(function()
  if CurrentState == "Roam" then
    local targetPoint = GetRandomPointInRadius()
    MoveToTarget(targetPoint)
    LookAtTarget(Workspace.CurrentCamera.LookVector)
  elseif CurrentState == "Chase" then
    ChaseTarget(Workspace.CurrentCamera) -- Chase the player
  end
end)

Dummy.Humanoid.TookDamage:Connect(HandleDamage)

-- You can add more states and behaviors here,
-- like reacting to obstacles, hiding, or using cover.

-- Remember to replace placeholders like "Dummy" and adjust values based on your setup.
```

  
