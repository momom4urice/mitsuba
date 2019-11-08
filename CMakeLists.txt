cmake_minimum_required(VERSION 3.4)
project(mitsuba)

set(CMAKE_CXX_STANDARD 11)

find_package(Boost REQUIRED COMPONENTS filesystem)
find_package(XercesC)

# Configuration
option(MTS_FLOAT_AS_DOUBLE "Use 64-bit floats" OFF)
IF (MTS_FLOAT_AS_DOUBLE)
    add_compile_definitions(DOUBLE_PRECISION)
ELSE ()
    add_compile_definitions(SINGLE_PRECISION)
ENDIF ()

add_compile_definitions(SPECTRUM_SAMPLES=3)

# Source declaration
set(CORE_H
    include/mitsuba/core/aabb.h
    include/mitsuba/core/aabb_sse.h
    include/mitsuba/core/appender.h
    include/mitsuba/core/atomic.h
    include/mitsuba/core/autodiff.h
    include/mitsuba/core/barray.h
    include/mitsuba/core/bitmap.h
    include/mitsuba/core/brent.h
    include/mitsuba/core/bsphere.h
    include/mitsuba/core/chisquare.h
    include/mitsuba/core/class.h
    include/mitsuba/core/cobject.h
    include/mitsuba/core/constants.h
    include/mitsuba/core/cstream.h
    include/mitsuba/core/formatter.h
    include/mitsuba/core/frame.h
    include/mitsuba/core/fresolver.h
    include/mitsuba/core/fstream.h
    include/mitsuba/core/fwd.h
    include/mitsuba/core/getopt.h
    include/mitsuba/core/half.h
    include/mitsuba/core/kdtree.h
    include/mitsuba/core/lock.h
    include/mitsuba/core/logger.h
    include/mitsuba/core/lrucache.h
    include/mitsuba/core/math.h
    include/mitsuba/core/matrix.h
    include/mitsuba/core/matrix.inl
    include/mitsuba/core/mempool.h
    include/mitsuba/core/mmap.h
    include/mitsuba/core/mstream.h
    include/mitsuba/core/netobject.h
    include/mitsuba/core/normal.h
    include/mitsuba/core/object.h
    include/mitsuba/core/octree.h
    include/mitsuba/core/platform.h
    include/mitsuba/core/plugin.h
    include/mitsuba/core/pmf.h
    include/mitsuba/core/point.h
    include/mitsuba/core/properties.h
    include/mitsuba/core/qmc.h
    include/mitsuba/core/quad.h
    include/mitsuba/core/quat.h
    include/mitsuba/core/random.h
    include/mitsuba/core/ray.h
    include/mitsuba/core/ray_sse.h
    include/mitsuba/core/ref.h
    include/mitsuba/core/rfilter.h
    include/mitsuba/core/sched.h
    include/mitsuba/core/sched_remote.h
    include/mitsuba/core/serialization.h
    include/mitsuba/core/sfcurve.h
    include/mitsuba/core/shvector.h
    include/mitsuba/core/simplecache.h
    include/mitsuba/core/spectrum.h
    include/mitsuba/core/spline.h
    include/mitsuba/core/sse.h
    include/mitsuba/core/ssemath.h
    include/mitsuba/core/ssevector.h
    include/mitsuba/core/sshstream.h
    include/mitsuba/core/sstream.h
    include/mitsuba/core/statistics.h
    include/mitsuba/core/stream.h
    include/mitsuba/core/thread.h
    include/mitsuba/core/timer.h
    include/mitsuba/core/tls.h
    include/mitsuba/core/track.h
    include/mitsuba/core/transform.h
    include/mitsuba/core/triangle.h
    include/mitsuba/core/util.h
    include/mitsuba/core/vector.h
    include/mitsuba/core/version.h
    include/mitsuba/core/vmf.h
    include/mitsuba/core/warp.h
    include/mitsuba/core/zstream.h)

