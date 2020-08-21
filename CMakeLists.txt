cmake_minimum_required(VERSION 2.8.11)

project(sscep)

set(CPACK_PACKAGE_VERSION_MAJOR 0)
set(CPACK_PACKAGE_VERSION_MINOR 7)
set(CPACK_PACKAGE_VERSION_PATCH 0)

find_package(OpenSSL REQUIRED)

set(SSCEP_SOURCES
	src/cmd.c
	src/sscep.c
	src/init.c
	src/net.c
	src/sceputils.c
	src/pkcs7.c
	src/ias.c
	src/fileutils.c
	src/configuration.c
	src/engine.c
)

if (WIN32)
	list(APPEND SSCEP_SOURCES src/engines/sscep_capi.c)
endif()

add_executable(${PROJECT_NAME} ${SSCEP_SOURCES})
include_directories(${OPENSSL_INCLUDE_DIR})

target_link_libraries(${PROJECT_NAME}
	${OPENSSL_LIBRARIES}
)

install(
	TARGETS ${PROJECT_NAME}
	DESTINATION bin
)

if (CMAKE_SYSTEM_NAME MATCHES "Linux")
    set(LINUX TRUE)
endif ()

if (CMAKE_SYSTEM_NAME MATCHES "(Solaris|SunOS)")
    set(SOLARIS TRUE)
endif ()

if(LINUX) 
	message(STATUS "Packaging RPM for Linux")

	set(CPACK_GENERATOR "RPM")
elseif(SOLARIS)
	message(STATUS "Packaging RPM for Non Solaris")

	set(CPACK_GENERATOR "RPM")
else()
	message(STATUS "Packaging RPM for Non Linux")

	set(CPACK_GENERATOR "RPM")
endif()

set(CPACK_RPM_PACKAGE_SUMMARY "Simple SCEP client")
set(CPACK_PACKAGE_DESCRIPTION "Simple SCEP (Simple Certificate Enrollment Protocol) client.")
set(CPACK_PACKAGE_RELEASE 1)
set(CPACK_RPM_PACKAGE_LICENSE "GPL")
set(CPACK_RPM_PACKAGE_GROUP "Productivity/Security")
set(CPACK_PACKAGE_HOMEPAGE_URL "https://github.com/certnanny/sscep")
set(CPACK_PACKAGE_CONTACT "m.seker@aol.com")
set(CPACK_PACKAGE_VENDOR "CertNanny")
set(CPACK_PACKAGING_INSTALL_PREFIX ${CMAKE_INSTALL_PREFIX})
set(CPACK_RPM_PACKAGE_AUTOREQPROV "yes")

include(CPack)
