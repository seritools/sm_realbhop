sm_realbhop
===========

RealBhop is a SourceMod plugin that aims
to recreate HL1/Quake-like bunnyhopping.

**Note that this plugin was created and tested for CS:GO only.**


Configuration
-------------

sm_realbhop uses the `AutoExecCommand()` function to create its config file
in the `csgo/cfg/sourcemod` folder: `sm_realbhop.cfg`

**Please note:** This plugin uses frame based calculations,
so the config values depend on the server's tickrate.
The default values are suitable for relatively easy bhopping
on 128-tick servers.

This plugin uses three cvars to control how it works:

### sm_realbhop_enabled

Sets whether RealBhop is enabled.

Default: *true*

### sm_realbhop_maxbhopframes

Sets the maximum number of frames the bhop calculation is active after
touching the ground.

Default: *12*

Minimum: *1*

Example: If the player lands and jumps again after less than or
equal to `sm_realbhop_maxbhopframes` frames, RealBhop's velocity calculation
will be used. If the player jumps after more than`sm_realbhop_maxbhopframes`
frames, the game's velocity calculation will be used.
In the case of CS:GO, this means the player's velocity will be set to the
running speed of the equipped weapon.

Setting this value to *1* will effectively disable the mod, since this means
the player would have to get every bhop perfectly,
in which case you wouldn't need this mod. ;)

### sm_realbhop_framepenalty

Sets the velocity penalty multiplier per frame the player jumped too late.

Default: *0.975*

Minimum: *0.0*

This setting might need a longer explanation:

In CS:GO, if a player does not jump exactly one frame after landing,
the player's velocity will be set back to his weapon's running speed,
effectively destroying any speed gains from airstrafing.
(This happpens with disabled stamina system,
with stamina it's even worse.)

In games with proper bunnyhopping the player wouldn't be instantly set to
running speed, but decelerate *over time* to it. This means the player loses
a bit of speed for every frame he jumped too late. With good airstrafing
he still would be able to make this loss up and gain speed in the end.

By calculating the difference between the velocity the player had while landing
and the velocity he has when jumping (after CS:GO has set him
back to running speed) and boosting him by exactly that amount this plugin
effectively disables the speed loss completely.

By just doing that, the player wouldn't be punished for delayed jumping,
so this cvar defines the multiplier that the boost velocity will be
multiplied to once for every frame the player jumped too late:

`[new velocity] = [velocity on jump] + (([landing velocity] -
[velocity on jump]) * framepenalty^FramesTooLate)`

With the default settings, the player loses 2.5% of his velocity per frame,
for example.

A good config takes the tickrate, sv_airaccelerate and this cvar into account.


Further config tips
-------------

To make the bhopping experience really good, please disable the stamina
system in your config:

`sv_staminajumpcost 0.0`

`sv_staminalandcost 0.0`

Furthermore activate the internal cvar sm_enablebunnyhopping with the
help of the sm_cvar command:

`sm_cvar sv_enablebunnyhopping 1`
