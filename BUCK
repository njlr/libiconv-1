def merge_dicts(x, y):
  z = x.copy()
  z.update(y)
  return z

genrule(
  name = 'make',
  out = 'out',
  srcs = glob([
    'cmake/**/*.cmake',
    'include/**/*.h',
    'include/**/*.in',
    'include/**/*.txt',
    'lib/**/*.c',
    'lib/**/*.h',
    'lib/**/*.def',
    'libcharset/**/*.c',
    'libcharset/**/*.h',
    'libcharset/**/*.in',
    'libcharset/**/*.txt',
    'srclib/**/*.c',
    'srclib/**/*.h',
    'srclib/**/*.in',
    'srclib/**/*.txt',
    'src/**/*.c',
    'src/**/*.in',
    'src/**/*.txt',
    'CMakeLists.txt',
    'config.h.cmake',
    'config.h.in',
    'dist.info',
  ]),
  cmd = 'mkdir $OUT && cd $OUT && cmake $SRCDIR && make ',
)

headers = [
  'config.h',
  'iconv.h',
  'libcharset.h',
  'localcharset.h',
]

def extract(x):
  genrule(
    name = x,
    out = x,
    cmd = 'cp $(location :make)/include/' + x + ' $OUT',
  )
  return ':' + x

prebuilt_cxx_library(
  name = 'libiconv',
  header_namespace = '',
  exported_headers = merge_dicts(
    dict({ (x, extract(x)) for x in headers }),
    subdir_glob([
      ('srclib', '*.h'),
    ]),
  ),
  lib_name = 'iconv',
  lib_dir = '$(location :make)',
  visibility = [
    'PUBLIC',
  ],
)
