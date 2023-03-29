Mode = "VERTIKAL" -- Mode room ( VERTIKAL / HORIZONTAL )

--             HORIZONTAL = [][][]

--             VERTIKAL =   []
--                          []
--                          []









-->             TIDAK USH DI UBAH              <--
function FChat(txt)
  p = {}
  p[0] = "OnTextOverlay"
  p[1] = txt
  SendVariantList(p)
end

FChat("`0[`4EXECUTED`0] `9Grow Script Community PROXY")
tax = 0
wls = 0
play = 0
cvbgl = 0
function printa(v)
  if v[0] == "OnConsoleMessage" and v[1]:find("spun the wheel") then
    if v[1]:find("<") and v[1]:find(">") then
      p = {}
      p[0] = "OnConsoleMessage"
      p[1] = " `0[`4FAKE`0] " .. v[1]
      SendVariantList(p)
      return true
    else
      p = {}
      p[0] = "OnConsoleMessage"
      p[1] = " `0[`2REAL`0] " .. v[1]
      SendVariantList(p)
      return true
    end
  elseif v[0] == "OnConsoleMessage" and v[1]:find("Collected ") then
    if v[1]:find("Diamond Lock") then
      play = tonumber(v[1]:match("(%d+) Diamond Lock")) * 100
      countTax(play * 2)
      msg = "`0[`bPI PROXY`0] " .. v[1]
      p = {}
      p[0] = "OnConsoleMessage"
      p[1] = msg
      SendVariantList(p)
      return true
    else
      msg = "`0[`bPI PROXY`0] " .. v[1]
      p = {}
      p[0] = "OnConsoleMessage"
      p[1] = msg
      SendVariantList(p)
    end
  elseif v[0] == "OnSDBroadcast" then
    LogToConsole("`0[`bPI PROXY`0] Blocked SDB")
    return true
  elseif v[0] == "OnConsoleMessage" then
    msg = "`0[`bPI PROXY`0] " .. v[1]
    p = {}
    p[0] = "OnConsoleMessage"
    p[1] = msg
    SendVariantList(p)
    return true
  elseif v[0] == "OnTalkBubble" and v[2]:find("spun the wheel") then
    p = {}
    p[0] = "OnTalkBubble"
    p[1] = v[1]
    p[2] = v[2]
    p[3] = 0
    p[4] = 0
    SendVariantList(p)
    return true
  end
  return false
end

AddHook("onvariant", 1, printa)


function drop(id, amt)
  SendPacket(2, "action|dialog_return\ndialog_name|drop\nitem_drop|" .. id .. "|\nitem_count|" .. amt)
  Sleep(180)
end

function findItem(id)
  for _, itm in pairs(GetInventory()) do
    if itm.id == id then
      return itm.amount
    end
  end
  return 0
end

function startBET()
  FindPath(poswin1x, poswin1y)
  Sleep(200)
  FindPath(poswin2x, poswin2y)
  Sleep(500)
  FindPath(poshostx, poshosty)
  SendPacket(2, "action|input\ntext|Gas")
end

function relogThread()
  FChat("Relog...")
  latest_world = GetWorld().name
  SendPacket(3, "action|quit_to_exit")
  Sleep(500)
  RequestJoinWorld(latest_world)
end

function countTax(amount)
  if tonumber(tax) < 10 then
    taxs = tonumber(tax) / 100
    total = math.floor(amount - (amount * taxs))
    FChat("`9" .. tostring(total) .. " WLS `2" .. tostring(math.floor(amount * taxs)) .. " WLS TAX")
    return total
  else
    total = math.floor(amount - (amount / tonumber(tax)))
    FChat("`9" .. tostring(total) .. " WLS `2" .. tostring(math.floor(amount / tax)) .. " WLS TAX")
    return total
  end
end

