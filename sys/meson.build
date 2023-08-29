sources = [
  'time.cpp'
]

headers = [
    'time.h',
    'times.h'
]

install_headers(headers, subdir : join_paths(get_option('prefix'), 'include/gstreamer-1.0/nvidia'))

sys_time_lib = static_library('sys_time',
  sources,
  include_directories: ['.'],
#   dependencies: [],
  install: true,
  install_dir: plugins_install_dir,
)


pkgconfig.generate(sys_time_lib, install_dir : plugins_pkgconfig_install_dir)
plugins += [sys_time_lib]