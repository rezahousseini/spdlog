project('spdlog', ['cpp'],
  license         : 'MIT',
  version         : run_command(find_program('scripts/extract_version.py')).stdout().strip(),
  default_options : [
    'warning_level=3',
    'cpp_std=c++11',
    'default_library=static',
    'buildtype=release',
    'b_colorout=always',
  ],
)

# ------------------------
# ---   Dependencies   ---
# ------------------------
dep_list = []
compile_args = []

# Threads
dep_list += dependency('threads')

# Check for FMT
if get_option('extrenal_fmt')
  if not meson.version().version_compare('>=0.49.0')
    warning('Finding fmt can fail wit meson versions before 0.49.0')
  endif
  dep_list     += dependency('fmt')
  compile_args += '-DSPDLOG_FMT_EXTERNAL'
endif

# ------------------------------------
# ---   Compiled library version   ---
# ------------------------------------

spdlog_inc = include_directories('./include')

spdlog = library('spdlog', ['src/spdlog.cpp'],
  cpp_args            : [compile_args] + ['-DSPDLOG_COMPILED_LIB'],
  include_directories : spdlog_inc,
  dependencies        : dep_list,
  install             : true,
)

spdlog_dep = declare_dependency(
  link_with           : spdlog,
  include_directories : spdlog_inc,
  compile_args        : compile_args + ['-DSPDLOG_COMPILED_LIB'],
  dependencies        : dep_list,
  version             : meson.project_version(),
)

# ----------------------------------
# ---   Header only dependency   ---
# ----------------------------------

spdlog_headeronly_dep = declare_dependency(
  include_directories : spdlog_inc,
  compile_args        : compile_args,
  dependencies        : dep_list,
  version             : meson.project_version(),
)

# ------------------------
# ---   Installation   ---
# ------------------------

install_subdir('include/spdlog', install_dir: get_option('includedir'))

pkg = import('pkgconfig')
pkg.generate(spdlog,
  name         : 'spdlog',
  description  : 'Fast C++ logging library',
  url          : 'https://github.com/gabime/spdlog',
  extra_cflags : ['-DSPDLOG_COMPILED_LIB']
)

# -------------------------------------
# ---   Conditionally add subdirs   ---
# -------------------------------------

if get_option('enable_tests')
  subdir('tests')
endif

if get_option('enable_examples')
  subdir('example')
endif

if get_option('enable_benchmarks')
  subdir('bench')
endif

# -------------------
# ---   Summary   ---
# -------------------

summary_str = '''spdlog build summary:

  - using extrenal fmt:  @0@
  - building tests:      @1@
  - building examples:   @2@
  - building benchmarks: @3@
'''.format(
  get_option('extrenal_fmt'),
  get_option('enable_tests'),
  get_option('enable_examples'),
  get_option('enable_benchmarks')
)

message(summary_str)