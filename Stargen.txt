@Echo off

REM Version 0.1 build 20170831
REM Originally built and currently maintained by u/Nerdnub of Reddit.

setlocal EnableExtensions ENABLEDELAYEDEXPANSION
cls

REM System Generation
REM ===========================================================================
REM This section determines the number of stars in a system. It is heavily
REM weighted towards single star systems, but can generate up to septernary 
REM systems. If no stars are generated, it will generate something from the 
REM Void_Gen section instead. This section also marks the start of the script.
:System_Gen
SET /A STAR_NUM=%RANDOM% * 100 / 32768 + 0
IF %STAR_NUM% LEQ 0 GOTO Void_Gen
IF %STAR_NUM% GEQ 1 IF %STAR_NUM% LEQ 75 SET STAR_NUM=1
IF %STAR_NUM% GEQ 76 IF %STAR_NUM% LEQ 94 SET STAR_NUM=2
IF %STAR_NUM%==95 SET STAR_NUM=3
IF %STAR_NUM%==96 SET STAR_NUM=4
IF %STAR_NUM%==97 SET STAR_NUM=5
IF %STAR_NUM%==98 SET STAR_NUM=6
IF %STAR_NUM%==99 SET STAR_NUM=7
GOTO Star_Gen

REM Void Generation
REM ===========================================================================
REM This section generates objects in the absence of a star being generated.
REM Right now it's pretty boring, but can really easily be expanded to include
REM much more interesting things. This section also does not go to the summary
REM section, and probably really needs to.
:Void_Gen
SET /A VOID_GEN=!RANDOM! * 100 / 32768 + 0
IF !VOID_GEN! LEQ 98 SET VOID_GEN=Debris Field
IF !VOID_GEN!==99 SET VOID_GEN=Dyson Sphere
ECHO !VOID_GEN!
GOTO EOF

REM Star Generation
REM ===========================================================================
REM This section takes each star generated in System Generation and assigns
REM them the following attributes: Size, type, and color, and then assigns
REM a single, easily referenceable variable at the end. Stars that are assigned
REM a size of 0 become either a black hole, a protostar, or a neutron star.
REM After that, they scale up to dwarf, main sequence, giant, and super giant.
:Star_Gen
FOR /L %%a in (%STAR_NUM%,-1,1) DO (
	SET STAR_COLOR_%%a=
	SET /A STAR_SIZE_%%a=!RANDOM! * 100 / 32768 + 0
	
	IF !STAR_SIZE_%%a! GEQ 0 IF !STAR_SIZE_%%a! LEQ 10 SET STAR_SIZE_%%a=0
	IF !STAR_SIZE_%%a! GEQ 11 IF !STAR_SIZE_%%a! LEQ 20 SET STAR_SIZE_%%a=1
	IF !STAR_SIZE_%%a! GEQ 21 IF !STAR_SIZE_%%a! LEQ 75 SET STAR_SIZE_%%a=2
	IF !STAR_SIZE_%%a! GEQ 76 IF !STAR_SIZE_%%a! LEQ 95 SET STAR_SIZE_%%a=3
	IF !STAR_SIZE_%%a! GEQ 96 SET STAR_SIZE_%%a=4
	
	IF !STAR_SIZE_%%a!==0 (
		SET /A STAR_TYPE_%%a=!RANDOM! * 3 / 32768 + 0
		IF !STAR_TYPE_%%a!==0 SET STAR_TYPE_%%a=Black Hole
		IF !STAR_TYPE_%%a!==1 SET STAR_TYPE_%%a=Protostar
		IF !STAR_TYPE_%%a!==2 SET STAR_TYPE_%%a=Neutron Star
		)
	IF !STAR_SIZE_%%a!==1 (
		SET STAR_TYPE_%%a=Dwarf Star
		SET /A STAR_COLOR_%%a=!RANDOM! * 3 / 32768 + 0
		IF !STAR_COLOR_%%a!==0 SET STAR_COLOR_%%a=Brown
		IF !STAR_COLOR_%%a!==1 SET STAR_COLOR_%%a=Red
		IF !STAR_COLOR_%%a!==2 SET STAR_COLOR_%%a=White
		)
	IF !STAR_SIZE_%%a!==2 (
		SET STAR_TYPE_%%a=Main Sequence
		SET /A STAR_COLOR_%%a=!RANDOM! * 5 / 32768 + 0
		IF !STAR_COLOR_%%a!==0 SET STAR_COLOR_%%a=Blue
		IF !STAR_COLOR_%%a!==1 SET STAR_COLOR_%%a=White
		IF !STAR_COLOR_%%a!==2 SET STAR_COLOR_%%a=Yellow
		IF !STAR_COLOR_%%a!==3 SET STAR_COLOR_%%a=Orange
		IF !STAR_COLOR_%%a!==4 SET STAR_COLOR_%%a=Red
		)
	IF !STAR_SIZE_%%a!==3 (
		SET STAR_TYPE_%%a=Giant
		SET /A STAR_COLOR_%%a=!RANDOM! * 3 / 32768 + 0
		IF !STAR_COLOR_%%a!==0 SET STAR_COLOR_%%a=Yellow
		IF !STAR_COLOR_%%a!==1 SET STAR_COLOR_%%a=Red
		IF !STAR_COLOR_%%a!==2 SET STAR_COLOR_%%a=Orange
		)
	IF !STAR_SIZE_%%a!==4 (
		SET STAR_TYPE_%%a=Super Giant
		SET /A STAR_COLOR_%%a=!RANDOM! * 3 / 32768 + 0
		IF !STAR_COLOR_%%a!==0 SET STAR_COLOR_%%a=Blue
		IF !STAR_COLOR_%%a!==1 SET STAR_COLOR_%%a=Red
		IF !STAR_COLOR_%%a!==2 SET STAR_COLOR_%%a=Orange
		)
	IF [!STAR_COLOR_%%a!]==[] (SET STAR_%%a=!STAR_TYPE_%%a!) ELSE (SET STAR_%%a=!STAR_COLOR_%%a! !STAR_TYPE_%%a!)
)
GOTO Planet_Gen

