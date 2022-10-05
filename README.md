# Dual-Universe-DU-Creators
Projects Listed as on DU Creators

## Tile Evaluation (LUA DEMO)

### Setup: 1) Load code below on to a program board. 
### Setup: 2) Link the program board to each Mining unit
### Setup: 3) F click to activate (run program board)

### unit.onStart()
```lua
system.print("----------------------------------------")
system.print("MU Monitor MK 0.01.01")
system.print("----------------------------------------")

machines = {}
if slot1 ~= nil then
    table.insert(machines, slot1)
end
if slot2 ~= nil then
    table.insert(machines, slot2)
end
if slot3 ~= nil then
    table.insert(machines, slot3)
end
if slot4 ~= nil then
    table.insert(machines, slot4)
end
if slot5 ~= nil then
    table.insert(machines, slot5)
end
if slot6 ~= nil then
    table.insert(machines, slot6)
end
if slot7 ~= nil then
    table.insert(machines, slot7)
end
if slot8 ~= nil then
    table.insert(machines, slot8)
end
if slot9 ~= nil then
    table.insert(machines, slot9)
end
if slot10 ~= nil then
    table.insert(machines, slot10)
end

r = {}
r[262147665] = "Bauxite"
r[299255727] = "Coal"
r[3724036288] = "Quartz"
r[4234772167] = "Hematite"

r[2029139010] = "Chromite"
r[3086347393] = "Limestone"
r[2289641763] = "Malachite"
r[343766315] = "Natron"

r[3837858336] = "Petalite"
r[4041459743] = "Pyrite"
r[1065079614] = "Garnierite"
r[1050500112] = "Acanthite"

r[3546085401] = "Cobaltite"
r[1467310917] = "Cryolite"
r[1866812055] = "Gold nuggets"
r[271971371] = "Kolbeckite"

r[629636034] = "Ilmenite"
r[3934774987] = "Rhodonite"
r[789110817] = "Columbite"
r[2162350405] = "Vanadinite"    


p = {}
p[262147665] = 30
p[299255727] = 30
p[3724036288] = 30
p[4234772167] = 30

p[2029139010] = 50
p[3086347393] = 50
p[2289641763] = 50
p[343766315] = 50

p[3837858336] = 125
p[4041459743] = 125
p[1065079614] = 125
p[1050500112] = 125

p[3546085401] = 250
p[1467310917] = 250
p[1866812055] = 250
p[271971371] = 250

p[629636034] = 500
p[3934774987] = 500
p[789110817] = 500
p[2162350405] = 500

function ov(id)
        
    -- Return the name, or unknown
    value = p[id]
    if value ~= nil then
        return p[id]
    else
        return 1
    end
    return 0
end

function rn(id)
    
    -- TODO: pull Display Info from system.
    --info = system.getItem(id)
    --dump(info)
    
    -- Return the name, or unknown
    name = r[id]
    if name ~= nil then
        return r[id]
    else
        return string.format("Unknown ID : %d", id)
    end
    return "Error"
end

function dump(o)
   if type(o) == 'table' then
      local s = '{ '
      for k,v in pairs(o) do
         if type(k) ~= 'number' then k = '"'..k..'"' end
         s = s .. '['..k..'] = ' .. dump(v) .. ','
      end
      return s .. '} '
   else
      return tostring(o)
   end
end

function rt(f)
    max_rate = 230 --export
    --return string.format("%.2f %%", f/max_rate)
    return f/max_rate
end
    
function ffs(f)      
    -- 100,690.12 %
    local i, j, minus, int, fraction = tostring(f):find('([-]?)(%d+)([.]?%d*)')
    int = int:reverse():gsub("(%d%d%d)", "%1,")
    --return minus .. int:reverse():gsub("^,","") .. fraction
    return minus .. int:reverse():gsub("^,","") 
end

function ffr(f)
    -- 69.12 L/Hr
    return string.format("%.2f L/Hr", f)
end

function ffp(f)
    -- 69.12 %
    return string.format("%.2f %%", f*100)
end

function tm(value)
    -- Hours : Minutes : Seconds
    hours = value / 3600
    hours = math.floor(hours)
    value = value - hours * 3600
    minutes = value / 60
    minutes = math.floor(minutes)
    value = value - minutes * 60
    
    -- to Float
    --value = value * 1.01
    --value = value / 1.01
    
    return string.format("%02d:%02d:%02d", hours, minutes, value)
end

-- Main
--tile_value = 0
tile_value_rate = 0
tile_harvest_value = 0
system.print("----------------------------------------")
for k,m in ipairs(machines) do
    --m.startMaintain(1)
    name = m.getName()
    ore = m.getActiveOre()
    time = m.getRemainingTime()
    pid = m.getLastExtractingPlayerId()
    pname = system.getPlayerName(pid)
    harvest = m.getLastExtractedVolume()
    -- debug
    -- oids = player.getOrgsIds()
    -- dump(oid)
    
    rate = m.getProductionRate()
    bonus = m.getAdjacencyBonus()
    
    value = ov(ore)
    --tile_value += value
    tile_value_rate = tile_value_rate + (rate*value)
    tile_harvest_value = tile_harvest_value + (value * harvest) 
    system.print()
    
    system.print("Name : "..name.. " Player : "..pname)
    system.print("Ore : "..rn(ore).." Value : "..value.." Harvest : "..ffs(harvest))
    system.print("Rate: "..ffr(rate).." Max Rate : "..ffp(rt(rate)).." Bonus : "..ffp(bonus))
    system.print("Value Per Hour: "..ffs(rate*value))    
    system.print("----------------------------------------") 
end
system.print("Tile Hr Value  : $ "..ffs(tile_value_rate))
system.print("Weekly Profit  : $ "..ffs(tile_value_rate*24*7-500000))
system.print("----------------------------------------")
system.print("Harveset Value : $ "..ffs(tile_harvest_value))
system.print("----------------------------------------")
system.print("Total Value    : $ "..ffs(tile_value_rate*24*7-500000 + tile_harvest_value))
system.print("----------------------------------------")
unit.exit()
```
