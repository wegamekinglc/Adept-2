file(GLOB_RECURSE ADEPT_FILES "*.cpp")
add_library(adept ${ADEPT_FILES})

install(DIRECTORY ../include DESTINATION . FILES_MATCHING PATTERN "*.hpp" PATTERN "*.h")

install(TARGETS adept
        RUNTIME DESTINATION lib
        PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
)