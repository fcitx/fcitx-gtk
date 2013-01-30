if(NOT ENABLE_GTK2_IM_MODULE)
  return()
endif()

PKG_CHECK_MODULES(GTK2 REQUIRED "gtk+-2.0" )
_pkgconfig_invoke("gtk+-2.0" GTK2 BINARY_VERSION
  "" "--variable=gtk_binary_version")
_pkgconfig_invoke("gtk+-2.0" GTK2 LIBDIR "" "--variable=libdir")
set(GTK2_IM_MODULEDIR
  "${GTK2_LIBDIR}/gtk-2.0/${GTK2_BINARY_VERSION}/immodules")

include_directories(${GTK2_INCLUDE_DIRS})
link_directories(${GTK2_LIBRARY_DIRS})

set(FCITX_GTK2_IM_MODULE_SOURCES
    fcitxim.c
    fcitximcontext-gio.c)

add_library(im-fcitx MODULE ${FCITX_GTK2_IM_MODULE_SOURCES})
set_target_properties( im-fcitx PROPERTIES PREFIX ""
  COMPILE_FLAGS "-fvisibility=hidden"
  LINK_FLAGS "-Wl,--no-undefined")

target_link_libraries(im-fcitx
    ${GTK2_LIBRARIES}
    ${FCITX4_FCITX_UTILS_LIBRARIES}
    ${FCITX_GCLIENT_LIBRARIES})
install(TARGETS im-fcitx DESTINATION "${GTK2_IM_MODULEDIR}")
