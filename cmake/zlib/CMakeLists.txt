find_package(Git)

if (NOT GIT_FOUND)
    message(FATAL_ERROR "Git not found")
endif()

option(ZLIB_ASM686 "Enable building i686 assembly implementation")
option(ZLIB_AMD64  "Enable building i686 assembly implementation")

set(ZLIB_TAG_VERSION "v1.2.12" CACHE PATH "")
set(ZLIB_INSTALL_PREFIX "/zlib" CACHE PATH "")
set(ZLIB_INSTALL_BIN_DIR "/bin" CACHE PATH 
        "Installation directory for executables")
set(ZLIB_INSTALL_LIB_DIR "/lib" CACHE PATH 
        "Installation directory for libraries")
set(ZLIB_INSTALL_INC_DIR "/include" CACHE PATH 
        "Installation directory for headers")
set(ZLIB_INSTALL_MAN_DIR "/share/man" CACHE PATH 
        "Installation directory for manual pages")
set(ZLIB_INSTALL_PKGCONFIG_DIR "/share/pkgconfig" CACHE PATH 
        "Installation directory for pkgconfig (.pc) files")

execute_process(COMMAND "${GIT_EXECUTABLE}" "fetch" "--tags" 
    WORKING_DIRECTORY ../../external/zlib)
execute_process(COMMAND "${GIT_EXECUTABLE}" "checkout" "${ZLIB_TAG_VERSION}" 
    WORKING_DIRECTORY ../../external/zlib)

ExternalProject_Add(zlib PREFIX ZLIB SOURCE_DIR ../../external/zlib 
    CMAKE_CACHE_ARGS 
        -DASM686:BOOL=${ZLIB_ASM686} 
        -DAMD64:BOOL=${ZLIB_AMD64} 
        -DCMAKE_INSTALL_PREFIX:PATH=${CMAKE_INSTALL_PREFIX}${ZLIB_INSTALL_PREFIX}
        -DINSTALL_BIN_DIR:PATH=${CMAKE_INSTALL_PREFIX}${ZLIB_INSTALL_PREFIX}${ZLIB_INSTALL_BIN_DIR}
        -DINSTALL_LIB_DIR:PATH=${CMAKE_INSTALL_PREFIX}${ZLIB_INSTALL_PREFIX}${ZLIB_INSTALL_LIB_DIR}
        -DINSTALL_INC_DIR:PATH=${CMAKE_INSTALL_PREFIX}${ZLIB_INSTALL_PREFIX}${ZLIB_INSTALL_INC_DIR}
        -DINSTALL_MAN_DIR:PATH=${CMAKE_INSTALL_PREFIX}${ZLIB_INSTALL_PREFIX}${ZLIB_INSTALL_MAN_DIR}
        -DINSTALL_PKGCONFIG_DIR:PATH=${CMAKE_INSTALL_PREFIX}${ZLIB_INSTALL_PREFIX}${ZLIB_INSTALL_PKGCONFIG_DIR}

    )


