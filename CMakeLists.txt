cmake_minimum_required(VERSION 2.8)

find_package(LibXml2 REQUIRED)
find_package(CURL REQUIRED)

include(ExternalProject)
externalproject_add(flickcurl_project
	SOURCE_DIR ${PROJECT_SOURCE_DIR}/flickcurl
	BINARY_DIR ${PROJECT_SOURCE_DIR}/flickcurl/
	CONFIGURE_COMMAND ./autogen.sh --prefix=${PROJECT_BINARY_DIR}/flickcurl/
	BUILD_COMMAND ${MAKE}
)

add_library(flickcurl STATIC IMPORTED)
set_target_properties(flickcurl PROPERTIES IMPORTED_LOCATION "${PROJECT_BINARY_DIR}/flickcurl/lib/libflickcurl.a")

include_directories("${PROJECT_BINARY_DIR}/flickcurl/include" "${LIBXML2_INCLUDE_DIR}")
link_directories("${LIBXML2_LIBRARIES}")

add_executable(example src/example.c)
target_link_libraries(example flickcurl curl xml2)