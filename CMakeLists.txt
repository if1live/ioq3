CMAKE_MINIMUM_REQUIRED( VERSION 2.6 )

PROJECT( ioq3 )

IF(${CMAKE_HOST_SYSTEM_PROCESSOR} STREQUAL "AMD64")
	SET( COMPILE_ARCH "x86_64" )
ELSE(  )
	SET( COMPILE_ARCH "x86" )
ENDIF(  )

SET( ARCH ${COMPILE_ARCH} )
SET( VERSION 1.36 )

SET( USE_INTERNAL_SPEEX 0 )
SET( BUILD_RENDERER_OPENGL2 0 )
SET( USE_INTERNAL_JPEG 1 )
SET( USE_INTERNAL_ZLIB 1 )
SET( USE_INTERNAL_OGG 0 )

ADD_DEFINITIONS( -DMISSIONPACK=missionpack )
ADD_DEFINITIONS( -DBOTLIB )
ADD_DEFINITIONS( -DUSE_LOCAL_HEADERS )


SET( IOQ3_CODE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/code )

FILE( GLOB IOQ3_BOTLIB_FILES "${IOQ3_CODE_DIR}/botlib/*.*" )
FILE( GLOB IOQ3_CGAME_FILES "${IOQ3_CODE_DIR}/cgame/*.*" )
FILE( GLOB IOQ3_CLIENT_FILES "${IOQ3_CODE_DIR}/client/*.*" )

FILE( GLOB IOQ3_GAME_FILES "${IOQ3_CODE_DIR}/game/*.*" )
LIST( REMOVE_ITEM IOQ3_GAME_FILES 
	"${IOQ3_CODE_DIR}/game/g_rankings.c" )

FILE( GLOB IOQ3_UI_FILES "${IOQ3_CODE_DIR}/ui/*.*" )
FILE( GLOB IOQ3_ZLIB_FILES "${IOQ3_CODE_DIR}/zlib/*.*" )

FILE( GLOB IOQ3_SERVER_FILES 
	"${IOQ3_CODE_DIR}/server/sv_bot.*"
	"${IOQ3_CODE_DIR}/server/sv_client.*"
	"${IOQ3_CODE_DIR}/server/sv_ccmds.*"
	"${IOQ3_CODE_DIR}/server/sv_game.*"
	"${IOQ3_CODE_DIR}/server/sv_init.*"
	"${IOQ3_CODE_DIR}/server/sv_main.*"
	"${IOQ3_CODE_DIR}/server/sv_net_chan.*"
	"${IOQ3_CODE_DIR}/server/sv_snapshot.*"
	"${IOQ3_CODE_DIR}/server/sv_world.*" )

FILE( GLOB IOQ3_SDL_FILES "${IOQ3_CODE_DIR}/sdl/*.*" )
FILE( GLOB IOQ3_RENDERERGL2_FILES "${IOQ3_CODE_DIR}/renderergl2/*.*" )
FILE( GLOB IOQ3_RENDERERGL1_FILES "${IOQ3_CODE_DIR}/renderergl1/*.*" )
FILE( GLOB IOQ3_RENDERERCOMMON_FILES "${IOQ3_CODE_DIR}/renderercommon/*.*" )

FILE( GLOB IOQ3_Q3UI_FILES "${IOQ3_CODE_DIR}/q3_ui/*.*" )
LIST( REMOVE_ITEM IOQ3_Q3UI_FILES 
	"${IOQ3_CODE_DIR}/q3_ui/ui_rankings.c"
	"${IOQ3_CODE_DIR}/q3_ui/ui_rankstatus.c"
	"${IOQ3_CODE_DIR}/q3_ui/ui_signup.c" )
	
FILE( GLOB_RECURSE IOQ3_OGG_FILES "${IOQ3_CODE_DIR}/libogg-1.3.0/*.*" )
FILE( GLOB IOQ3_JPEG_FILES "${IOQ3_CODE_DIR}/jpeg-8c/*.*" )	

FILE( GLOB IOQ3_SPEEX_FILES 
	"${IOQ3_CODE_DIR}/libspeex/*.c"
	"${IOQ3_CODE_DIR}/libspeex/*.h"	)
