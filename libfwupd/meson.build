cargs = [
  '-DG_LOG_DOMAIN="Fwupd"',
]

fwupd_version_h = configure_file(
  input : 'fwupd-version.h.in',
  output : 'fwupd-version.h',
  configuration : conf
)

install_headers(
  'fwupd.h',
  subdir : 'fwupd-1',
)

install_headers([
    'fwupd-client.h',
    'fwupd-enums.h',
    'fwupd-error.h',
    'fwupd-result.h',
    fwupd_version_h,
  ],
  subdir : 'fwupd-1/libfwupd',
)

deps = [
  giounix,
]

mapfile = 'fwupd.map'
vflag = '-Wl,--version-script,@0@/@1@'.format(meson.current_source_dir(), mapfile)
fwupd = shared_library(
  'fwupd',
  sources : [
    'fwupd-client.c',
    'fwupd-enums.c',
    'fwupd-error.c',
    'fwupd-result.c',
  ],
  soversion : lt_current,
  version : lt_version,
  dependencies : deps,
  c_args : cargs,
  include_directories : include_directories('..'),
  link_args : vflag,
  link_depends : mapfile,
  install : true
)

pkgg = import('pkgconfig')
pkgg.generate(
  libraries : fwupd,
  requires : [ 'gio-2.0' ],
  subdirs : 'fwupd-1',
  version : meson.project_version(),
  name : 'fwupd',
  filebase : 'fwupd',
  description : 'fwupd is a system daemon for installing device firmware',
)

gnome.generate_gir(fwupd,
  sources : [
    'fwupd-client.c',
    'fwupd-enums.c',
    'fwupd-enums.h',
    'fwupd-error.c',
    'fwupd-result.c',
  ],
  nsversion : '1.0',
  namespace : 'Fwupd',
  symbol_prefix : 'fwupd',
  identifier_prefix : 'Fwupd',
  export_packages : 'fwupd',
  dependencies : deps,
  includes : [
    'GObject-2.0',
  ],
  install : true
)

e = executable(
  'fwupd-self-test',
  sources : [
    'fwupd-self-test.c'
  ],
  include_directories : [
    include_directories('..'),
  ],
  dependencies : [
    gio,
  ],
  link_with : fwupd,
  c_args : cargs
)
test('fwupd-self-test', e)
