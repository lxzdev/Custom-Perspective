# Custom-Perspective
Inspired by the game Animal Crossing 

ReplicatedStorage/Player

CameraModule
export type CameraType = 'Moving' | 'Idle'

local CameraController = {}
CameraController.Type = 'Moving'

local Players = game:GetService('Players')
local Workspace = game:GetService('Workspace')
local RunService = game:GetService('RunService')
local ReplicatedStorage = game:GetService('ReplicatedStorage')

local Constants = require(ReplicatedStorage.Constants)

local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local Camera = Workspace.CurrentCamera

function CameraController.SetCamera(Type: CameraType): ()
	CameraController.Type = Type
end

function CameraController.ComputeCFrame(Type: CameraType): CFrame
	local CharacterPivot = Character:GetPivot()
	
	return CFrame.new(CharacterPivot.Position) * Constants.CAMERA.OFFSET_BY_TYPE[Type]
end

local function Initialize()
	--> Init Properties
	Camera.FieldOfView = Constants.CAMERA.DEFAULT_FOV
	Camera.CameraType = Enum.CameraType.Scriptable
	
	--> Calling Side Functions
	CameraController.SetCamera('Moving')
	
	--> Events & Update
	LocalPlayer.CharacterAdded:Connect(function(AddedCharacter: Model)
		Character = AddedCharacter
	end)
	
	RunService.RenderStepped:Connect(function(DeltaTime: number)
		--> Init Guard Clause
		if not LocalPlayer.Character then return end

		--> Computing CFrame & Changing Properties
		local ComputedCFrame = CameraController.ComputeCFrame(CameraController.Type)
		
		Camera.CFrame = Camera.CFrame:Lerp(ComputedCFrame, Constants.CAMERA.SMOOTHNESS)
	end)
end

Initialize()
return CameraController

Init

local ReplicatedStorage = game:GetService('ReplicatedStorage')

for _, Module in ReplicatedStorage:GetDescendants() do
	if not Module:IsA('ModuleScript') then continue end
	require(Module)
end

task.defer(function()
	script:Destroy()
end)

Constants Module

local Constants = table.freeze{
	
	PLAYER = {
		
		
		PLAYER_NORMAL_SPEED = 16,
		PLAYER_RUN_SPEED = 20,
	},
	
	
	CAMERA = {
		DEFAULT_FOV = 45,
		OFFSET_BY_TYPE = {
			Moving = CFrame.new(0, 9.5, 20) * CFrame.Angles(-math.rad(27.5), 0, 0),
			Idle = CFrame.new(0, 3.5, 20) * CFrame.Angles(-math.rad(15.5), 0, 0)
		},
		SMOOTHNESS = 0.075
	}
	
	
	
}

return Constants