LIST( REMOVE_ITEM IOQ3_SPEEX_FILES
	"${IOQ3_CODE_DIR}/libspeex/testdenoise.c"
	"${IOQ3_CODE_DIR}/libspeex/testecho.c"
	"${IOQ3_CODE_DIR}/libspeex/testenc.c"
	"${IOQ3_CODE_DIR}/libspeex/testenc_uwb.c"
	"${IOQ3_CODE_DIR}/libspeex/testenc_wb.c" )
	
FILE( GLOB IOQ3_QCOMMON_FILES 
	"${IOQ3_CODE_DIR}/qcommon/cm_*.*"
	"${IOQ3_CODE_DIR}/qcommon/cmd.*"
	"${IOQ3_CODE_DIR}/qcommon/common.*"
	"${IOQ3_CODE_DIR}/qcommon/cvar.*"
	"${IOQ3_CODE_DIR}/qcommon/files.*"
	"${IOQ3_CODE_DIR}/qcommon/huffman.*"
	"${IOQ3_CODE_DIR}/qcommon/ioapi.*"
	"${IOQ3_CODE_DIR}/qcommon/md4.*"
	"${IOQ3_CODE_DIR}/qcommon/md5.*"
	"${IOQ3_CODE_DIR}/qcommon/msg.*"
	"${IOQ3_CODE_DIR}/qcommon/net_*.*"
	"${IOQ3_CODE_DIR}/qcommon/puff.*"
	"${IOQ3_CODE_DIR}/qcommon/q_*.*"
	"${IOQ3_CODE_DIR}/qcommon/qcommon.*"
	"${IOQ3_CODE_DIR}/qcommon/qfiles.*"
	"${IOQ3_CODE_DIR}/qcommon/surfaceflags.*"
	"${IOQ3_CODE_DIR}/qcommon/unzip.*" 
	
	"${IOQ3_CODE_DIR}/qcommon/vm.c"
	"${IOQ3_CODE_DIR}/qcommon/vm_interpreted.c"
	"${IOQ3_CODE_DIR}/qcommon/vm_local.h" )
	
IF( MSVC )
	LIST( APPEND IOQ3_QCOMMON_FILES "${IOQ3_CODE_DIR}/qcommon/vm_x86.c" )
ENDIF(  )
	
FILE( GLOB IOQ3_SYS_FILES 
	"${IOQ3_CODE_DIR}/sys/con_log.*"	
	"${IOQ3_CODE_DIR}/sys/con_passive.*"
	"${IOQ3_CODE_DIR}/sys/sys_local.*"
	"${IOQ3_CODE_DIR}/sys/sys_loadlib.*"
	"${IOQ3_CODE_DIR}/sys/sys_main.*")
IF( MSVC )
	LIST( APPEND IOQ3_SYS_FILES "${IOQ3_CODE_DIR}/sys/sys_win32.*" )
ENDIF(  )
	
FILE( GLOB IOQ3_ASM_FILES "${IOQ3_CODE_DIR}/asm/*.*" )

INCLUDE_DIRECTORIES( ${IOQ3_CODE_DIR}/libcurl )
INCLUDE_DIRECTORIES( ${IOQ3_CODE_DIR}/AL )
INCLUDE_DIRECTORIES( ${IOQ3_CODE_DIR}/SDL12/include )
INCLUDE_DIRECTORIES( ${IOQ3_CODE_DIR}/libspeex/include )
INCLUDE_DIRECTORIES( ${IOQ3_CODE_DIR}/jpeg-8c )
INCLUDE_DIRECTORIES( ${IOQ3_CODE_DIR}/libogg-1.3.0/include )

IF( MSVC )
	LINK_DIRECTORIES( ${IOQ3_CODE_DIR}/libs/win32-msvc/ )
ENDIF(  )

ADD_LIBRARY( cgame ${IOQ3_CGAME_FILES} )

ADD_LIBRARY( game ${IOQ3_GAME_FILES} )

