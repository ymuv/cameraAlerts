PROJECT(${WEB_DOWNLOADER_LIB_NAME})

file(GLOB_RECURSE WEB_SOURCES *cpp *.hpp)
file(GLOB_RECURSE WEB_HEADERS )
FIND_PACKAGE (Qt5 COMPONENTS Network REQUIRED)

ADD_LIBRARY(${WEB_DOWNLOADER_LIB_NAME} ${LIBRARY_TYPE} ${WEB_SOURCES})
set_target_properties(${WEB_DOWNLOADER_LIB_NAME}  PROPERTIES AUTOMOC TRUE)

INCLUDE_DIRECTORIES(${Qt5Network_INCLUDE_DIRS})
TARGET_LINK_LIBRARIES( ${WEB_DOWNLOADER_LIB_NAME} ${Qt5Network_LIBRARIES}  )

INSTALL (
   TARGETS ${WEB_DOWNLOADER_LIB_NAME}
   RUNTIME DESTINATION ${INSTALL_BIN_DIR}
   LIBRARY DESTINATION ${INSTALL_LIB_DIR}
   ARCHIVE DESTINATION ${INSTALL_LIB_DIR}
)

IF (INSTALL_HEADERS)
    FILE(GLOB INSTALL_HEADERS  "*.hpp" )
    STRING(REPLACE ${CMAKE_ROOT_SOURCES} ""  INSTALL_PATH ${PROJECT_SOURCE_DIR})
    INSTALL (
        FILES  ${INSTALL_HEADERS}
        DESTINATION ${INSTALL_SRC_DIR}/${INSTALL_PATH}/
    )
ENDIF (INSTALL_HEADERS)
