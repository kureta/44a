cmake_minimum_required(VERSION 2.8.4)
cmake_policy(SET CMP0042 NEW)
project(ofApplication)
set(APP_NAME ofApplication)
set(BUNDLE_NAME "ofApplication")

# ===================== oF Directory =====================
set(OF_DIRECTORY "/YOUR/OF/DIRECTORY/HERE")
# ========================================================

# ===================== Compiler Flags =====================
set(CMAKE_C_COMPILER "/usr/bin/clang")
set(CMAKE_C_FLAGS "") # -x objective-c

set(CMAKE_CXX_COMPILER "/usr/bin/clang++")
set(CMAKE_CXX_FLAGS "-std=c++11 -stdlib=libc++ -D__MACOSX_CORE__") # Removed "-stdlib=libstdc++

set(CMAKE_OSX_ARCHITECTURES i386)
add_compile_options(-Wno-deprecated-declarations)
# ==========================================================

# ===================== Source Files =====================
set(SOURCE_FILES
        src/main.cpp
        src/ofApp.cpp)
# ========================================================

# =========== Set Global Properties ==============
if (EXISTS "addons.cmake")
    include(addons.cmake)
endif ()
include(openFrameworks.cmake)
# ================================================

# ===================== System Frameworks =====================

# =============================================================

# ===================== Include Dirs =====================

# ========================================================

# ===================== Libs =====================

# ================================================

# ============ Compile and Link ==================
add_executable(${APP_NAME} MACOSX_BUNDLE ${SOURCE_FILES})
add_dependencies(${APP_NAME} of_shared)
target_link_libraries(${APP_NAME} $<TARGET_FILE:of_shared> ${opengl_lib})
# ================================================


ADD_CUSTOM_COMMAND(TARGET ${APP_NAME}
        POST_BUILD
        COMMAND ${CMAKE_INSTALL_NAME_TOOL}
        ARGS -change "@rpath/libopenFrameworks.dylib" "@loader_path/../Frameworks/libopenFrameworks.dylib" $<TARGET_FILE:${APP_NAME}>
)

set_target_properties(${APP_NAME} PROPERTIES MACOSX_BUNDLE_INFO_STRING "Cmake openFrameworks CLion test app")
set_target_properties(${APP_NAME} PROPERTIES MACOSX_BUNDLE_LONG_VERSION_STRING "0.0.1 pre-release alpha")
set_target_properties(${APP_NAME} PROPERTIES MACOSX_BUNDLE_BUNDLE_NAME ${BUNDLE_NAME})
set_target_properties(${APP_NAME} PROPERTIES MACOSX_BUNDLE_SHORT_VERSION_STRING 0.0.1)
set_target_properties(${APP_NAME} PROPERTIES MACOSX_BUNDLE_BUNDLE_VERSION 0.0.1)
set_target_properties(${APP_NAME} PROPERTIES MACOSX_BUNDLE_COPYRIGHT "--YOUR NAME HERE--")
set_target_properties(${APP_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)
# ================================================
