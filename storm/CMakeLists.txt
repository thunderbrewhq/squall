include(CheckCXXCompilerFlag)
check_cxx_compiler_flag(-Wno-invalid-offsetof HAS_NO_INVALID_OFFSETOF)

file(GLOB STORM_SOURCES
    "*.cpp"
    "big/*.cpp"
    "crypto/*.cpp"
    "error/*.cpp"
    "hash/*.cpp"
    "option*/*.cpp"
    "queue/*.cpp"
    "string/*.cpp"
    "thread/*.cpp"
)

if(WHOA_SYSTEM_WIN)
    file(GLOB STORM_WIN_SOURCES
        "win/*.cpp"
        "error/win/*.cpp"
        "thread/win/*.cpp"
        "registry/win/*.cpp"
    )
    list(APPEND STORM_SOURCES ${STORM_WIN_SOURCES})
endif()

if(WHOA_SYSTEM_MAC)
    file(GLOB STORM_MAC_SOURCES
        "mac/*.cpp"
        "mac/*.mm"
        "error/unix/*.cpp"
        "thread/mac/*.cpp"
        "thread/mac/*.mm"
        "registry/mac/*.mm"
    )
    list(APPEND STORM_SOURCES ${STORM_MAC_SOURCES})
endif()

if(WHOA_SYSTEM_LINUX)
    file(GLOB STORM_LINUX_SOURCES
        "linux/*.cpp"
        "error/unix/*.cpp"
        "thread/linux/*.cpp"
        "registry/linux/*.cpp"
    )
    list(APPEND STORM_SOURCES ${STORM_LINUX_SOURCES})
endif()

add_library(storm STATIC
    ${STORM_SOURCES}
)

target_include_directories(storm
    PUBLIC
        ${PROJECT_SOURCE_DIR}
)

target_link_libraries(storm
    PUBLIC
        bc
    PRIVATE
        system
)

if(HAS_NO_INVALID_OFFSETOF)
    target_compile_options(storm
        PUBLIC
            -Wno-invalid-offsetof
    )
endif()
