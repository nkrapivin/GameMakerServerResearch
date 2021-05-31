# GameMakerServerResearch
DF: CONNECTED (RickyG) anticheat research.

*When your almost-zero-budget fangame has more checks than VAC! Yeah boy it's a RickyG fangame!*

Oh and, all this research doesn't cover the server side of things, obviously, we're not admins.

## expr.dll

### Dangerous things it does:
- Query the MachineGuid.
- Make a series of requests to WMIC, to try and detect VirtualBox(!!!).

### Circumvention

Pretty simple to stub, actually:
```cpp
extern "C" EXPR_API double Initialize() { // @1
	return 1541334.0f; // is a float actually.
}

extern "C" EXPR_API double IsValidContext() { // @2
	// -20 - check fail, 0 - dll is not present (gamemaker), 20 - check OK.
	return 20.0;
}
```

## Dll.dll

### Dangerous things it does:
- Query the MachineGuid.
- Even more WMIC queries than in expr.dll!
- Uses an another API called CMIV2, to probe GPU and stuff.
- Take a list of all opened windows' names, loop through them, and if it contains any of these strings:
  - Cheat Engine
  - OllyDbg
  - CrySearch
  - ArtMoney
  - Squalr
  - Memory Scanner
  
  
  Set a flag somewhere in memory, and do... whatever!
- Try and detect presence of VirtualBox user-mode drivers. (**batshit crazy**)

### Circumvention

Okay so this one is a bit harder, since it's like... has more than just privacy-violating checks.

But strings can be edited in HxD, wounds can heal, and life can become easier.