function SendMenu()
    menuVarList = {}
    menuVarList[0] = "OnDialogRequest"
    menuVarList[1] = [[
set_default_color|`o
add_label_with_icon|big|`2PI Proxy Command Menu``|left|32
add_spacer|small|
add_textbox|`0~> `9/pos1 `0(Set Position Count Gems)|left|
add_textbox|`0~> `9/pos2 `0(Set Position Count Gems)|left|
add_textbox|`0~> `9/setwin1 `0(Set Position Display Box)|left|
add_textbox|`0~> `9/setwin2 `0(Set Position Display Box)|left|
add_textbox|`0~> `9/sethost `0(Set Position Host)|left|
add_textbox|`0~> `9/mode `0(Set Count Gems Position)|left|
add_spacer|small|
add_textbox|`8-------------- Play Menu|left|
add_textbox|`0~> `9/c `0(Count Gems On Position 1 and 2)|left|
add_textbox|`0~> `9/relog `0(Relog world)|left|
add_textbox|`0~> `9/start `0(Collect WLS from two pos)|left|
add_textbox|`0~> `9/back `0(Teleport to Host Position)|left|
add_textbox|`0~> `9/w1 `0(Teleport to Display Box pos 1)|left|
add_textbox|`0~> `9/w2 `0(Teleport to Display Box pos 2)|left|
add_spacer|small|
add_textbox|`8-------------- Drop Menu|left|
add_textbox|`0~> `9/wd <amount> `0(World Lock Drop)|left|
add_textbox|`0~> `9/dd <amount> `0(Diamond Lock Drop)|left|
add_textbox|`0~> `9/db <amount> `0(Blue Lock Drop)|left|
add_textbox|`0~> `9/cd <amount> `0(Custom Drop) `4Use safely to avoid crash!|left|
add_textbox|`0~> `9/phone `0(Open Telephone menu)|left|
add_spacer|small|
add_textbox|`8-------------- Calculator (Optional)|left|
add_textbox|`0~> `9/tax <amount> `0(Set Tax)|left|
add_textbox|`0~> `9/play <amount> `0(Callculate tax)|left|
add_textbox|`0</nopldx#5898>|left|
end_dialog|Cancel|Host now!|
add_quick_exit]]
    SendVariantList(menuVarList)
end

function customDrop()
  pecahan = { 10000, 100, 1 }
  bgl = 0
  dl = 0
  wl = 0
  for i = 1, 3 do
    while wls >= pecahan[i] do
      if pecahan[i] == pecahan[1] then
        bgl = bgl + 1
        wls = wls - 10000
      elseif pecahan[i] == pecahan[2] then
        dl = dl + 1
        wls = wls - 100
      elseif pecahan[i] == pecahan[3] then
        wl = wl + 1
        wls = wls - 1
      end
    end
  end
  itemamt = { bgl, dl, wl }
  itemid = { 7188, 1796, 242 }
  if findItem(1796) < dl then
    SendPacketRaw(false, { type = 10, value = 7188 })
  elseif findItem(242) < wl then
    SendPacketRaw(false, { type = 10, value = 1796 })
  end
  Sleep(200)
  if bgl > 0 then
    drop(itemid[1], itemamt[1])
    if dl > 0 then
      drop(itemid[2], itemamt[2])
      if wl > 0 then
        drop(itemid[3], itemamt[3])
      end
    end
  else
    if dl > 0 then
      drop(itemid[2], itemamt[2])
      if wl > 0 then
        drop(itemid[3], itemamt[3])
      end
    else
      if wl > 0 then
        drop(itemid[3], itemamt[3])
      end
    end
  end
  LogToConsole("`0[`bPI PROXY`0] Dropped `1" .. bgl .. " BGL " .. dl .. " DLS " .. wl .. " WLS")
end

function place(id, x, y)
  pkt = {}
  pkt.type = 3
  pkt.value = id
  pkt.px = math.floor(GetLocal().pos.x / 32 + x)
  pkt.py = math.floor(GetLocal().pos.y / 32 + y)
  pkt.x = GetLocal().pos.x
  pkt.y = GetLocal().pos.y
  SendPacketRaw(false, pkt)
end

