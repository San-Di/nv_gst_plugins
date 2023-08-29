sources = [
  'MuxConfigParser.cpp',
  'nvstreammux_batch.cpp',
  'nvstreammux_pads.cpp',
  'nvstreammux.cpp'
]

headers = [
    'MuxConfigParser.h',
    'nvstreammux_batch.h',
    'nvstreammux_debug.h',
    'nvstreammux_pads.h',
    'nvstreammux.h',
]

install_headers(headers, subdir : join_paths(get_option('prefix'), 'include/gstreamer-1.0'))

custom_nvlibraries = static_library('nvlibs',
  sources,
  include_directories: [general_includes, yaml_includes, '.'],
  dependencies: [gst_dep, yaml_dep, gstbase_dep, gstaudio_dep, gstfft_dep, gstriff_dep, gstrtp_dep, gstrtsp_dep, gstsdp_dep, gsttag_dep, gstvideo_dep, gobject_dep, glib_dep],
  install: true,
  install_dir: plugins_install_dir,
)


pkgconfig.generate(custom_nvlibraries, install_dir : plugins_pkgconfig_install_dir)
plugins += [custom_nvlibraries]