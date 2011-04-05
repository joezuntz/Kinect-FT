######################################################################################
# Packages needed for examples
######################################################################################

set(CMAKE_C_FLAGS "-Wall")

if (WIN32)
  set_source_files_properties(view_ft.c PROPERTIES LANGUAGE CXX)
  set(THREADS_USE_PTHREADS_WIN32 true)
  find_package(Threads REQUIRED)
  include_directories(${THREADS_PTHREADS_INCLUDE_DIR})
endif()

add_executable(view_ft view_ft.c)

# We need to include libfreenect_sync.h for glpclview
include_directories (../wrappers/c_sync/)

# Mac just has everything already
if(APPLE)
  set(CMAKE_EXE_LINKER_FLAGS "-framework OpenGL -framework GLUT -lfftw3")
  target_link_libraries(view_ft freenect)
  if (BUILD_C_SYNC)
    target_link_libraries(view_ft freenect_sync)
  endif()
# Linux, not so much
else()

  find_package(Threads REQUIRED)
  find_package(OpenGL REQUIRED)
  find_package(GLUT REQUIRED)

  include_directories(${OPENGL_INCLUDE_DIRS} ${GLUT_INCLUDE_DIRS} ${USB_INCLUDE_DIRS})

  if (WIN32)
    set(MATH_LIB "")
  else(WIN32)
    set(MATH_LIB "m")
  endif()

  target_link_libraries(view_ft freenect ${OPENGL_LIBRARIES} ${GLUT_LIBRARY} ${CMAKE_THREAD_LIBS_INIT} ${MATH_LIB})
endif()


install (TARGETS view_ft 
  DESTINATION bin)