REM Planet Generation
REM ===========================================================================
REM This section simply generates a number of planets per star ranging from 0
REM to as many as 5 for main sequence and larger stars. Size 0 stars and
REM Size 4 (Super Giant) stars only have a 5% chance to generate any planets at
REM all, whereas main sequence stars only have a 25% chance to not generate any
REM planets.
:Planet_Gen
FOR /L %%c in (%STAR_NUM%,-1,1) DO (
		IF !STAR_SIZE_%%c!==0 (
		SET /A PLAN_NUM_%%c=!RANDOM! * 100 / 32768 + 0
		IF !PLAN_NUM_%%c! LEQ 94 SET PLAN_NUM_%%c=0
		IF !PLAN_NUM_%%c! GEQ 95 SET /A PLAN_NUM_%%c=!RANDOM! * 2 / 32768 + 0
		)
	IF !STAR_SIZE_%%c!==1 (
		SET /A PLAN_NUM_%%c=!RANDOM! * 100 / 32768 + 0
		IF !PLAN_NUM_%%c! LEQ 49 SET PLAN_NUM_%%c=0
		IF !PLAN_NUM_%%c! GEQ 50 SET /A PLAN_NUM_%%c=!RANDOM! * 2 / 32768 + 0
		)
	IF !STAR_SIZE_%%c!==2 (
		SET /A PLAN_NUM_%%c=!RANDOM! * 100 / 32768 + 0
		IF !PLAN_NUM_%%c! LEQ 24 SET PLAN_NUM_%%c=0
		IF !PLAN_NUM_%%c! GEQ 25 SET /A PLAN_NUM_%%c=!RANDOM! * 5 / 32768 + 0
		)
	IF !STAR_SIZE_%%c!==3 (
		SET /A PLAN_NUM_%%c=!RANDOM! * 100 / 32768 + 0
		IF !PLAN_NUM_%%c! LEQ 49 SET PLAN_NUM_%%c=0
		IF !PLAN_NUM_%%c! GEQ 50 SET /A PLAN_NUM_%%c=!RANDOM! * 5 / 32768 + 0
		)
	IF !STAR_SIZE_%%c!==4 (
		SET /A PLAN_NUM_%%c=!RANDOM! * 100 / 32768 + 0
		IF !PLAN_NUM_%%c! LEQ 94 SET PLAN_NUM_%%c=0
		IF !PLAN_NUM_%%c! GEQ 95 SET /A PLAN_NUM_%%c=!RANDOM! * 5 / 32768 + 0
		)
	)

