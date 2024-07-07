local HttpService = game:GetService('HttpService')
local Request = (syn and syn.request) or request or (http and http.request) or http_request

while true do
    print("Xr Tool Logdes Online!")
    
    local gemsSuccess, gems = pcall(function()
        local data = game.ReplicatedStorage.Remotes.GetInventory:InvokeServer()
        return tonumber(data.Currencies.Gems)
    end)
    
    local levelSuccess, level = pcall(function()
        local data = game.ReplicatedStorage.Remotes.GetInventory:InvokeServer()
        return tonumber(data.Level)
    end)

    local CySuccess, Cy = pcall(function()
        local data = game.ReplicatedStorage.Remotes.GetInventory:InvokeServer()
        return tonumber(data.Items['Trait Crystal'])
    end)

    local Map 
    local IntroGui = game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChild("MatchIntroGui")
    if IntroGui then
        Map = "User is IN Game !"
    else 
        Map = "User is IN Lobby"
    end
    
    local data
    if gemsSuccess and levelSuccess and CySuccess then
        data = {
            Username = game:GetService('Players').LocalPlayer.Name,
            Gems = gems,
            lv = level,
            Reroll = Cy,
            Map = Map
        }
    else
        data = {
            Username = game:GetService('Players').LocalPlayer.Name,
            Gems = "nil",
            lv = "nil",
            Reroll = "nil",
            Map = Map
        }
    end
    
    local jsonData = HttpService:JSONEncode(data)
    local url = "http://127.0.0.1:7963/Xrlogdes_Data"
    
    local response = Request({
        Url = url,
        Method = "POST",
        Headers = {
            ["Content-Type"] = "application/json"
        },
        Body = jsonData
    })
    
    if response then
    else
        print("Request failed")
    end
    
    wait(0.1)
end
