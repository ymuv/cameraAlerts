PROJECT(${OPENSSL_WRAPPER_LIB_NAME})

file(GLOB_RECURSE ZIP_LIB_SOURCES *cpp)
file(GLOB_RECURSE ZIP_LIB_HEADERS *.hpp)

add_library(${OPENSSL_WRAPPER_LIB_NAME} ${LIBRARY_TYPE} ${ZIP_LIB_SOURCES} ${ZIP_LIB_HEADERS})

TARGET_LINK_LIBRARIES( ${OPENSSL_WRAPPER_LIB_NAME} ${Qt5Core_LIBRARIES} )
TARGET_LINK_LIBRARIES( ${OPENSSL_WRAPPER_LIB_NAME} ${ZLIB_LIBRARIES} )

include_directories(${ZLIB_INCLUDE_DIRS})

INSTALL (
   TARGETS ${OPENSSL_WRAPPER_LIB_NAME}
   RUNTIME DESTINATION ${INSTALL_BIN_DIR}
   LIBRARY DESTINATION ${INSTALL_LIB_DIR}
   ARCHIVE DESTINATION ${INSTALL_LIB_DIR}
)

if (INSTALL_HEADERS)
   FILE(GLOB INSTALL_HEADERS  "*.hpp" )
   INSTALL (
      FILES  ${INSTALL_HEADERS}
      DESTINATION ${INSTALL_SRC_DIR}/zip/
      )
endif (INSTALL_HEADERS)