GOTO Planet_Type

REM Planet Type Generation
REM ===========================================================================
REM This section takes each planet generated around each star and assigns them
REM one of two attributes based on the star they orbit, making them either a
REM terrestrial planet or a gas giant.
:Planet_Type
FOR /L %%d in (%STAR_NUM%,-1,1) DO (
	IF NOT !PLAN_NUM_%%d!==0 (
			FOR /L %%f in (!PLAN_NUM_%%d!,-1,1) DO (
					IF !STAR_SIZE_%%d!==0 (
						SET /A PLAN_TYPE_%%d=!RANDOM! * 100 / 32768 + 0
						IF !PLAN_TYPE_%%d! LEQ 95 SET PLAN_TYPE_%%d_%%f=Terrestrial
						IF !PLAN_TYPE_%%d! GTR 95 SET PLAN_TYPE_%%d_%%f=Gas Giant
						)
					IF !STAR_SIZE_%%d!==1 (
						SET /A PLAN_TYPE_%%d=!RANDOM! * 100 / 32768 + 0
						IF !PLAN_TYPE_%%d! LEQ 50 SET PLAN_TYPE_%%d_%%f=Terrestrial
						IF !PLAN_TYPE_%%d! GTR 50 SET PLAN_TYPE_%%d_%%f=Gas Giant
						)
					IF !STAR_SIZE_%%d!==2 (
						SET /A PLAN_TYPE_%%d=!RANDOM! * 100 / 32768 + 0
						IF !PLAN_TYPE_%%d! LEQ 50 SET PLAN_TYPE_%%d_%%f=Terrestrial
						IF !PLAN_TYPE_%%d! GTR 50 SET PLAN_TYPE_%%d_%%f=Gas Giant
						)
					IF !STAR_SIZE_%%d!==3 (
						SET /A PLAN_TYPE_%%d=!RANDOM! * 100 / 32768 + 0
						IF !PLAN_TYPE_%%d! LEQ 50 SET PLAN_TYPE_%%d_%%f=Terrestrial
						IF !PLAN_TYPE_%%d! GTR 50 SET PLAN_TYPE_%%d_%%f=Gas Giant
						)
					IF !STAR_SIZE_%%d!==4 (
						SET /A PLAN_TYPE_%%d=!RANDOM! * 100 / 32768 + 0
						IF !PLAN_TYPE_%%d! LEQ 50 SET PLAN_TYPE_%%d_%%f=Terrestrial
						IF !PLAN_TYPE_%%d! GTR 50 SET PLAN_TYPE_%%d_%%f=Gas Giant
						)
				)
			)
		)
	)
)

GOTO Planet_Specs

