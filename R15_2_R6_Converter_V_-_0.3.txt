--[[	
	
















	
	/ Ｒ１５　２　Ｒ６　ＣＯＮＶＥＲＴＥＲ
	Version - 0.3
	Link - discord.gg/HBzvWE6Rp3
	
	| Convert R15 To R6
	
	
	| 𝐔𝐒𝐄 𝐀𝐓 𝐘𝐎𝐔𝐑 𝐎𝐖𝐍 𝐑𝐈𝐒𝐊
	| 𝐖𝐎𝐑𝐊 𝐈𝐍 𝐏𝐑𝐎𝐆𝐑𝐄𝐒𝐒	
	
















	
]]

-- Settings
local Method = 1 -- 1/Maximum (2 is Maximum)
local TorsoType = "Male" -- Male/Female
local RobloxShirt = false -- true/false
local SizeLock = false -- true/false
local R15PartTransparency = true -- true/false
local R6PartTransparency = false -- true/false
local Target = game.Players.LocalPlayer -- game.Players:WaitForChild("OtherPlayerName")
--

























local Method2CharacterFolder = game.Workspace:FindFirstChild("Method2CharacterFolder")

if not Method2CharacterFolder then
	Method2CharacterFolder = Instance.new("Folder", game.Workspace)
	Method2CharacterFolder.Name = "Method2CharacterFolder"
end

local RS = game:GetService("RunService")
local IS = game:GetService("InsertService")

local R15Size = {
	["UpperTorso"] = Vector3.new(2, 1.6, 1),
	["UpperTorsoFemale"] = Vector3.new(2.043, 1.796, 1.01),
	["RightLowerArm"] = Vector3.new(1, 1.052, 1),
	["LeftLowerArm"] = Vector3.new(1, 1.052, 1),
	["RightLowerLeg"] = Vector3.new(1, 1.193, 1),
	["LeftLowerLeg"] = Vector3.new(1, 1.193, 1),
}

local R15Transparency = {
	"UpperTorso",
	"LowerTorso",
	"RightUpperArm",
	"RightLowerArm",
	"RightHand",
	"LeftUpperArm",
	"LeftLowerArm",
	"LeftHand",
	"RightUpperLeg",
	"RightLowerLeg",
	"RightFoot",
	"LeftUpperLeg",
	"LeftLowerLeg",
	"LeftFoot",
}

local Method2BodyPart = {
	"UpperTorso",
	"RightLowerArm",
	"LeftLowerArm",
	"RightLowerLeg",
	"LeftLowerLeg",

	"Head",
}

local R6Size = {
	["Head"] = Vector3.new(2, 1, 1),
	["Torso"] = Vector3.new(2, 2, 1),
	["Left Arm"] = Vector3.new(1, 2, 1),
	["Left Leg"] = Vector3.new(1, 2, 1),
	["Right Arm"] = Vector3.new(1, 2, 1),
	["Right Leg"] = Vector3.new(1, 2, 1),
}

local WeldCFrame = {
	["Torso"] = CFrame.new(0, -0.2, 0),
	["Right Arm"] = CFrame.new(0, 0.2, 0),
	["Left Arm"] = CFrame.new(0, 0.2, 0),
	["Right Leg"] = CFrame.new(0, 0.2, 0),
	["Left Leg"] = CFrame.new(0, 0.2, 0)
}

local ConvertPart = {
	["Torso"] = "UpperTorso",
	["Right Arm"] = "RightLowerArm",
	["Left Arm"] = "LeftLowerArm",
	["Right Leg"] = "RightLowerLeg",
	["Left Leg"] = "LeftLowerLeg"
}

local R6Mesh = {
	["TorsoMale"] = "rbxassetid://456901040",
	["TorsoFemale"] = "rbxassetid://9747912904",
	["Right Arm"] = "rbxassetid://5062992824",
	["Left Arm"] = "rbxassetid://5062992824",
	["Right Leg"] = "rbxassetid://5062992824",
	["Left Leg"] = "rbxassetid://5062992824"
}

local ConvertedPart = {}
local ShirtPart = {}

function MultiplyCalculate(Base, Default)
	local X = Base.X / Default.X
	local Y = Base.Y / Default.Y
	local Z = Base.Z / Default.Z

	return X,Y,Z
end

