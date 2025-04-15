if(MSVC)
	message("MSCV detected")
	set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS}")
	set (CMAKE_C_FLAGS "${CMAKE_CXX_FLAGS}")
else()
	set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra -pedantic -std=c89")
	set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wnon-virtual-dtor -pedantic -std=c++03 -fno-exceptions -fno-rtti")
endif()

include_directories(include)
file(GLOB INCLUDES "spine-cpp/include/**/*.h")
file(GLOB SOURCES "spine-cpp/src/**/*.cpp")

add_library(spine-cpp STATIC ${SOURCES} ${INCLUDES})
target_include_directories(spine-cpp
  PRIVATE spine-cpp/include
  INTERFACE $<INSTALL_INTERFACE:include>
)

# Install target
install(TARGETS spine-cpp EXPORT spine DESTINATION lib)
install(FILES ${INCLUDES} DESTINATION include/spine)
install(EXPORT spine DESTINATION "lib/cmake"
        FILE spine-targets.cmake
        NAMESPACE spine::)

include(CMakePackageConfigHelpers)
# generate the config file that includes the exports
configure_package_config_file(${CMAKE_CURRENT_SOURCE_DIR}/../SpineConfig.cmake.in
  "${CMAKE_CURRENT_BINARY_DIR}/spine-config.cmake"
  INSTALL_DESTINATION "."
  NO_SET_AND_CHECK_MACRO
  NO_CHECK_REQUIRED_COMPONENTS_MACRO
  )

install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/spine-config.cmake
  DESTINATION "."
  )

