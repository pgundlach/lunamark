#!/usr/bin/env lua

require "luarocks.require"
require "lunamark"

-- from Programming Lua
local function expandTabs(s, tabstop)
  local tab = tabstop or 4
  local corr = 0
  return (string.gsub(s, "()\t", function(p)
          local sp = tab - (p - 1 + corr)%tab
          corr = corr - 1 + sp
          return string.rep(" ",sp)
        end))
end

local function read_and_expand_tabs()
  local buffer = {}
  for line in io.lines() do
    table.insert(buffer, expandTabs(line,4))
  end
  -- need blank line at end to emulate Markdown.pl
  table.insert(buffer, "\n")
  return table.concat(buffer,"\n")
end

numargs = table.getn(arg)
if numargs > 0 then
  io.input(arg[1])
end

inp = read_and_expand_tabs()

local convert = lunamark.converter("markdown", "html")

io.write(convert(inp))

