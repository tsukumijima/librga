project(
    'librga',
    'cpp',
    version : '2.2.0',
    meson_version : '>=0.47.0',
    default_options : ['warning_level=3', 'cpp_std=c++14']
)

pkgconfig = import('pkgconfig')

add_project_arguments('-DLINUX=1', language : 'cpp')

incdir = include_directories('include')

# Create dummy source for building dummy libraries
dummy_source = join_paths(meson.current_build_dir(), 'dummy.cpp')
run_command('touch', dummy_source, check : false)

# Create a dummy library which will be replaced by the prebuilt librga library
librga = shared_library(
    'rga',
    dummy_source,
	include_directories : incdir,
    version : meson.project_version(),
    install : true,
)

# Install the prebuilt librga library
install_data(
    'libs/Linux/gcc-aarch64/librga.so',
    install_dir : '/usr/lib/aarch64-linux-gnu',
    rename : 'librga.so.2.2.0',
)
install_data(
    'libs/Linux/gcc-aarch64/librga.a',
    install_dir : '/usr/lib/aarch64-linux-gnu',
    rename : 'librga.a',
)

install_headers(
    'include/drmrga.h',
    'include/GrallocOps.h',
    'include/im2d_buffer.h',
    'include/im2d_common.h',
    'include/im2d_expand.h',
    'include/im2d_mpi.h',
    'include/im2d_single.h',
    'include/im2d_task.h',
    'include/im2d_type.h',
    'include/im2d_version.h',
    'include/im2d.h',
    'include/im2d.hpp',
    'include/rga.h',
    'include/RgaApi.h',
    'include/RgaMutex.h',
    'include/RgaSingleton.h',
    'include/RgaUtils.h',
    'include/RockchipRga.h',
    subdir : 'rga',
)

pkgconfig.generate(
    libraries : librga,
    filebase : 'librga',
    name : 'librga',
    version : meson.project_version(),
    description : 'Userspace interface to Rockchip RGA 2D accelerator',
)

librga_demo_option = get_option('librga_demo')
if librga_demo_option != 'false'
    demo_src = [
		'samples/im2d_api_demo/rgaImDemo.cpp',
		'samples/im2d_api_demo/args.cpp'
	]
    demo_incdir = include_directories('include', 'im2d_api')
    librga_dep = dependency('librga')
    executable(
            'rgaImDemo',
            demo_src,
            include_directories : demo_incdir,
            dependencies : librga_dep,
            cpp_args : ['-Wno-pedantic'],
            install : true,
    )
endif
