cmake_minimum_required(VERSION 3.10...4.0)

project(mbew
    VERSION 1.0
    LANGUAGES C CXX
)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)

set(CMAKE_POLICY_VERSION_MINIMUM 3.5)

if(WIN32)
    add_definitions(-D_CRT_SECURE_NO_WARNINGS)
endif()


add_subdirectory("ext/mbew-vpx")
add_subdirectory("ext/mbew-nestegg")
add_subdirectory("ext/mbew-ogg")
add_subdirectory("ext/mbew-vorbis")
add_subdirectory("ext/mbew-opus")


add_library(mbew SHARED
    src/mbew.h
    src/mbew.hpp
    src/mbew-private.h
    src/mbew.c
    src/mbew-format.c
    src/mbew-io.c
    src/mbew-iterate.c
    src/mbew-string.c
)


target_compile_definitions(mbew
    PRIVATE
        MBEW_BUILD
)


target_include_directories(mbew
    PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/src
)


target_link_libraries(mbew
    PUBLIC
        mbew-vpx
        mbew-nestegg
        mbew-vorbis
        mbew-ogg
        mbew-opus
)



if(MSVC)
    set_target_properties(mbew PROPERTIES
        WINDOWS_EXPORT_ALL_SYMBOLS ON
    )
endif()


include(GNUInstallDirs)

install(
    TARGETS mbew
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

install(
    FILES src/mbew.h
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)