REM Planet Specs Generation
REM ===========================================================================
REM This section does most of the heavy lifting in planet generation. For each
REM planet around each star, depending on whether it's terrestrial or gas
REM giant, and how big it is, it will assign different attributes. For
REM terrestrial planets, this includes calculating a mass respective to that
REM of Earth, an atmosphere type, the thickness of that atmosphere, and between
REM one and four biomes. While the script doesn't pick up to four unique
REM biomes, any time it selects the same biome more than once could just be
REM considered the degree to which that biome is present on the planet. In
REM addition to the 6 standard biomes described in the core rulebook, if no
REM atmosphere is generated on the planet (or if the planet too small to
REM support an atmosphere) the planet will be assigned the "barren" biome,
REM signifying that it is incapable of supporting life. Beyond that, a gravity
REM category will be assigned based on how large the planet is. If it is
REM smaller than 1.0 Earth masses, it will be assigned low gravity, and if it
REM is larger than 3.0 Earth masses, it will be assigned high gravity. This
REM section also makes a call to an arithmetic subroutine to calculate decimal
REM places for the mass (because batch scripts don't handle floating point math
REM at all). If a planet manages to generate with a mass of 0.0, it will be
REM assigned a new type of "asteroid field". This occurance is fairly rare, but
REM it does happen. Gas giants are much simpler in that they are assigned one
REM of three types depending on mass: gas dwarf, gas giant, and cold gas giant.
:Planet_Specs
FOR /L %%g in (%STAR_NUM%,-1,1) DO (
	FOR /L %%h in (!PLAN_NUM_%%g!,-1,1) DO (
			IF /i !PLAN_TYPE_%%g_%%h!==Terrestrial (
				call :Planet_Size_Calc !RANDOM!*10/32768+0 %%g %%h
				IF !calc_v1! GEQ 1 (
					SET /A PLAN_ATMO1_%%g_%%h=!RANDOM! * 4 / 32768 + 0
					IF !PLAN_ATMO1_%%g_%%h!==0 SET PLAN_ATMO1_%%g_%%h=None
					IF !PLAN_ATMO1_%%g_%%h!==1 SET PLAN_ATMO1_%%g_%%h=Breathable
					IF !PLAN_ATMO1_%%g_%%h!==2 SET PLAN_ATMO1_%%g_%%h=Corrosive
					IF !PLAN_ATMO1_%%g_%%h!==3 SET PLAN_ATMO1_%%g_%%h=Toxic
					SET /A PLAN_ATMO2_%%g_%%h=!RANDOM! * 3 / 32768 + 0
					IF !PLAN_ATMO2_%%g_%%h!==0 SET PLAN_ATMO2_%%g_%%h=Thin
					IF !PLAN_ATMO2_%%g_%%h!==1 SET PLAN_ATMO2_%%g_%%h=Normal
					IF !PLAN_ATMO2_%%g_%%h!==2 SET PLAN_ATMO2_%%g_%%h=Thick
					IF NOT "!PLAN_ATMO1_%%g_%%h!"=="None" (
						SET /A PLAN_BIOME_%%g_%%h=!RANDOM! * 100 / 32768 + 0
						IF !PLAN_BIOME_%%g_%%h! GEQ 0 IF !PLAN_BIOME_%%g_%%h! LEQ 25 SET PLAN_BIOME_%%g_%%h_NUM=1
						IF !PLAN_BIOME_%%g_%%h! GEQ 26 IF !PLAN_BIOME_%%g_%%h! LEQ 50 SET PLAN_BIOME_%%g_%%h_NUM=2
						IF !PLAN_BIOME_%%g_%%h! GEQ 51 IF !PLAN_BIOME_%%g_%%h! LEQ 75 SET PLAN_BIOME_%%g_%%h_NUM=3
						IF !PLAN_BIOME_%%g_%%h! GEQ 76 IF !PLAN_BIOME_%%g_%%h! LEQ 99 SET PLAN_BIOME_%%g_%%h_NUM=4
						FOR /L %%k in (!PLAN_BIOME_%%g_%%h_NUM!,-1,1) DO (
							SET /A PLAN_BIOME_%%g_%%h_%%k=!RANDOM! * 6 / 32768 + 0
							IF !PLAN_BIOME_%%g_%%h_%%k!==0 SET PLAN_BIOME_%%g_%%h_%%k=Aerial
							IF !PLAN_BIOME_%%g_%%h_%%k!==1 SET PLAN_BIOME_%%g_%%h_%%k=Aquatic
							IF !PLAN_BIOME_%%g_%%h_%%k!==2 SET PLAN_BIOME_%%g_%%h_%%k=Desert
							IF !PLAN_BIOME_%%g_%%h_%%k!==3 SET PLAN_BIOME_%%g_%%h_%%k=Forest
							IF !PLAN_BIOME_%%g_%%h_%%k!==4 SET PLAN_BIOME_%%g_%%h_%%k=Marsh
							IF !PLAN_BIOME_%%g_%%h_%%k!==5 SET PLAN_BIOME_%%g_%%h_%%k=Mountainous
						)
					) Else (
						SET PLAN_BIOME_%%g_%%h=Barren
					)
					IF !calc_v1! GEQ 3 (SET PLAN_GRAV_%%g_%%h=High) Else (SET PLAN_GRAV_%%g_%%h=Normal)
					) ELSE (
						SET PLAN_ATMO1_%%g_%%h=None
						SET PLAN_BIOME_%%g_%%h=Barren
						SET PLAN_GRAV_%%g_%%h=Low
						IF !calc_v2!==0 (
							SET PLAN_TYPE_%%g_%%h=Asteroid Field
							)
						)
				) Else (
				call :Planet_Size_Calc !RANDOM!*500/32768+0 %%g %%h
				IF !calc_v1! LEQ 10 SET PLAN_TYPE_%%g_%%h=Gas Dwarf
				IF !calc_v1! GEQ 301 SET PLAN_TYPE_%%g_%%h=Cold Gas Giant
			)
	)
)