ADD_LIBRARY( q3_ui ${IOQ3_Q3UI_FILES} )
	
ADD_LIBRARY( ui ${IOQ3_UI_FILES} )

SET( QUAKE_FILE_LIST 
	${IOQ3_BOTLIB_FILES}
	${IOQ3_RENDERERCOMMON_FILES}
	${IOQ3_SYS_FILES} 
	${IOQ3_CLIENT_FILES}
	${IOQ3_SERVER_FILES} 
	${IOQ3_ASM_FILES} 
	${IOQ3_SDL_FILES}
	${IOQ3_QCOMMON_FILES} )
	
IF( ${USE_INTERNAL_SPEEX} )
	ADD_DEFINITIONS( -DFLOATING_POINT )
	LIST( APPEND QUAKE_FILE_LIST ${IOQ3_SPEEX_FILES} )
ENDIF(  )

IF( ${BUILD_RENDERER_OPENGL2} )
	LIST( APPEND QUAKE_FILE_LIST ${IOQ3_RENDERERGL2_FILES} )
ELSE(  )
	LIST( APPEND QUAKE_FILE_LIST ${IOQ3_RENDERERGL1_FILES} )
ENDIF(  )

IF( ${USE_INTERNAL_JPEG} )
	LIST( APPEND QUAKE_FILE_LIST ${IOQ3_JPEG_FILES} )
ENDIF(  )

IF( ${USE_INTERNAL_ZLIB} )
	LIST( APPEND QUAKE_FILE_LIST ${IOQ3_ZLIB_FILES} )
ENDIF(  )

IF( ${USE_INTERNAL_OGG} )
	LIST( APPEND QUAKE_FILE_LIST ${IOQ3_OGG_FILES} )
ENDIF(  )

ADD_EXECUTABLE( quake ${QUAKE_FILE_LIST} )

	
SOURCE_GROUP( botlib FILES ${IOQ3_BOTLIB_FILES} )
SOURCE_GROUP( client FILES ${IOQ3_CLIENT_FILES} )
SOURCE_GROUP( server FILES ${IOQ3_SERVER_FILES} )
SOURCE_GROUP( sys FILES ${IOQ3_SYS_FILES} )
SOURCE_GROUP( asm FILES ${IOQ3_ASM_FILES} )
SOURCE_GROUP( qcommon FILES ${IOQ3_QCOMMON_FILES} )
SOURCE_GROUP( renderer FILES ${IOQ3_RENDERERCOMMON_FILES} )
	
IF( ${USE_INTERNAL_SPEEX} )
	ADD_DEFINITIONS( -DUSE_INTERNAL_SPEEX )
	ADD_DEFINITIONS( -DHAVE_CONFIG_H )	
	SOURCE_GROUP( speex FILES ${IOQ3_SPEEX_FILES} )
ENDIF(  )

IF( ${BUILD_RENDERER_OPENGL2} )
	SOURCE_GROUP( renderer FILES ${IOQ3_RENDERERGL2_FILES} )
ELSE(  )
	SOURCE_GROUP( renderer FILES ${IOQ3_RENDERERGL1_FILES} )
ENDIF(  )

IF( ${USE_INTERNAL_JPEG} )
	ADD_DEFINITIONS( -DUSE_INTERNAL_JPEG )
	SOURCE_GROUP( jpeg FILES ${IOQ3_JPEG_FILES} )
ENDIF(  )

IF( ${USE_INTERNAL_ZLIB} )
	ADD_DEFINITIONS( -DUSE_INTERNAL_ZLIB )
	SOURCE_GROUP( zlib FILES ${IOQ3_ZLIB_FILES} )
ENDIF(  )

IF( ${USE_INTERNAL_OGG} )
	ADD_DEFINITIONS( -DUSE_INTERNAL_OGG )
	SOURCE_GROUP( ogg FILES ${IOQ3_OGG_FILES} )
ENDIF(  )


TARGET_LINK_LIBRARIES( quake
	SDL
	SDLmain
	cgame
	game
	q3_ui
	ui
	opengl32
	wsock32
	Ws2_32
	Winmm
	psapi )