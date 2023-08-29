sources = [
    'gstnvbufaudio.cpp',
    'gstnvmultistream.cpp',
    'gstnvstreamdemux.cpp',
    'gstnvstreammux.cpp',
    'GstNvStreamMuxCtx.cpp',
    'gstnvstreammux_audio.cpp',
    'gstnvstreammux_ntp.cpp',
    'gstnvstreammux_pads.cpp',
    'gstnvstreampad.cpp',
    'gstnvtimesynch.cpp',
]

headers = [
    'gstnvbufaudio.h',
    'gstnvstreamdemux.h',
    'gstnvstreammux.h',
    'GstNvStreamMuxCtx.h',
    'gstnvstreammuxdebug.h',
    'gstnvstreammux_audio.h',
    'gstnvstreammux_impl.h',
    'gstnvstreammux_ntp.h',
    'gstnvstreampad.h',
    'gstnvtimesynch.h',
    '../../sys/time.h',
]

install_headers(headers, subdir : join_paths(get_option('prefix'), 'include/gstreamer-1.0'))

gst_nvlibraries = static_library('gstnvlibs',
  sources,
  #include_directories: include_directories('.','../gst-metadata','../'), 
  include_directories: [general_includes, lib_includes, yaml_includes, cuda_includes, '.'],
  dependencies: [gst_dep, yaml_dep, gstbase_dep, gstnet_dep, gstcontroller_dep, gstpbutils_dep, gstallocators_dep, gstapp_dep, gstaudio_dep, gstfft_dep, gstriff_dep, gstrtp_dep, gstrtsp_dep, gstsdp_dep, gsttag_dep, gstvideo_dep, gobject_dep, glib_dep],
  install: true,
  install_dir: plugins_install_dir,
)

pkgconfig.generate(gst_nvlibraries, install_dir : plugins_pkgconfig_install_dir)
plugins += [gst_nvlibraries]
