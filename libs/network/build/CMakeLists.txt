include_directories(${CPP-NETLIB_SOURCE_DIR})

include(sugar_add_library)
find_package( Boost 1.45.0 COMPONENTS unit_test_framework system regex thread filesystem )

sugar_add_library(cppnetlib-uri STATIC ${CPP-NETLIB_SOURCE_DIR}/libs/network/src/parse_uri_impl.cpp)
sugar_add_library(cppnetlib-server-parsers STATIC ${CPP-NETLIB_SOURCE_DIR}/libs/network/src/server_request_parsers_impl.cpp)

