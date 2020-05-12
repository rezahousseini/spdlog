project('spdlog', 'cpp', 
  version: '1.5.0',
  license: 'MIT',
  default_options : ['warning_level=3', 'cpp_std=c++11']
)

thread_dep = dependency('threads')

src = ['src/spdlog.cpp', 'src/stdout_sinks.cpp', 'src/color_sinks.cpp', 
       'src/file_sinks.cpp', 'src/async.cpp', 'src/cfg.cpp']

if not get_option('SPDLOG_FMT_EXTERNAL')
  src += 'src/fmt.cpp'
endif

if get_option('SPDLOG_COMPILED_LIB')

  spdlog_lib = library('spdlog', src, 
    include_directories : include_directories('include'),
    cpp_args : '-DSPDLOG_COMPILED_LIB',
    dependencies : thread_dep,
    install : not meson.is_subproject())

  spdlog_dep = declare_dependency(
    include_directories: include_directories('include'),
    link_with : spdlog_lib,
    dependencies : thread_dep
  )

else

  spdlog_dep = declare_dependency(
    include_directories: include_directories('include'),
    dependencies : thread_dep
  )

endif

if not meson.is_subproject()
  install_subdir('include/spdlog', install_dir: 'include/spdlog', strip_directory : true)
  
  pkgc = import('pkgconfig')
  pkgc.generate(name: 'spdlog',
    version: meson.project_version(),
    description : 'Fast C++ logging library'
  )
endif

if get_option('SPDLOG_BUILD_EXAMPLE')
  message('Generating example(s)')
  subdir('example')
endif

if get_option('SPDLOG_BUILD_TESTS')
  message('Generating tests')
  subdir('tests')
endif

if get_option('SPDLOG_BUILD_BENCH')
  message('Generating benchmarks')
  subdir('bench')
endif 
