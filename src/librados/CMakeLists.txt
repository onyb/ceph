set(librados_internal_srcs
  RadosClient.cc
  IoCtxImpl.cc
  RadosXattrIter.cc
  snap_set_diff.cc
  )
add_library(librados_internal STATIC ${librados_internal_srcs})

add_library(librados_api STATIC 
  ${CMAKE_SOURCE_DIR}/src/common/buffer.cc 
  librados.cc)

add_library(librados 
  ${CMAKE_SOURCE_DIR}/src/common/buffer.cc
  librados.cc
  #$<TARGET_OBJECTS:cls_references_objs>
  #$<TARGET_OBJECTS:common_util_obj>)
  )
#add_dependencies(librados osdc)
# LINK_PRIVATE instead of PRIVATE is used to backward compatibility with cmake 2.8.11
target_link_libraries(librados LINK_PRIVATE rados_internal cls_lock_client osdc common pthread ${CRYPTO_LIBS} {EXTRALIBS})
#osdc osd os global common cls_lock_client
#${BLKID_LIBRARIES} ${CRYPTO_LIBS} ${EXTRALIBS})
if(${ENABLE_SHARED})
  set_target_properties(librados PROPERTIES OUTPUT_NAME rados VERSION 2.0.0
    SOVERSION 2)
endif(${ENABLE_SHARED})

install(FILES include/rados/librados.h
  include/rados/rados_types.h
  include/rados/rados_types.hpp
  include/rados/librados.hpp
  include/buffer.h
  include/buffer_fwd.h
  include/page.h
  include/crc32c.h
  DESTINATION include/rados)
install(TARGETS librados DESTINATION lib)

  #librados.cc
