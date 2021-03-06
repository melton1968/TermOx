cmake_minimum_required(VERSION 3.2 FATAL_ERROR)
project(TermOx LANGUAGES CXX)

# Build Types
set(CMAKE_BUILD_TYPE ${CMAKE_BUILD_TYPE}
    CACHE STRING "Choose the type of build, options are: None Debug Release RelWithDebInfo MinSizeRel tsan asan lsan msan ubsan"
    FORCE
)

# ThreadSanitizer
set(CMAKE_CXX_FLAGS_TSAN
    "-fsanitize=thread -g -O1"
    CACHE STRING "Flags used by the C++ compiler during ThreadSanitizer builds."
    FORCE
)

# AddressSanitize
set(CMAKE_CXX_FLAGS_ASAN
    "-fsanitize=address -fno-optimize-sibling-calls -fsanitize-address-use-after-scope -fno-omit-frame-pointer -g -O1"
    CACHE STRING "Flags used by the C++ compiler during AddressSanitizer builds."
    FORCE
)

# LeakSanitizer
set(CMAKE_CXX_FLAGS_LSAN
    "-fsanitize=leak -fno-omit-frame-pointer -g -O1"
    CACHE STRING "Flags used by the C++ compiler during LeakSanitizer builds."
    FORCE
)

# MemorySanitizer
set(CMAKE_CXX_FLAGS_MSAN
    "-fsanitize=memory -fno-optimize-sibling-calls -fsanitize-memory-track-origins=2 -fno-omit-frame-pointer -g -O2"
    CACHE STRING "Flags used by the C++ compiler during MemorySanitizer builds."
    FORCE
)

# UndefinedBehaviour
set(CMAKE_CXX_FLAGS_UBSAN
    "-fsanitize=undefined"
    CACHE STRING "Flags used by the C++ compiler during UndefinedBehaviourSanitizer builds."
    FORCE
)

message("Build Type: ${CMAKE_BUILD_TYPE}" )

if(${CMAKE_VERSION} VERSION_LESS "3.8")
    set(CMAKE_CXX_STANDARD_REQUIRED ON)
endif()

# Add nCurses
add_subdirectory(external/ncurses-snapshots)

# Add Signals-Light
add_subdirectory(external/signals-light)

# Add TermOx
add_subdirectory(src)

# Add Demos
add_subdirectory(demos)

# Add Tests
add_subdirectory(tests)

set(LIB_PATH "${CMAKE_CURRENT_SOURCE_DIR}")
set(LIB_FILE "${LIB_PATH}/external/ncurses-snapshots/output/lib/libncursesw.a")
install(FILES ${LIB_FILE} TYPE LIB)
install(DIRECTORY external/signals-light/include/signals_light TYPE INCLUDE)
