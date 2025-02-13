# Welcome to Blueberry Engine!
goo goo ga ga, baby documentation, goo goo ga ga, not ready, goo goo ga ga.

## How to use Custom Character support
It's pretty simple, if you know how (Psych Engine)[https://github.com/ShadowMario/FNF-PsychEngine]'s Custom Character support you can figure this out easily by just reading the Character Json's in `assets/shared/data/characters`. But because this is a tutorial I do have to actually WRITE how to use it, damn it.

So for context, here's part of Daddy Dearest's Character file.
```json
{
  "imagePath": "characters/DADDY_DEAREST.png",
  "xmlPath": "characters/DADDY_DEAREST.xml",
  "Position": [0, 0],
  "CamPosition": [0, 0],
  "animations": [
    {
      "prefix": "Dad idle dance",
      "name": "idle",
      "fps": 24,
      "indices": null,
      "offsets": [0, 0]
    },
    {
      "prefix": "Dad Sing Note UP",
      "name": "singUP",
      "fps": 24,
      "indices": null,
      "offsets": [-6, 50]
    }
  ],
	"healthBarColor": [
		175,
		102,
		206
	],
  "antialiasing": true,
  "singHold": 6.1,
  "size": 1
}
```

Here's what the shits mean:
- `imagePath`: The path to the image file, excluding `assets/images` or `/images/`, don't put that shit.
- `xmlPath`: Basically `imagePath` but for XML's.
- `Position`: The X and Y offsets for the Character.
- `CamPosition`: The X and Y offsets for the Camera when the Character is focused on.
- `animations`: The array of animations.
    - `prefix`: Animation name within XML File.
    - `name`: Animation name in-game.
    - `fps`: Animation Framerate.
    - `indices`: What frames to play specifically.
    - `offsets`: Animation offsets.
- `healthBarColor`: RGB Values for the Health Bar Color.
- `antialiasing`: Antialiasing.
- `singHold`: Not exactly sure what it does to be honest. My theory because I'm too lazy to check is that it's the amount of frames to offset the animation when there's a sustain note or something.
- `size`: Scale of the Character.

If you're wondering how Icons work, just make an Icons folder in your Mods `images` folder and then put an icon with a name like this: `icon-character.png`. Putting a dash in the character name will allow any characters with the first part of the name to use this icon no matter what.

## How to use Custom Stage support
In your mods data folder, create a folder named "stages". now make a new folder within the stages folder, naming the folder what you want the stages name to be.
Now that you have your stage folder ready, create a JSON named `data` and a hx file named `script`.

The JSON data should contain the following data:
```json
{
    "defaultZoom": 0.9,
    "spawnGirlfriend": true,

    "boyfriend": [770, 100],
    "girlfriend": [400, 130],
    "dad": [100, 100]
}
```
Should be self-explanatory.

For the script information, scroll down.

## How to use Hscript Support.
To use Hscript you must have basic and sometimes intermediate knowledge of Haxe and HaxeFlixel, The engine provides several hooks that modders can use to insert custom behavior into the game. The following hooks are currently available:

Avaliable on all:
- `create()`: Called when the gameplay state is created.
- `update(elapsed:Float)`: Called every frame before the gameplay state is updated.
Avaliable on some:
- `createPost()`: Called after the gameplay state is created.
- `updatePost(elapsed:Float)`: Called every frame after the gameplay state is updated.
- `beatHit()`: Called when a beat is hit.
- `stepHit()`: Called when a step is hit.
Avaliable in PlayState:
- `goodNoteHit(note:Note)`: Called when a note is hit correctly.
- `noteTooLate(daNote:Note)`: Called when a note is missed because it was not hit.
- `noteMiss(direction:Int)`: Called when the player makes a misinput.

To add custom behavior to the game, modders can write their own functions and add them to the script. For example:
```haxe
var sprite:FlxSprite;

function create(){
    trace("State created.");

    sprite = new FlxSprite(0, 0);
    sprite.loadGraphic(ModAssets.getGraphic("images/test.png", curMod)); // or you can do `ModAssets.getGraphic("images/test.png", null, "ModIDHere");` to get a graphic within another Mod.
    add(sprite);
}

function update(elapsed){
    // Custom update behavior.
}

function updatePost(elapsed){
    // Custom post update behavior.
}
```

When a script is loaded, it overrides these functions. So if you have 2 scripts that use the same functions, it won't work. There's most likely ways around this but I haven't tested them. I do know you can run functions and even modify variables from other scripts within a script if that script is loaded with it, so you could probably make some sort of host script that contains the functions needed.

### Accessing Haxe and Flixel Classes and Functions.
The engine provides access to various classes, and functions, through global variables.

Here is a list of avaliable global variables:

Classes:
- `Int`
- `String`
- `Float`
- `Array`
- `Bool`
- `Dynamic`
- `Math`
- `FlxMath`
- `Std`
- `StringTools`
- `FlxG`
- `FlxSound`
- `FlxSprite`
- `FlxText`
- `FlxGraphic`
- `FlxTween`
- `FlxEase`
- `FlxCamera`
- `File`
- `FileSystem`
- `Assets`
- `FlxGroup`
- `FlxTypedGroup`
- `Paths`
- `Path`
- `Json`
- `FlxAngle`
- `FlxAtlasFrames`
- `FlxAtlas`
- `Character`
- `Boyfriend`: Not related to the `Boyfriend` character in PlayState, this is the class used for that object.
- `PreferencesMenu`
- `Song`: Not related to the `SONG` variable in PlayState, this is the class used for that object.
- `Conductor`
- `Section`
- `Note`
- `ModLib`: The modding class.
- `ModAssets`: The modding assets class.
- `VideoHandler`: HxCodec Class for Cutscenes.
- `VideoSprite`: HxCodec Class for Cutscenes.

Functions:
- `getModID()`: Returns the current Mod ID.
- `trace(value:Dynamic)`: Logs anything to the console.
- `createThread(func:Void -> Void)`: Creates a thread and runs the given function, if threads are not supported it will run anyways.
- `cacheGraphic(path:String, modID:String)`: Cache a Graphic, if used, it will cause less lag when loading a Graphic.

### Accesssing PlayState Game Objects.
The engine provides access to various objects, classes, and functions, through global variables. Modders can use these variables to modify the PlayState or get information. Here is a list of available global variables:

Classes:
- `StrumNotes`: StrumNotes class.
- `BackgroundDancer`: Background dancer class.
- `BackgroundGirls`: Background girls class.
- `WiggleEffect`: Wiggle effect class.
- `FlxWaveEffect`: Wave effect class.
- `TankmenBG`: Tankmen background class.
- `BGSprite`: Background sprite class.
- `FlxWaveMode`: Wave mode class.

Functions:
- `add(value:FlxObject)`: Adds a FlxObject to the scene.
- `setDefaultZoom(value:Dynamic, ?immediateZoom:Bool = false)`: Sets the default camera zoom.
- `setCameraSpeed(value:Dynamic)`: Sets the camera speed.
- `setGF(value:String)`: Sets the current Girlfriend.
- `curGF()`: Returns the current Girlfriend.
- `createTrail(char:FlxObject, graphic:FlxGraphic, length:Int, delay:Float, alpha:Float, diff:Float, ?addInGroup:Bool, ?group:FlxGroup)`: Creates a trail effect.
- `replaceStrum(style:String, ?skipTransition:Bool, ?dontGenCPU:Bool)`: Replace all strumlines with given data, Generates with no Preferences attatched.
- `addCharacter(charName:String, charID:Int = 0, isDad = false, xOff:Float, yOff:Float)`: Add another Character to the stage.
- `removeCharacter(charID:Int = 0, isDad:Bool = false)`: Remove a Character from the stage.
- `getCharFromID(id:Int, isDad:Bool = false)`: Get a Character from a specified ID.

Objects:
- `camHUD`: The HUD Camera.
- `camGame`: The Game Camera.
- `camFollow`: The object camGame follows.
- `playerStrums`: Player Strums.
- `cpuStrums`: CPU Strums.

Groups:
- `strumLines`: All Strums.
- `stageLayer0`: Layer 0 of the stage - Behind all.
- `stageLayer1`: Layer 1 of the stage - Above GF.
- `stageLayer2`: Layer 2 of the stage - Above All.
- `boyfriendGroup`: The group of BF's.
- `dadGroup`: The group of Dad's.
- `gfGroup`: The group of GF's.

Characters:
- `boyfriend`: The `Boyfriend` character.
- `dad`: The `Dad` character.
- `gf`: The `GF` character.

Shaders:
- `BuildingShaders`: Building shader class.
- `ColorSwap`: Color swap shader class.

Misc:
- `curMod`: The current mod-data loaded.
- `SONG`: The current song data.
- `curSong`: The current song name.
- `curStage`: The current stage name.
- `gfVersion`: The current Girlfriend.
- `inCutscene`: Whether or not the game is in a cutscene.
- `curBeat`: The current beat number.
- `curStep`: The current step number.

### Scripts for Mid-song Events in PlayState
Same variables as PlayState except the only function that can be called is `event`, path can be anywhere though aslong as it's in the Mod's directory.

Example:
```haxe
function event(){
    trace('this is all i can do, how lame am i right?');
}
```

### What it's used for, and Advice.
Currently, Hscript is used for making Stages and for any purpose you have in-song. You put scripts in `data/stages` or in `data/charts/SONG_NAME`.

For stages you have to name your script `script.hx` or it will not be detected, all scripts within your songs folder will be detected automatically.

I would recommend reading the code in `source/game/PlayState.hx`, `source/engine/modutil/Hscript.hx`, `source/engine/modding/SpunModLib.hx`, and the code for the variables you have access to for more information.
