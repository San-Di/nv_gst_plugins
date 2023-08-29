project('custom-nvlibraries', 'c',
    version : '1.0.2',
    meson_version : '>= 0.54',
    default_options : [ 'warning_level=1',
                      'buildtype=debugoptimized' ]
    )

gst_version = meson.project_version()
version_arr = gst_version.split('.')
gst_version_major = version_arr[0].to_int()
gst_version_minor = version_arr[1].to_int()
gst_version_micro = version_arr[2].to_int()
 if version_arr.length() == 4
  gst_version_nano = version_arr[3].to_int()
else
  gst_version_nano = 0
endif
gst_version_is_dev = gst_version_minor % 2 == 1 and gst_version_micro < 90

have_cxx = add_languages('cpp', native: false, required: false)

glib_req = '>= 2.56.0'
orc_req = '>= 0.4.17'
gst_req = '>= @0@.@1@.0'.format(gst_version_major, gst_version_minor)

api_version = '1.0'

plugins_install_dir = join_paths(get_option('prefix'), 'lib/gstreamer-1.0')
plugins = []

cc = meson.get_compiler('c')
host_system = host_machine.system()

cdata = configuration_data()
cdata.set_quoted('PACKAGE_VERSION', gst_version)
cdata.set_quoted('PACKAGE', 'custom-nvlibraries')
cdata.set_quoted('GST_LICENSE', 'LGPL')
cdata.set_quoted('GST_API_VERSION', api_version)
cdata.set_quoted('GST_PACKAGE_NAME', 'Nvidia custom libraries')
# cdata.set_quoted('GST_PACKAGE_ORIGIN', 'https://github.com/rosemary-crypto/Custom-Gstreamer-Plugins.git')

# Mandatory GST deps
gst_dep = dependency('gstreamer-1.0', version : gst_req,
  fallback : ['gstreamer', 'gst_dep'])
gstbase_dep = dependency('gstreamer-base-1.0', version : gst_req,
  fallback : ['gstreamer', 'gst_base_dep'])
gstnet_dep = dependency('gstreamer-net-1.0', version : gst_req,
  fallback : ['gstreamer', 'gst_net_dep'])
gstcontroller_dep = dependency('gstreamer-controller-1.0', version : gst_req,
  fallback : ['gstreamer', 'gst_controller_dep'])
gstpbutils_dep = dependency('gstreamer-pbutils-1.0', version : gst_req,
    fallback : ['gst-plugins-base', 'pbutils_dep'])
gstallocators_dep = dependency('gstreamer-allocators-1.0', version : gst_req,
    fallback : ['gst-plugins-base', 'allocators_dep'])
gstapp_dep = dependency('gstreamer-app-1.0', version : gst_req,
    fallback : ['gst-plugins-base', 'app_dep'])
gstaudio_dep = dependency('gstreamer-audio-1.0', version : gst_req,
    fallback : ['gst-plugins-base', 'audio_dep'])
gstfft_dep = dependency('gstreamer-fft-1.0', version : gst_req,
    fallback : ['gst-plugins-base', 'fft_dep'])
gstriff_dep = dependency('gstreamer-riff-1.0', version : gst_req,
    fallback : ['gst-plugins-base', 'riff_dep'])
gstrtp_dep = dependency('gstreamer-rtp-1.0', version : gst_req,
    fallback : ['gst-plugins-base', 'rtp_dep'])
gstrtsp_dep = dependency('gstreamer-rtsp-1.0', version : gst_req,
    fallback : ['gst-plugins-base', 'rtsp_dep'])
gstsdp_dep = dependency('gstreamer-sdp-1.0', version : gst_req,
    fallback : ['gst-plugins-base', 'sdp_dep'])
gsttag_dep = dependency('gstreamer-tag-1.0', version : gst_req,
    fallback : ['gst-plugins-base', 'tag_dep'])
gstvideo_dep = dependency('gstreamer-video-1.0', version : gst_req,
    fallback : ['gst-plugins-base', 'video_dep'])
gobject_dep = dependency('gobject-2.0')
glib_dep = [dependency('glib-2.0', version : glib_req, fallback: ['glib', 'libglib_dep']),
             dependency('gobject-2.0', fallback: ['glib', 'libgobject_dep'])]
yaml_dep = dependency('yaml-cpp', required: true)

# yaml_include_dir = 'C:\gstreamer_plugins\lib\yaml-cpp\include'
# yaml_lib_dir = 'C:\gstreamer_plugins\lib\yaml-cpp\build\Debug'


# yaml_libs = ['yaml-cppd']
# yaml_deps = []

# foreach libname: yaml_libs
#   yaml_deps += cc.find_library(libname, dirs: yaml_lib_dir)
# endforeach

# yaml_dep = declare_dependency(
#   dependencies: yaml_deps,
#   include_directories: include_directories(yaml_include_dir)
# )

pkgconfig = import('pkgconfig')
plugins_pkgconfig_install_dir = join_paths(plugins_install_dir, 'pkgconfig')
if get_option('default_library') == 'shared'
  # If we don't build static plugins there is no need to generate pc files
  plugins_pkgconfig_install_dir = disabler()
endif

pkgconfig_variables = ['exec_prefix=${prefix}',
    'toolsdir=${exec_prefix}/bin',
    'pluginsdir=${libdir}/gstreamer-1.0',
    'datarootdir=${prefix}/share',
    'datadir=${datarootdir}',
    'girdir=${datadir}/gir-1.0',
    'typelibdir=${libdir}/girepository-1.0',
    'libexecdir=${prefix}/libexec']
pkgconfig_subdirs = ['gstreamer-1.0']

python3 = import('python').find_installation()

configinc = include_directories('.')
libsinc = include_directories('includes')
yamlinc = include_directories('C:/vcpkg/installed/x64-windows/include')


subdir('libs')
# subdir('includes')

run_command(python3, '-c', 'import shutil; shutil.copy("hooks/pre-commit.hook", ".git/hooks/pre-commit")')

if meson.version().version_compare('>= 0.54')
  plugin_names = []
  foreach plugin: plugins
    # FIXME: Use str.subtring() when we can depend on Meson 0.56
    split = plugin.name().split('gst')
    if split.length() == 2
      plugin_names += [split[1]]
    else
      warning('Need substring API in meson >= 0.56 to properly parse plugin name: ' + plugin.name())
      plugin_names += [plugin.name()]
    endif
  endforeach
  summary({'Plugins':plugin_names}, list_sep: ', ')
endif
