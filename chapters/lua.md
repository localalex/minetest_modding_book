---
title: Lua Scripts
layout: default
root: ../
---

<div class="notice">
	<h2>This chapter is incomplete</h2>

	The wording or phrasing may be hard to understand.
	Don't worry, we're working on it.
</div>

Introduction
------------

In this chapter we will talk about scripting in Lua, the tools required,
and go over some techniques which you will probably find useful.

This chapter will assume that you have had some programming experience before,
even Scratch level is acceptable.

* Tools
	* Integrated Programming Environments
* Local and Global
* Including other Lua Scripts

Tools
-----

A text editor with code highlighting is sufficient for writing scripts in Lua.
Code highlighting gives different words and characters different colors in order to
make it easier to read the code and spot any mistakes.

{% highlight lua %}
function ctf.post(team,msg)
	if not ctf.team(team) then
		return false
	end
	if not ctf.team(team).log then
		ctf.team(team).log = {}
	end

	table.insert(ctf.team(team).log,1,msg)
	ctf.save()

	return true
end
{% endhighlight %}

For example, keywords in the above snippet are highlighted, such as if, then, end, return.
table.insert is a function which comes with Lua by default.

### Integrated Programming Environments

IDEs allow you to debug code like a native application.
These are harder to set up than just a text editor.

One such IDE is Eclipse with the Koneki Lua plugin:

* Install Eclipse + Koneki.
* Create a new Lua project from existing source (specify Minetest's base directory).
* Follow instructions from Koneki wiki how to do "Attach to remote Application" debugging (just a few steps).
* It is suggested to add those lines from wiki at beginning of builtin.lua.
* Start the debugger (set "Break on first line" in debugger configuration to see if it is working).
* Start Minetest.
* Enter the game to startup Lua.

Local and Global
----------------

Whether a variable is local or global determines where it can be written to or read to.
A local variable is only accessible from where it is defined. Here are some examples:

{% highlight lua %}
-- Accessible from within this script file
local one = 1

function myfunc()
	-- Accessible from within this function
	local two = one + one

	if two == one then
		-- Accessible from within this if statement
		local three = one + two
	end
end
{% endhighlight %}

Whereas global variables can be accessed from anywhere in the script file, and from any other mod.

{% highlight lua %}
my_global_variable = "blah"

function one()
	my_global_variable_2 = "blah"
end

{% endhighlight %}


### Locals should be used as much as possible

Lua is global by default (unlike most other programming languages).
Local variables must be identified as such.

{% highlight lua %}
function one()
	foo = "bar"
end

function two()
	print(dump(foo))  -- Output: "bar"
end
{% endhighlight %}

This is sloppy coding, and Minetest will in fact warn you about this:

	[WARNING] Assigment to undeclared global 'foo' inside function at init.lua:2

To correct this, use "local":

{% highlight lua %}
function one()
	local foo = "bar"
end

function two()
	print(dump(foo))  -- Output: nil
end
{% endhighlight %}

The same goes for functions. Functions are variables of a special type.
You should make functions as local as much as possible,
as other mods could have functions of the same name.

{% highlight lua %}
local function foo(bar)
	return bar * 2
end
{% endhighlight %}

If you want your functions to be accessible from other scripts or mods, it is recommended that
you add them all into a table with the same name as the mod:

{% highlight lua %}
mymod = {}

function mymod.foo(bar)
	return foo .. "bar"
end

-- In another mod, or script:
mymod.foo("foobar")
{% endhighlight %}

Including other Lua Scripts
---------------------------

You can include Lua scripts from your mod, or another mod like this:

{% highlight lua %}
dofile(minetest.get_modpath("modname") .. "/script.lua")
{% endhighlight %}

"local" variables declared outside of any functions in a script file will be local to that script.
You won't be able to access them from any other scripts.



{% highlight lua %}
{% endhighlight %}