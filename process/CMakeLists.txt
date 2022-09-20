find_package(Draco)

if (NOT DRACO_INCLUDES)
  message(FATAL_ERROR "Draco package not found.")
endif ()

find_package(GIFLIB)
if (NOT GIFLIB_FOUND)
  message(FATAL_ERROR "Giflib package not found.")
endif ()

find_package(TurboJPEG)
if (NOT TurboJPEG_FOUND)
  message(FATAL_ERROR "Turbo Jpeg package not found.")
endif ()

find_package(PNG)
if (NOT PNG_FOUND)
  message(FATAL_ERROR "PNG package not found.")
endif ()

include_directories(
  ..
  ${USD_INCLUDE_DIRS}
  ${DRACO_INCLUDES}
  ${GIFLIB_INCLUDE_DIR}
  ${TurboJPEG_INCLUDE_DIRS}
)

add_library(process
  image_png.h
  math.cc
  math.h
  mesh.cc
  mesh.h
  process_util.cc
  process_util.h
  skin.cc
  skin.h
  access.h
  animation.cc
  animation.h
  color.cc
  color.h
  float_image.cc
  float_image.h
  image.cc
  image.h
  image_fallback.cc
  image_fallback.h
  image_gif.cc
  image_gif.h
  image_jpg.cc
  image_jpg.h
  image_png.cc
)

file(GLOB PROCESS_HEADERS "*.h")
set_target_properties(process PROPERTIES PUBLIC_HEADER "${PROCESS_HEADERS}")

message("PNG: ${PNG_LIBRARY}")

if (MSVC)
target_link_libraries(process
  common
  "${DRACO_LIBRARY}"
  "${GIFLIB_LIBRARY}"
  "${TurboJPEG_LIBRARIES}"
  "${PNG_LIBRARY}"
  "${ZLIB_LIBRARIES}"
)
else (MSVC)

target_link_libraries(process
  common
  "${DRACO_LIBRARY}"
  "${GIFLIB_LIBRARY}"
  "${TurboJPEG_LIBRARIES}"
  "${PNG_LIBRARY}"
  "${ZLIB_LIBRARIES}"
)
endif (MSVC)

install(TARGETS process EXPORT ufglib DESTINATION lib/ufg PUBLIC_HEADER DESTINATION include/ufg/process)