AddHook("onsendpacket", 0, function(type, packet)
  if packet:find("/pos1") then
    if Mode == "VERTIKAL" then
      pos1x1 = math.floor(GetLocal().pos.x / 32)
      pos1y1 = math.floor(GetLocal().pos.y / 32)
      pos1y2 = math.floor(GetLocal().pos.y / 32) - 1
      pos1y3 = math.floor(GetLocal().pos.y / 32) - 2
    elseif Mode == "HORIZONTAL" then
      pos1y1 = math.floor(GetLocal().pos.y / 32)
      pos1x1 = math.floor(GetLocal().pos.x / 32) - 1
      pos1x2 = math.floor(GetLocal().pos.x / 32)
      pos1x3 = math.floor(GetLocal().pos.x / 32) + 1
    end
    FChat("`9[PI] Pos 1 configured")
    return true
  elseif packet:find("/pos2") then
    if Mode == "VERTIKAL" then
      pos2x1 = math.floor(GetLocal().pos.x / 32)
      pos2y1 = math.floor(GetLocal().pos.y / 32)
      pos2y2 = math.floor(GetLocal().pos.y / 32) - 1
      pos2y3 = math.floor(GetLocal().pos.y / 32) - 2
      FChat("`9[PI] Pos 2 configured")
      return true
    elseif Mode == "HORIZONTAL" then
      pos2y1 = math.floor(GetLocal().pos.y / 32)
      pos2x1 = math.floor(GetLocal().pos.x / 32) - 1
      pos2x2 = math.floor(GetLocal().pos.x / 32)
      pos2x3 = math.floor(GetLocal().pos.x / 32) + 1
      FChat("`9[PI] Pos 2 configured")
      return true
    end
  elseif packet:find("/start") then
    RunThread(startBET)
    return true
  elseif packet:find("/setwin1") then
    poswin1x = math.floor(GetLocal().pos.x / 32)
    poswin1y = math.floor(GetLocal().pos.y / 32)
    FChat("`9[PI] Pos Win 1 configured")
    return true
  elseif packet:find("/setwin2") then
    poswin2x = math.floor(GetLocal().pos.x / 32)
    poswin2y = math.floor(GetLocal().pos.y / 32)
    FChat("`9[PI] Pos Win 2 configured")
    return true
  elseif packet:find("/sethost") then
    poshostx = math.floor(GetLocal().pos.x / 32)
    poshosty = math.floor(GetLocal().pos.y / 32)
    FChat("`9[PI] Pos Host configured")
    return true
  elseif packet:find("/back") then
    FindPath(poshostx, poshosty)
    return true
  elseif packet:find("/w1") then
    -- LogToConsole("Teleport to win 1")
    FindPath(poswin1x, poswin1y)
    return true
  elseif packet:find("/w2") then
    -- LogToConsole("Teleport to win 2")
    FindPath(poswin2x, poswin2y)
    return true
  elseif packet:find("/wd (%d+)") then
    wls = tonumber(packet:match("/wd (%d+)"))
    SendPacket(2, "action|dialog_return\ndialog_name|drop\nitem_drop|" .. tostring(242) .. "|\nitem_count|" .. wls)
    LogToConsole("`0[`bPI PROXY`0] Dropped `1" .. wls .. " WLS")
    return true
  elseif packet:find("/dd (%d+)") then
    wls = tonumber(packet:match("/dd (%d+)"))
    SendPacket(2, "action|dialog_return\ndialog_name|drop\nitem_drop|" .. tostring(1796) .. "|\nitem_count|" .. wls)
    LogToConsole("`0[`bPI PROXY`0] Dropped `1" .. wls .. " DLS")
    return true
  elseif packet:find("/db (%d+)") then
    wls = tonumber(packet:match("/db (%d+)"))
    SendPacket(2, "action|dialog_return\ndialog_name|drop\nitem_drop|" .. tostring(7188) .. "|\nitem_count|" .. wls)
    LogToConsole("`0[`bPI PROXY`0] Dropped `1" .. wls .. " BGL")
    return true
  elseif packet:find("/cd (%d+)") then
    wls = tonumber(packet:match("/cd (%d+)"))
    RunThread(customDrop)
    return true
  elseif packet:find("/tax (%d+)") then
    tax = packet:match("/tax (%d+)")
    FChat("`9Tax has been set `0" .. tax .. "%")
    return true
  elseif packet:find("/play (%d+)") then
    amount = tonumber(packet:match("/play (%d+)"))
    countTax(amount)
    return true
  elseif packet:find("/relog") then
    RunThread(relogThread)
    return true
  elseif packet:find("/c") or packet:find("/C") then
    if Mode == "VERTIKAL" then
      count1 = 0
      count2 = 0
      for _, value in pairs(GetObjectList()) do
        if value.id == 112 then
          checkY = math.floor(value.pos.y / 32)
          checkX = math.floor(value.pos.x / 32)
          if checkX == pos1x1 then
            if checkY == pos1y1 or checkY == pos1y2 or checkY == pos1y3 then
              count1 = count1 + value.amount
            end
          end
          if checkX == pos2x1 then
            if checkY == pos2y1 or checkY == pos2y2 or checkY == pos2y3 then
              count2 = count2 + value.amount
            end
          end
        end
      end
      if count1 == count2 then
        SendPacket(2, "action|input\ntext|`2" .. count1 .. " Gems `0/ `2" .. count2 .. " Gems `0[SERI]")
      elseif count1 < count2 then
        SendPacket(2, "action|input\ntext|`a" .. count1 .. " Gems `0/ `2" .. count2 .. " Gems `0[MENANG]")
      elseif count1 > count2 then
        SendPacket(2, "action|input\ntext|`0[WIN] `2" .. count1 .. " Gems `0/ `a" .. count2 .. " Gems")
      end
    elseif Mode == "HORIZONTAL" then
      count1 = 0
      count2 = 0
      for _, value in pairs(GetObjectList()) do
        if value.id == 112 then
          checkY = math.floor(value.pos.y / 32)
          checkX = math.floor(value.pos.x / 32)
          if checkY == pos1y1 then
            if checkX == pos1x1 or checkX == pos1x2 or checkX == pos1x3 then
              count1 = count1 + value.amount
            end
          end
          if checkY == pos2y1 then
            if checkX == pos2x1 or checkX == pos2x2 or checkX == pos2x3 then
              count2 = count2 + value.amount
            end
          end
        end
      end
      if count1 == count2 then
        SendPacket(2, "action|input\ntext|`2" .. count1 .. " Gems `0/ `2" .. count2 .. " Gems `0[SERI]")
      elseif count1 < count2 then
        SendPacket(2, "action|input\ntext|`a" .. count1 .. " Gems `0/ `2" .. count2 .. " Gems `0[MENANG]")
      elseif count1 > count2 then
        SendPacket(2, "action|input\ntext|`0[MENANG] `2" .. count1 .. " Gems `0/ `a" .. count2 .. " Gems")
      end
    end
    return true
  elseif packet:find("wrench_bgl") then
    if cvbgl == 0 then
      cvbgl = 1
      FChat("`5Fast Conver BGL Mode is `2enabled")
      AddHook("onvariant", "changebgl", function(var)
        if var[0] == "OnDialogRequest" and var[1]:find("end_dialog|telephone") then
          x = var[1]:match("x|(%d+)")
          y = var[1]:match("y|(%d+)")
          SendPacket(2,
            "action|dialog_return\ndialog_name|telephone\nnum|53785|\nx|" .. x ..
            "|\ny|" .. y .. "|\nbuttonClicked|bglconvert\n")
          return true
        end
        return false
      end)
    elseif cvbgl == 1 then
      cvbgl = 0
      RemoveHook("changebgl")
      FChat("`9Fast Conver BGL Mode is `4disabled")
    end
  elseif packet:find("/phone") then
    menuVarList = {}
    menuVarList[0] = "OnDialogRequest"
    menuVarList[1] = [[
set_default_color|`o
add_label_with_icon|big|`9Wrench Menu``|left|32
add_spacer|small|
text_scaling_string|iprogramtext
add_button_with_icon|wrench_bgl|`5Convert BGL|staticYellowFrame|32||
add_button_with_icon||END_LIST|noflags|0||
add_spacer|small|
end_dialog|WrenchShortcut|Cancel|
add_quick_exit]]
    SendVariantList(menuVarList)
    return true
  elseif packet:find ("mode_vertikal") then
    Mode = "VERTIKAL"
    FChat("`9Mode set to `0Vertical")
    return true
  elseif packet:find ("mode_horizontal") then
    Mode = "HORIZONTAL"
    FChat("`9Mode set to `0Horizontal")
    return true
  elseif packet:find("/mode") then
    menuVarList = {}
    menuVarList[0] = "OnDialogRequest"
    menuVarList[1] = [[
set_default_color|`o
add_label_with_icon|big|`9Select Mode``|left|32
add_spacer|small|
text_scaling_string|iprogramtext
add_button_with_icon|mode_vertikal|`9Vertikal|staticYellowFrame|646||
add_button_with_icon|mode_horizontal|`9Horizontal|staticYellowFrame|644||
add_button_with_icon||END_LIST|noflags|0||
add_spacer|small|
end_dialog|WrenchShortcut|Cancel|
add_quick_exit]]
    SendVariantList(menuVarList)
    return true
  elseif packet:find("/menu") then
    SendMenu()
    return true
  end
  return false
end)

SendMenu()
