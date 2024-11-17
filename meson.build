# Copyright (c) 2024-2025 fei_cong(https://github.com/feicong/ebpf-course)

project('argp', 'c', version: '1.0', license: 'GPL-3.0-or-later')

# Gnulib requires some specific compiler arguments, generally to suppress warnings
# and add GNU extensions if needed.
argp_extra_cflags = [
  '-Wall',
  '-Wextra',
  '-Wno-unused-parameter',
  '-D_GL_CONFIG_H_INCLUDED',
  '-DHAVE_CONFIG_H',
  '-D_GNU_SOURCE',
  '-include', 'config.h',  # Ensure config.h is included first as required
]

# Include the Gnulib header directory
gnulib_inc = include_directories('src')

# Source files from Gnulib's argp implementation
argp_sources = files(
  'src/argp-ba.c',
  'src/argp-eexst.c',
  'src/argp-fmtstream.c',
  'src/argp-fs-xinl.c',
  'src/argp-help.c',
  'src/argp-parse.c',
  'src/argp-pv.c',
  'src/argp-xinl.c',
  'src/getopt.c',
  'src/getopt1.c',
  'src/argp-pvh.c'
)

# Configuration checks similar to CMake checks
cc = meson.get_compiler('c')

# Check for headers
have_mempcpy_h = cc.has_header('mempcpy.h')
have_strcase_h = cc.has_header('strcase.h')
have_strchrnul_h = cc.has_header('strchrnul.h')
have_strndup_h = cc.has_header('strndup.h')
have_sysexits_h = cc.has_header('sysexits.h')
have_unistd_h = cc.has_header('unistd.h')

# Check for symbols and functions
have_asprintf = cc.has_function('asprintf')
have_mempcpy = cc.has_function('mempcpy')
have_random = cc.has_function('random')
have_sleep = cc.has_function('sleep')
have_strerror_r = cc.has_function('strerror_r')
have_putc_unlocked = cc.has_function('putc_unlocked')
have_fputs_unlocked = cc.has_function('fputs_unlocked')
have_fwrite_unlocked = cc.has_function('fwrite_unlocked')
have_strcasecmp = cc.has_function('strcasecmp')
have_strchrnul = cc.has_function('strchrnul')
have_strndup = cc.has_function('strndup')
have_program_invocation_short_name = cc.has_function('program_invocation_short_name')
have_program_invocation_name = cc.has_function('program_invocation_name')
have_ssize_t = cc.has_type('ssize_t', dependencies : cc.find_library('c', required : false))

# Generate config.h using the results
config_h = configuration_data()
config_h.set('HAVE_CONFIG_H', 1)
config_h.set('HAVE_MEMPCPY_H', have_mempcpy_h)
config_h.set('HAVE_STRCASE_H', have_strcase_h)
config_h.set('HAVE_STRCHRNUL_H', have_strchrnul_h)
config_h.set('HAVE_STRNDUP_H', have_strndup_h)
config_h.set('HAVE_SYSEXITS_H', have_sysexits_h)
config_h.set('HAVE_UNISTD_H', have_unistd_h)
config_h.set('HAVE_ASPRINTF', have_asprintf)
config_h.set('HAVE_MEMPCPY', have_mempcpy)
config_h.set('HAVE_RANDOM', have_random)
config_h.set('HAVE_SLEEP', have_sleep)
config_h.set('HAVE_STRERROR_R', have_strerror_r)
config_h.set('HAVE_DECL_PUTC_UNLOCKED', have_putc_unlocked)
config_h.set('HAVE_DECL_FPUTS_UNLOCKED', have_fputs_unlocked)
config_h.set('HAVE_DECL_FWRITE_UNLOCKED', have_fwrite_unlocked)
config_h.set('HAVE_STRCASECMP', have_strcasecmp)
config_h.set('HAVE_STRCHRNUL', have_strchrnul)
config_h.set('HAVE_STRNDUP', have_strndup)
config_h.set('HAVE_DECL_PROGRAM_INVOCATION_SHORT_NAME', have_program_invocation_short_name)
config_h.set('HAVE_DECL_PROGRAM_INVOCATION_NAME', have_program_invocation_name)
config_h.set('HAVE_SSIZE_T', have_ssize_t)

# Write out config.h
configure_file(
  input: 'config.h.in.meson',
  output: 'config.h',
  configuration: config_h
)

argp_lib = static_library(
  'argp',
  sources: argp_sources,
  c_args: argp_extra_cflags,
  include_directories: gnulib_inc,
  install: true,
)

argp_dep = declare_dependency(
  link_with: argp_lib,
  include_directories: gnulib_inc,
)

pkg = import('pkgconfig')
pkg.generate(
  libraries: argp_lib,
  name: 'argp',
  description: 'Gnulib argp library',
  version: '1.0',
  url: 'https://www.gnu.org/software/gnulib/',
  subdirs: ['gnulib'],
)

meson.override_dependency('argp', argp_dep)
