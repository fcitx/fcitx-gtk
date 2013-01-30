cmake_minimum_required(VERSION 2.6)
project(fcitx-gtk)

include(FindPkgConfig)

find_package(Fcitx 4.2.7 REQUIRED)

option(ENABLE_GTK2_IM_MODULE "Enable GTK2 IM Module" ON)
option(ENABLE_GTK3_IM_MODULE "Enable GTK3 IM Module" ON)
option(ENABLE_SNOOPER "Enable Key Snooper for gtk app" On)

pkg_check_modules(FCITX_GCLIENT "fcitx-gclient>=4.2.7" REQUIRED)

if(NOT NO_SNOOPER_APPS)
  set(NO_SNOOPER_APPS ".*chrome.*,.*chromium.*,firefox.*,Do.*"
    CACHE STRING "Disable Key Snooper for following app")
endif()

add_definitions("-DLOCALEDIR=\"${FCITX4_PREFIX}/share/locale\"" "-DNO_SNOOPER_APPS=\"${NO_SNOOPER_APPS}\"")
if (ENABLE_SNOOPER)
    add_definitions("-D_ENABLE_SNOOPER=1")
endif (ENABLE_SNOOPER)

include_directories (
  ${FCITX4_FCITX_INCLUDE_DIRS}
  ${FCITX4_FCITX_UTILS_INCLUDE_DIRS}
  ${FCITX4_FCITX_CONFIG_INCLUDE_DIRS}
  ${FCITX_GCLIENT_INCLUDE_DIRS}
  )

link_directories (
  ${FCITX4_FCITX_LIBRARY_DIRS}
  ${FCITX4_FCITX_UTILS_LIBRARY_DIRS}
  ${FCITX4_FCITX_CONFIG_LIBRARY_DIRS}
  ${FCITX_GCLIENT_LIBRARY_DIRS}
  )

add_subdirectory(gtk2)
add_subdirectory(gtk3)
