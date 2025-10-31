cmake_minimum_required(VERSION 3.10)  # keep it modest; avoids very new features
project(RooBarb LANGUAGES CXX)

# Options
option(BUILD_SHARED_LIBS "Build shared libs instead of static" OFF)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Add subdirs (order doesn’t matter with this pattern)
add_subdirectory(XmlConfig)
add_subdirectory(TaskEngine)
add_subdirectory(RooPlotLib)
add_subdirectory(RootAna)

# ------------------------------------------------------------------------------
# OPTION A: "Aggregator" target that pulls in everything transitively
# (link to this if you want all functionality without a single-file archive)
add_library(IRooBarb INTERFACE)
target_link_libraries(IRooBarb
    INTERFACE
        RootAna
        TaskEngine
        RooPlotLib
        XmlConfig
)
# return()
# ------------------------------------------------------------------------------
# OPTION B: TRUE single combined library (static or shared) using OBJECT libs
# This actually merges all objects into one archive/DSO.
if(BUILD_SHARED_LIBS)
    add_library(RooBarb SHARED  # STATIC if OFF, SHARED if ON
        $<TARGET_OBJECTS:XmlConfig_obj>
        $<TARGET_OBJECTS:TaskEngine_obj>
        $<TARGET_OBJECTS:RooPlotLib_obj>
        $<TARGET_OBJECTS:RootAna_obj>
    )
else()
    add_library(RooBarb STATIC  # STATIC if ON, SHARED if OFF
        $<TARGET_OBJECTS:XmlConfig_obj>
        $<TARGET_OBJECTS:TaskEngine_obj>
        $<TARGET_OBJECTS:RooPlotLib_obj>
        $<TARGET_OBJECTS:RootAna_obj>
    )
endif()

# Inherit include dirs & link deps from the component libs
target_link_libraries(RooBarb
    PUBLIC
        XmlConfig
        TaskEngine
        RooPlotLib
        RootAna
)

# Propagate public include directories (so consumers of RooBarb see headers)
target_include_directories(RooBarb
    PUBLIC
        $<TARGET_PROPERTY:XmlConfig,INTERFACE_INCLUDE_DIRECTORIES>
        $<TARGET_PROPERTY:TaskEngine,INTERFACE_INCLUDE_DIRECTORIES>
        $<TARGET_PROPERTY:RooPlotLib,INTERFACE_INCLUDE_DIRECTORIES>
        $<TARGET_PROPERTY:RootAna,INTERFACE_INCLUDE_DIRECTORIES>
)