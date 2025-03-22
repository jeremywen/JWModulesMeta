#cmake -B build -G Ninja -DCMAKE_C_COMPILER="/Applications/ArmGNUToolchain/12.3.rel1/arm-none-eabi/bin/arm-none-eabi-gcc"
#cmake --build build

if(NOT "${METAMODULE_SDK_DIR}" STREQUAL "")
	message("METAMODULE_SDK_DIR set by CMake variable ${METAMODULE_SDK_DIR}")
elseif (DEFINED ENV{METAMODULE_SDK_DIR})
    set(METAMODULE_SDK_DIR "$ENV{METAMODULE_SDK_DIR}")
	message("METAMODULE_SDK_DIR set by environment variable ${METAMODULE_SDK_DIR}")
else()
    set(METAMODULE_SDK_DIR "${CMAKE_CURRENT_LIST_DIR}/../metamodule-plugin-sdk")
	message("METAMODULE_SDK_DIR set to default: ${METAMODULE_SDK_DIR}")
endif()

include(${METAMODULE_SDK_DIR}/plugin.cmake)

cmake_minimum_required(VERSION 3.19)
# include(../metamodule-plugin-sdk/plugin.cmake)

project(JWModules VERSION 0.1 DESCRIPTION "JWModules for MetaModule" LANGUAGES C CXX ASM)

add_library(JWModules STATIC)

# Add sources from the Rack source repo
set(SOURCE_DIR ../Rack/plugins/JW-Modules)

target_sources(JWModules PRIVATE
    ${SOURCE_DIR}/src/JWModules.cpp                    # Note: this file contains init(rack::Plugin*)
    ${SOURCE_DIR}/src/AbcdSeq.cpp 
    ${SOURCE_DIR}/src/Add5.cpp 
    ${SOURCE_DIR}/src/BlankPanel.cpp 
    ${SOURCE_DIR}/src/BouncyBalls.cpp 
    ${SOURCE_DIR}/src/Cat.cpp 
    ${SOURCE_DIR}/src/D1v1de.cpp 
    ${SOURCE_DIR}/src/DivSeq.cpp 
    ${SOURCE_DIR}/src/EightSeq.cpp 
    ${SOURCE_DIR}/src/FullScope.cpp 
    ${SOURCE_DIR}/src/GridSeq.cpp 
    ${SOURCE_DIR}/src/JWResizableHandle.hpp 
    ${SOURCE_DIR}/src/MinMax.cpp 
    ${SOURCE_DIR}/src/NoteSeq.cpp 
    ${SOURCE_DIR}/src/NoteSeq16.cpp 
    ${SOURCE_DIR}/src/NoteSeqFu.cpp 
    ${SOURCE_DIR}/src/OnePattern.cpp 
    ${SOURCE_DIR}/src/Patterns.cpp 
    ${SOURCE_DIR}/src/Pete.cpp 
    ${SOURCE_DIR}/src/Pres1t.cpp 
    ${SOURCE_DIR}/src/Quantizer.cpp 
    ${SOURCE_DIR}/src/QuantizeUtils.cpp 
    ${SOURCE_DIR}/src/SimpleClock.cpp 
    ${SOURCE_DIR}/src/Str1ker.cpp 
    ${SOURCE_DIR}/src/ThingThing.cpp 
    ${SOURCE_DIR}/src/Timer.cpp 
    ${SOURCE_DIR}/src/Tree.cpp 
    ${SOURCE_DIR}/src/Trigs.cpp 
    ${SOURCE_DIR}/src/WavHead.cpp 
    ${SOURCE_DIR}/src/XYPad.cpp
    # ${SOURCE_DIR}/src/MyModule2.cpp
)

# Add includes and compile options for source repo
target_include_directories(JWModules PRIVATE ${SOURCE_DIR}/src)

# Call this to link and create the plugin file
create_plugin(
    SOURCE_LIB      JWModules                                          # The cmake target name (defined in add_target)
    PLUGIN_NAME     JWModules                                          # This must match the brand "slug" used in VCV Rack
    PLUGIN_JSON     /Users/jeremy/vcv/Rack/plugins/JW-Modules/plugin.json                         # Path to the plugin.json file used by VCV Rack
    SOURCE_ASSETS   ${CMAKE_CURRENT_LIST_DIR}/assets                  # Path to the assets/ dir containing the PNGs
    DESTINATION     ${CMAKE_CURRENT_LIST_DIR}/../metamodule-plugins   # Path to where you want the plugin file output
)