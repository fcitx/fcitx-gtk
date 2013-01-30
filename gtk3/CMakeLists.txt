if(NOT ENABLE_GTK3_IM_MODULE)
  return()
endif()

PKG_CHECK_MODULES(GTK3 REQUIRED "gtk+-3.0" )
_pkgconfig_invoke("gtk+-3.0" GTK3 BINARY_VERSION
  "" "--variable=gtk_binary_version")
_pkgconfig_invoke("gtk+-3.0" GTK3 LIBDIR "" "--variable=libdir")
set(GTK3_IM_MODULEDIR
  "${GTK3_LIBDIR}/gtk-3.0/${GTK3_BINARY_VERSION}/immodules")

include_directories(${GTK3_INCLUDE_DIRS})
link_directories(${GTK3_LIBRARY_DIRS})

set(FCITX_GTK3_IM_MODULE_SOURCES
    fcitxim.c
    fcitximcontext-gio.c)

add_library(im-fcitx-gtk3 MODULE ${FCITX_GTK3_IM_MODULE_SOURCES})
set_target_properties( im-fcitx-gtk3 PROPERTIES PREFIX ""   OUTPUT_NAME "im-fcitx"
  COMPILE_FLAGS "-fvisibility=hidden"
  LINK_FLAGS "-Wl,--no-undefined")

target_link_libraries(im-fcitx-gtk3
    ${GTK3_LIBRARIES}
    ${FCITX4_FCITX_UTILS_LIBRARIES}
    ${FCITX_GCLIENT_LIBRARIES})
install(TARGETS im-fcitx-gtk3 DESTINATION "${GTK3_IM_MODULEDIR}")