GOTO Summary

REM System Generation Summary
REM ===========================================================================
REM This section takes all of the generated values and puts them all together
REM into a "readable" summary. In addition to collating the results of the 
REM script, a small section of code at the beginning attempts to help it 
REM generate more interesting star systems by restarting if a system with 2 or
REM fewer stars (not including Void Generation) stars and no planets. The one
REM exception to this are stars with a size of 0, since those are usually
REM interesting in their own right and have a low chance of generating planets.
:Summary
FOR /L %%i in (1,1,%STAR_NUM%) DO (
	IF %STAR_NUM% LEQ 2 (
		IF !PLAN_NUM_%%i!==0 (
			IF NOT !STAR_SIZE_%%i!==0 (
				GOTO System_Gen
			)
		)
	)
	ECHO =========================================================
	ECHO Star Type           ^| !STAR_%%i!
	ECHO =========================================================
	IF !PLAN_NUM_%%i!==0 (
		ECHO No Planets
		ECHO.
		)
	FOR /L %%j in (1,1,!PLAN_NUM_%%i!) DO (
			 IF NOT %%j==1 ECHO ---------------------------------------------------------
			 IF NOT !PLAN_TYPE_%%i_%%j!==Terrestrial (
				ECHO Planet %%j Type       ^| !PLAN_TYPE_%%i_%%j!
				ECHO ---------------------------------------------------------
				)
			 IF !PLAN_TYPE_%%i_%%j!==Terrestrial (
				ECHO Planet %%j Type       ^| !PLAN_TYPE_%%i_%%j!
				ECHO Planet %%j Mass       ^| !PLAN_SIZE_%%i_%%j!
				IF NOT "!PLAN_ATMO1_%%i_%%j!"=="None" (
					ECHO Planet %%j Atmosphere ^| !PLAN_ATMO2_%%i_%%j! !PLAN_ATMO1_%%i_%%j!
					FOR /L %%l in (1,1,!PLAN_BIOME_%%i_%%j_NUM!) DO (ECHO Planet %%j Biome %%l    ^| !PLAN_BIOME_%%i_%%j_%%l!)
					) Else (
					ECHO Planet %%j Atmosphere ^| !PLAN_ATMO1_%%i_%%j!
					ECHO Planet %%j Biome      ^| !PLAN_BIOME_%%i_%%j!
					)
				ECHO Planet %%j Gravity    ^| !PLAN_GRAV_%%i_%%j!
				ECHO ---------------------------------------------------------
				)
			)
		ECHO.
		)
GOTO EOF


REM Planet Size Decimal Calculation Subroutine
REM ===========================================================================
REM This section handles the floating point math for the script.
REM Credit is due to Chef Pharaoh of StackOverflow who I adapted the code from.
:Planet_Size_Calc
set scale_=1
set calc_v=
for /l %%m in (1,1,1) do set /a scale_*=10
set /a "calc_v=!scale_!*%1"
set /a calc_v1=!calc_v!/!scale_!
set /a calc_v2=!calc_v!-!calc_v1!*!scale_!
set PLAN_SIZE_%2_%3=!calc_v1!.!calc_v2!
exit /b



:EOF
ENDLOCAL
pause