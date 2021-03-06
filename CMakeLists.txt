cmake_minimum_required(VERSION 2.8.11)

# Project name without spaces or special characters
project("freeshop")

set(APP_TITLE "freeShop")
set(APP_DESCRIPTION "Open source eShop clone")
set(APP_AUTHOR "Cruel")
set(APP_ICON ${PROJECT_SOURCE_DIR}/res/app/icon.png)
set(APP_VERSION 1)

# CIA settings
set(APP_UNIQUE_ID 0xF12EE) # This must be unique for your CIA project
set(APP_PRODUCT_CODE ${PROJECT_NAME})
set(BANNER_IMAGE ${PROJECT_SOURCE_DIR}/res/app/banner.png)
set(BANNER_SOUND ${PROJECT_SOURCE_DIR}/res/app/banner.wav)
set(CIA_CONFIG ${PROJECT_SOURCE_DIR}/res/app/cia.rsf)

# Include cpp3ds cmake module
set(CPP3DS $ENV{CPP3DS})
if(NOT CPP3DS)
	message(FATAL_ERROR "You need to set the env variable CPP3DS before compiling.")
endif()
set(CMAKE_MODULE_PATH "${CPP3DS}/cmake" ${CMAKE_MODULE_PATH})
include(cpp3ds)

include_directories(
	${PROJECT_SOURCE_DIR}/external/tween-engine/include
	${PROJECT_SOURCE_DIR}/external/rapidjson/include
)
FILE(GLOB_RECURSE TWEEN_ENGINE_SOURCE ${PROJECT_SOURCE_DIR}/external/tween-engine/src/*.cpp)

set(SOURCE_FILES
	${PROJECT_SOURCE_DIR}/src/main.cpp
	${PROJECT_SOURCE_DIR}/src/AppItem.cpp
	${PROJECT_SOURCE_DIR}/src/AppList.cpp
	${PROJECT_SOURCE_DIR}/src/AppInfo.cpp
	${PROJECT_SOURCE_DIR}/src/Config.cpp
	${PROJECT_SOURCE_DIR}/src/IconSet.cpp
	${PROJECT_SOURCE_DIR}/src/FreeShop.cpp
	${PROJECT_SOURCE_DIR}/src/Util.cpp
	${PROJECT_SOURCE_DIR}/src/RichText.cpp
	${PROJECT_SOURCE_DIR}/src/Download.cpp
	${PROJECT_SOURCE_DIR}/src/DownloadQueue.cpp
	${PROJECT_SOURCE_DIR}/src/AssetManager.hpp
	${PROJECT_SOURCE_DIR}/src/Notification.cpp
	${PROJECT_SOURCE_DIR}/src/Keyboard/Keyboard.cpp
	${PROJECT_SOURCE_DIR}/src/Keyboard/tinyxml2.cpp
	${PROJECT_SOURCE_DIR}/src/GUI/Button.cpp
	${PROJECT_SOURCE_DIR}/src/GUI/NinePatch.cpp
	${PROJECT_SOURCE_DIR}/src/States/State.cpp
	${PROJECT_SOURCE_DIR}/src/States/StateStack.cpp
	${PROJECT_SOURCE_DIR}/src/States/LoadingState.cpp
	${PROJECT_SOURCE_DIR}/src/States/TitleState.cpp
	${PROJECT_SOURCE_DIR}/src/States/BrowseState.cpp
	${PROJECT_SOURCE_DIR}/src/States/SyncState.cpp
	${TWEEN_ENGINE_SOURCE}
)

set(ARM_SOURCE_FILES
	${PROJECT_SOURCE_DIR}/src/Installer.cpp
)

set(EMU_SOURCE_FILES
	${PROJECT_SOURCE_DIR}/src/Installer_emu.cpp
)

# Use either nihstro or picasso for your shader scripts
set(SHADER_AS nihstro)
FILE(GLOB_RECURSE SHADER_FILES ${PROJECT_SOURCE_DIR}/res/romfs/shaders/[^.]*.vsh)

##################################################
# Don't usually need to change anything below
##################################################
set(CPP3DS_ARM_FLAGS "-O2 -march=armv6k -mtune=mpcore -mfloat-abi=hard -mtp=soft")
set(CPP3DS_EMU_FLAGS "-O0")

set(CPP3DS_ARM_LIBS cpp3ds-window cpp3ds-network cpp3ds-graphics cpp3ds-audio cpp3ds-system citro3d ctru freetype png z vorbisenc vorbisfile vorbis ogg archive)
set(CPP3DS_EMU_LIBS cpp3ds-emu sfml-graphics sfml-window sfml-system sfml-audio openal GLEW GL jpeg freetype vorbisenc vorbisfile vorbis ogg ssl crypto pthread archive)

add_subdirectory(${CPP3DS}/cmake/template_arm build/arm)
add_subdirectory(${CPP3DS}/cmake/template_emu build/emu)