set(CORE_SRC
    src/libcore/aabb.cpp
    src/libcore/appender.cpp
    src/libcore/bitmap.cpp
    src/libcore/brent.cpp
    src/libcore/chisquare.cpp
    src/libcore/class.cpp
    src/libcore/cstream.cpp
    src/libcore/fmtconv.cpp
    src/libcore/formatter.cpp
    src/libcore/fresolver.cpp
    src/libcore/fstream.cpp
    src/libcore/getopt.c
    src/libcore/half.cpp
    src/libcore/lock.cpp
    src/libcore/logger.cpp
    src/libcore/math.cpp
    src/libcore/mmap.cpp
    src/libcore/mstream.cpp
    src/libcore/object.cpp
    src/libcore/platform_darwin.mm
    src/libcore/plugin.cpp
    src/libcore/properties.cpp
    src/libcore/qmc.cpp
    src/libcore/quad.cpp
    src/libcore/random.cpp
    src/libcore/rfilter.cpp
    src/libcore/sched.cpp
    src/libcore/sched_remote.cpp
    src/libcore/serialization.cpp
    src/libcore/shvector.cpp
    src/libcore/spectrum.cpp
    src/libcore/spline.cpp
    src/libcore/ssemath.cpp
    src/libcore/sshstream.cpp
    src/libcore/sstream.cpp
    src/libcore/statistics.cpp
    src/libcore/stream.cpp
    src/libcore/thread.cpp
    src/libcore/timer.cpp
    src/libcore/tls.cpp
    src/libcore/track.cpp
    src/libcore/transform.cpp
    src/libcore/triangle.cpp
    src/libcore/util.cpp
    src/libcore/vmf.cpp
    src/libcore/warp.cpp
    src/libcore/zstream.cpp)

add_library(libcore ${CORE_SRC} ${CORE_H})
target_include_directories(libcore PRIVATE include)
target_link_libraries(libcore PRIVATE Boost::filesystem)

set(RENDER_H
    include/mitsuba/render/fwd.h
    include/mitsuba/render/photon.h
    include/mitsuba/render/sahkdtree4.h
    include/mitsuba/render/noise.h
    include/mitsuba/render/range.h
    include/mitsuba/render/scenehandler.h
    include/mitsuba/render/shader.h
    include/mitsuba/render/emitter.h
    include/mitsuba/render/imageproc.h
    include/mitsuba/render/sensor.h
    include/mitsuba/render/sampler.h
    include/mitsuba/render/imageblock.h
    include/mitsuba/render/photonmap.h
    include/mitsuba/render/irrcache.h
    include/mitsuba/render/triaccel.h
    include/mitsuba/render/spiral.h
    include/mitsuba/render/records.inl
    include/mitsuba/render/skdtree.h
    include/mitsuba/render/sahkdtree3.h
    include/mitsuba/render/vpl.h
    include/mitsuba/render/subsurface.h
    include/mitsuba/render/medium.h
    include/mitsuba/render/gkdtree.h
    include/mitsuba/render/shape.h
    include/mitsuba/render/renderqueue.h
    include/mitsuba/render/renderproc.h
    include/mitsuba/render/testcase.h
    include/mitsuba/render/common.h
    include/mitsuba/render/sahkdtree2.h
    include/mitsuba/render/renderjob.h
    include/mitsuba/render/util.h
    include/mitsuba/render/integrator.h
    include/mitsuba/render/texture.h
    include/mitsuba/render/volume.h
    include/mitsuba/render/particleproc.h
    include/mitsuba/render/film.h
    include/mitsuba/render/phase.h
    include/mitsuba/render/triaccel_sse.h
    include/mitsuba/render/rectwu.h
    include/mitsuba/render/mipmap.h
    include/mitsuba/render/gatherproc.h
    include/mitsuba/render/bsdf.h
    include/mitsuba/render/trimesh.h
    include/mitsuba/render/scene.h)

