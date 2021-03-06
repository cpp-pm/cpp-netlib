# Copyright (c) Glyn Matthews 2011.
# Copyright 2011 Dean Michael Berris (dberris@google.com)
# Copyright 2011 Google, Inc.
# Distributed under the Boost Software License, Version 1.0.
#    (See accompanying file LICENSE_1_0.txt or copy at
#          http://www.boost.org/LICENSE_1_0.txt)

include(sugar_add_library)
include(sugar_install_library)
include(sugar_target_link_libraries)

set(CPP-NETLIB_URI_SRCS uri/uri.cpp uri/schemes.cpp)
sugar_add_library(cppnetlib-uri ${CPP-NETLIB_URI_SRCS})
sugar_target_link_libraries(cppnetlib-uri ${CMAKE_DL_LIBS})

set(CPP-NETLIB_HTTP_SERVER_SRCS server_request_parsers_impl.cpp)
sugar_add_library(cppnetlib-server-parsers ${CPP-NETLIB_HTTP_SERVER_SRCS})

set(CPP-NETLIB_HTTP_CLIENT_SRCS client.cpp)
sugar_add_library(cppnetlib-client-connections ${CPP-NETLIB_HTTP_CLIENT_SRCS})

sugar_install_library(
    TARGETS
    cppnetlib-uri
    cppnetlib-server-parsers
    cppnetlib-client-connections
    DESTINATION
    lib
)
