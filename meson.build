project('mrrr', ['c', 'fortran'],
  version : '1.0',
  meson_version : '>=1.2',
  default_options : ['c_std=c11'])

# Options to mimic your make.inc flags

# Collect headers (not strictly required by Meson, but good for IDEs)
include = include_directories('INCLUDE')

# Collect C sources
# c_sources = []
subdir('SRC')


# --- BLAS / LAPACK handling ---
cc = meson.get_compiler('c')

blas = dependency('blas', required : true)
lapack = dependency('lapack', required : true)

#f_sources = []
#lapack_blas_dep = []

#if blas.found() and lapack.found()
#  # System-provided BLAS/LAPACK
#  message('Using system BLAS/LAPACK')
#  lapack_blas_dep = [blas, lapack]
#else
#  # Fall back to bundled Fortran sources
#  message('Falling back to bundled LAPACK/BLAS sources')
#
#  foreach d : src_dirs
#    f_sources += files(glob(d + '/*.f'))
#  endforeach
#endif


## Collect Fortran sources only if inclapack=true
#f_sources = []
#if get_option('inclapack')
#  f_sources = []
#  foreach d : src_dirs
#    f_sources += files(glob(d + '/*.f'))
#  endforeach
#endif

# Compiler flags
cflags = ['-DNDEBUG', '-O3', '-g', '-pthread']

if get_option('complex_support')
  cflags += ['-DCOMPLEX_SUPPORTED']
endif
if not get_option('spinlock_support')
  cflags += ['-DNOSPINLOCKS']
endif


# Build the static library
libmrrr = static_library('mrrr',
  sources : c_sources,
  include_directories : include,
  dependencies : [lapack, blas],
  c_args : cflags,
  install : true,
  install_dir : 'LIB'
)


mr3smp_dep = declare_dependency(
  link_with : libmrrr,
  include_directories : include
)

if get_option('build_examples')
  subdir('EXAMPLES')
endif
