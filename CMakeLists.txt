set(CMAKE_LEGACY_CYGWIN_WIN32 OFF)
cmake_minimum_required(VERSION 3.15)
project(BreakoutMona_sln C CXX)

option(MONA_BUILD_EXAMPLES "" OFF)
option(MONA_INSTALL "" OFF)
add_subdirectory(MonaEngine)
add_subdirectory(breakout_lib)

set(breakout_lib_INCLUDES breakout_lib/public)

add_executable(BreakoutMona "breakout.cpp") 
set_property(TARGET BreakoutMona  PROPERTY CXX_STANDARD 20)
target_link_libraries(BreakoutMona  PRIVATE
    MonaEngine
    breakout_lib
)
target_include_directories(BreakoutMona  PRIVATE
    ${MONA_INCLUDE_DIRECTORY}
    ${THIRD_PARTY_INCLUDE_DIRECTORIES}
    ${breakout_lib_INCLUDES}
)
add_custom_command(TARGET BreakoutMona POST_BUILD        
		COMMAND ${CMAKE_COMMAND} -E copy_if_different 
        $<TARGET_FILE:OpenAL> $<TARGET_FILE_DIR:BreakoutMona>)