set(RENDER_SRC
    src/librender/phase.cpp
    src/librender/common.cpp
    src/librender/intersection.cpp
    src/librender/util.cpp
    src/librender/gatherproc.cpp
    src/librender/scene.cpp
    src/librender/bsdf.cpp
    src/librender/emitter.cpp
    src/librender/scenehandler.cpp
    src/librender/renderproc.cpp
    src/librender/testcase.cpp
    src/librender/subsurface.cpp
    src/librender/rectwu.cpp
    src/librender/photonmap.cpp
    src/librender/film.cpp
    src/librender/texture.cpp
    src/librender/skdtree.cpp
    src/librender/sampler.cpp
    src/librender/imageproc.cpp
    src/librender/noise.cpp
    src/librender/shader.cpp
    src/librender/renderjob.cpp
    src/librender/particleproc.cpp
    src/librender/medium.cpp
    src/librender/shape.cpp
    src/librender/integrator.cpp
    src/librender/imageblock.cpp
    src/librender/sensor.cpp
    src/librender/irrcache.cpp
    src/librender/SConscript
    src/librender/vpl.cpp
    src/librender/renderqueue.cpp
    src/librender/trimesh.cpp
    src/librender/photon.cpp
    src/librender/volume.cpp)

add_library(librender ${RENDER_SRC} ${RENDER_H})
target_include_directories(librender PUBLIC include)
target_link_libraries(librender
    PUBLIC libcore
    PRIVATE Boost::filesystem)

add_executable(plugin_bsdfs
    src/bsdfs/diffuse.cpp
    src/bsdfs/hk.cpp)
target_link_libraries(plugin_bsdfs PRIVATE librender)

add_executable(plugin_emitters
    src/emitters/area.cpp
    src/emitters/envmap.cpp)
target_link_libraries(plugin_emitters PRIVATE librender Boost::filesystem)

add_executable(plugin_films
    src/films/annotations.h
    src/films/banner.h
    src/films/cnpy.cpp
    src/films/cnpy.h
    src/films/hdrfilm.cpp
    src/films/ldrfilm.cpp
    src/films/mfilm.cpp
    src/films/tiledhdrfilm.cpp)
target_link_libraries(plugin_films PRIVATE librender Boost::filesystem)

add_executable(plugin_integrator_direct
    src/integrators/direct/ao.cpp
    src/integrators/direct/direct.cpp)
target_link_libraries(plugin_integrator_direct PRIVATE librender Boost::filesystem)

add_executable(plugin_integrator_path
    src/integrators/path/path.cpp
    src/integrators/path/volpath.cpp
    src/integrators/path/volpath_simple.cpp)
target_link_libraries(plugin_integrator_path PRIVATE librender Boost::filesystem)

add_executable(plugin_integrator_ptracer
    src/integrators/ptracer/ptracer.cpp
    src/integrators/ptracer/ptracer_proc.cpp
    src/integrators/ptracer/ptracer_proc.h)
target_link_libraries(plugin_integrator_ptracer PRIVATE librender Boost::filesystem)

add_executable(plugin_media
    src/medium/homogeneous.cpp)
target_link_libraries(plugin_media PRIVATE librender Boost::filesystem)

add_executable(plugin_samplers
    src/samplers/independent.cpp
    src/samplers/halton.cpp
    src/samplers/stratified.cpp)
target_link_libraries(plugin_samplers PRIVATE librender)

add_executable(plugin_sensors
    src/sensors/irradiancemeter.cpp
    src/sensors/perspective.cpp)
target_link_libraries(plugin_sensors PRIVATE librender Boost::filesystem)

add_executable(plugin_shapes
    src/shapes/cube.cpp
    src/shapes/sphere.cpp)
target_link_libraries(plugin_shapes PRIVATE librender)

add_executable(plugin_subsurface
    src/subsurface/dipole.cpp)
target_link_libraries(plugin_subsurface PRIVATE librender)

add_executable(mitsuba
    src/mitsuba/mitsuba.cpp)
target_link_libraries(mitsuba PRIVATE libcore librender Boost::headers XercesC::XercesC)