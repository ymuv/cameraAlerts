PROJECT(${MAIN_NAME})

file(GLOB_RECURSE MAIN_SOURCES *cpp)
file(GLOB_RECURSE MAIN_HEADERS *.hpp)

ADD_EXECUTABLE(${MAIN_NAME} ${MAIN_SOURCES} ${MAIN_HEADERS})
TARGET_LINK_LIBRARIES( ${MAIN_NAME} ${COMMON_LIB_NAME})
TARGET_LINK_LIBRARIES( ${MAIN_NAME} ${HELP_LINK_LIBRARIES})
TARGET_LINK_LIBRARIES( ${MAIN_NAME} ${LOG_LIB_NAME} )
TARGET_LINK_LIBRARIES( ${MAIN_NAME} ${WEB_DOWNLOADER_LIB_NAME} )

#TODO: IF CLIENT_LIB_NAME build

IF (BUILD_WITH_CLIENT)
    TARGET_LINK_LIBRARIES( ${MAIN_NAME} ${CLIENT_LIB_NAME} )
    ADD_DEFINITIONS(-D__BUILD_CLIENTS__)
ELSE ()
    REMOVE_DEFINITIONS(-D__BUILD_CLIENTS__)
ENDIF (BUILD_WITH_CLIENT)

IF (BUILD_MJPEG)
    ADD_DEFINITIONS(-D__BUILD_MJPEG__)
    TARGET_LINK_LIBRARIES( ${MAIN_NAME} ${MJPEG_SERVER_LIB_NAME} )
ENDIF (BUILD_MJPEG)

IF (BUILD_SERVERS)
    TARGET_LINK_LIBRARIES( ${MAIN_NAME} ${NET_LIB_NAME} )
    TARGET_LINK_LIBRARIES( ${MAIN_NAME} ${IMAGE_SERVER_LIB_NAME} )
    ADD_DEFINITIONS(-D__BUILD_SERVERS__)
ELSE (BUILD_SERVERS)
    REMOVE_DEFINITIONS(-D__BUILD_SERVERS__ )
ENDIF (BUILD_SERVERS)

IF (BUILD_VIDEOCAPTURE)
   ADD_DEFINITIONS(-D__BUILD_VIDEOCAPTURE__)
ELSE ()
   REMOVE_DEFINITIONS(-D__BUILD_VIDEOCAPTURE__)
ENDIF (BUILD_VIDEOCAPTURE)

IF (BUILD_ALGO_RND)
    TARGET_LINK_LIBRARIES( ${MAIN_NAME} ${ALGO_RND_LIB_NAME})
ENDIF (BUILD_ALGO_RND)

IF (BUILD_SERIAL)
    TARGET_LINK_LIBRARIES( ${MAIN_NAME} ${SERIAL_LIB_NAME} )
ENDIF (BUILD_SERIAL)

SET (MAIN_BIN_DIR  ${INSTALL_BIN_DIR}/main/)

add_definitions(-D__INSTALL_BIN_DIR__="${CMAKE_INSTALL_PREFIX}/${MAIN_BIN_DIR}")

INSTALL (
    TARGETS ${MAIN_NAME}
    RUNTIME DESTINATION ${INSTALL_BIN_DIR}
    LIBRARY DESTINATION ${INSTALL_LIB_DIR}
    ARCHIVE DESTINATION ${INSTALL_LIB_DIR}
    )

IF (INSTALL_CONFIG)
    SET(EXE_NAME ${CMAKE_INSTALL_PREFIX}/${INSTALL_BIN_DIR}/${MAIN_NAME})
    SET(OUT_LOOP_FILE "${CMAKE_CURRENT_BINARY_DIR}/run_loop_${MAIN_NAME}.sh")
    CONFIGURE_FILE (${RUN_LOOP_FILE} "${OUT_LOOP_FILE}" )
    INSTALL (
        FILES  ${CONFIG_FILE} ${OUT_LOOP_FILE} ${INSTALL_FILES}
        DESTINATION ${INSTALL_BIN_DIR}
        )
ENDIF (INSTALL_CONFIG)