function Converter(Character)
	for _, v in pairs(Character:GetChildren()) do
		if v:IsA("BasePart") and table.find(R15Transparency, v.Name) and R15PartTransparency then
			v.Transparency = 1
		end
	end
	
	if Method == 2 then
		local OldCharacter = Character

		Character = Method2CharacterFolder:FindFirstChild(OldCharacter.Name)

		if not Character then
			Character = Instance.new("Model", Method2CharacterFolder)
			Character.Name = OldCharacter.Name

			local Humanoid = Instance.new("Humanoid", Character)
			Humanoid.RigType = Enum.HumanoidRigType.R6
			Humanoid.PlatformStand = true

			local CharacterValue = Instance.new("ObjectValue", Character)
			CharacterValue.Value = OldCharacter

			for _, v in pairs(CharacterValue.Value:GetChildren()) do
				if v:IsA("BasePart") and table.find(Method2BodyPart, v.Name) then
					local Part
					
					if v.Name ~= "Head" then
						Part = Instance.new("Part", Character)
					else
						Part = Instance.new("MeshPart", Character)
					end
					
					Part.Size = v.Size
					Part.Name = v.Name
					Part.Transparency = 1
					Part.CanCollide = false
					Part.CanQuery = false
					Part.CanTouch = false
					Part.Massless = true
					Part.Color = v.Color

					local Weld = Instance.new("Weld", Part)
					Weld.Part0 = v
					Weld.Part1 = Part
				end
			end
		end
	end

	for New, Base in pairs(ConvertPart) do
		local BasePart = Character:FindFirstChild(Base)

		if BasePart then
			local Part

			if New == "Torso" then
				Part = IS:CreateMeshPartAsync(R6Mesh[New..TorsoType], Enum.CollisionFidelity.Box, Enum.RenderFidelity.Performance)
			else
				Part = IS:CreateMeshPartAsync(R6Mesh[New], Enum.CollisionFidelity.Box, Enum.RenderFidelity.Performance)
			end

			if R6PartTransparency then
				Part.Transparency = 1
			end

			Part.Name = New
			Part.Color = BasePart.Color
			Part.CanCollide = false
			Part.CanQuery = false
			Part.CanTouch = false
			Part.Massless = false
			Part.Size = R6Size[New]
			Part.Material = Enum.Material.SmoothPlastic

			local Weld = Instance.new("Weld", Part)
			Weld.Part0 = BasePart
			Weld.Part1 = Part
			Weld.C0 = WeldCFrame[New]

			Part.Parent = Character

			ConvertedPart[New] = {Part = Part, Weld = Weld, Base = BasePart}
		end
	end

	if RobloxShirt then
		local Shirt = Character:FindFirstChildOfClass("Shirt")
		local Pants = Character:FindFirstChildOfClass("Pants")
		local ShirtGraphic = Character:FindFirstChildOfClass("ShirtGraphic")

		if not Shirt and not Pants and not ShirtGraphic then return end

		local ShirtModel = Instance.new("Model")
		local Humanoid = Instance.new("Humanoid", ShirtModel)
		Humanoid.RigType = Enum.HumanoidRigType.R6
		Humanoid.PlatformStand = true

		if Shirt then
			Shirt:Clone().Parent = ShirtModel
		end
		if Pants then
			Pants:Clone().Parent = ShirtModel
		end
		if ShirtGraphic then
			ShirtGraphic:Clone().Parent = ShirtModel
		end

		if TorsoType == "Female" then
			local CharacterMesh = Instance.new("CharacterMesh", ShirtModel)
			CharacterMesh.BodyPart = Enum.BodyPart.Torso
			CharacterMesh.MeshId = 48112070
		end

		ShirtModel.Name = "ShirtModel"
		ShirtModel.Parent = Character

		for New, Base in pairs(ConvertPart) do
			local BasePart = Character:FindFirstChild(New)

			if BasePart then
				local Part = Instance.new("Part")
				Part.Name = New
				Part.Color = BasePart.Color
				Part.CanCollide = false
				Part.CanQuery = false
				Part.CanTouch = false
				Part.Massless = false
				Part.Size = BasePart.Size + Vector3.new(0.01,0.01,0.01)

				local Weld = Instance.new("Weld", Part)
				Weld.Part0 = BasePart
				Weld.Part1 = Part

				Part.Parent = ShirtModel

				ShirtPart[New] = {Part = Part, Base = BasePart}
			end
		end
	end
end

function Convert(Player)
	if Player.Character then
		Converter(Player.Character)
	end

	Player.CharacterAdded:Connect(function(Character)
		repeat task.wait(0.1) until Character:FindFirstChild("HumanoidRootPart")
		ConvertedPart = {}

		Converter(Character)
	end)
end

RS.RenderStepped:Connect(function()
	for _, v in pairs(Method2CharacterFolder:GetChildren()) do
		local CharacterValue = v:FindFirstChildOfClass("ObjectValue")

		if CharacterValue and CharacterValue.Value.Parent == nil then
			v:Destroy()
		end
	end
	
	for Name, Property in pairs(ConvertedPart) do
		local Part = Property.Part
		local Weld = Property.Weld
		local Base = Property.Base
		local BaseSize = R15Size[Base.Name]

		if Base:IsA("MeshPart") and Base.MeshId == "http://www.roblox.com/asset/?id=542765884" then
			BaseSize = R15Size["UpperTorsoFemale"]
		end

		local XMultiply, YMultiply, ZMultiply = MultiplyCalculate(Base.Size, BaseSize)

		Part.Color = Base.Color

		if SizeLock == false then
			Part.Size = Vector3.new(R6Size[Name].X * XMultiply, R6Size[Name].Y * YMultiply, R6Size[Name].Z * ZMultiply)
			Weld.C0 = CFrame.new(WeldCFrame[Name].Position.X * XMultiply, WeldCFrame[Name].Position.Y * YMultiply, WeldCFrame[Name].Position.Z * ZMultiply) * WeldCFrame[Name].Rotation
		end
	end

	for Name, Property in pairs(ShirtPart) do
		local Part = Property.Part
		local Base = Property.Base

		Part.CanCollide = false
		Part.Color = Base.Color

		if SizeLock == false then
			Part.Size = Base.Size + Vector3.new(0.01,0.01,0.01)
		end
	end
end)

if RS:IsStudio() then
	return Convert
else
	if RS:IsClient() then
		Convert(Target)
	elseif RS:IsServer() then
		Convert(game.Players:WaitForChild("FileOfThorn"))
	end
end