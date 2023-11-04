cmake_minimum_required(VERSION 3.27)

project(dwm
        VERSION 6.4
        LANGUAGES C
        )

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED True)

add_executable(dwm drw.c dwm.c util.c)

# X11
target_include_directories(dwm PRIVATE /usr/X11R6/include)
target_link_directories(dwm PRIVATE /usr/X11R6/lib)
target_link_libraries(dwm PRIVATE X11)

# Xinerama, comment if you don't want it
# target_link_libraries(dwm PRIVATE Xinerama)
# target_compile_definitions(dwm PRIVATE XINERAMA)

# freetype
target_link_libraries(dwm PRIVATE fontconfig Xft)
target_include_directories(dwm PRIVATE /usr/include/freetype2)

target_compile_definitions(dwm PRIVATE
                           VERSION="${PROJECT_VERSION}"
                           _DEFAULT_SOURCE _BSD_SOURCE _XOPEN_SOURCE=700
                           )
target_compile_options(dwm PRIVATE
                       -pedantic
                       -Wall -Wno-deprecated-declarations
                       -Os
                       )

install(TARGETS dwm DESTINATION bin)
install(PROGRAMS sh/startdwm DESTINATION bin)
install(FILES dwm.1 DESTINATION share/man/man1)
install(PROGRAMS
        sh/dwm-screenshot sh/dwm-screenshot-box
        sh/dwm-clock
        DESTINATION share/dwm
        )
