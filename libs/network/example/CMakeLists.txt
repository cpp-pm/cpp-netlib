#            Copyright (c) Dean Michael Berris 2010.
# Distributed under the Boost Software License, Version 1.0.
#    (See accompanying file LICENSE_1_0.txt or copy at
#          http://www.boost.org/LICENSE_1_0.txt)

include(sugar_add_executable)
include(sugar_target_link_libraries)

sugar_add_executable(http_client http_client.cpp)
sugar_add_executable(simple_wget simple_wget.cpp)
sugar_add_executable(atom_reader atom/atom.cpp atom/main.cpp)
sugar_add_executable(rss_reader rss/rss.cpp rss/main.cpp)
sugar_add_executable(twitter_search twitter/search.cpp)
sugar_add_executable(hello_world_server http/hello_world_server.cpp)
sugar_add_executable(hello_world_client http/hello_world_client.cpp)
if (UNIX)
  sugar_add_executable(fileserver http/fileserver.cpp)
endif (UNIX)
add_dependencies(http_client cppnetlib-uri cppnetlib-client-connections)
add_dependencies(simple_wget cppnetlib-uri cppnetlib-client-connections)
add_dependencies(atom_reader cppnetlib-uri cppnetlib-client-connections)
add_dependencies(rss_reader cppnetlib-uri cppnetlib-client-connections)
add_dependencies(twitter_search cppnetlib-uri cppnetlib-client-connections)
set(BOOST_CLIENT_LIBS
    ${Boost_PROGRAM_OPTIONS_LIBRARY}
    ${Boost_THREAD_LIBRARY}
    ${Boost_DATE_TIME_LIBRARY}
    ${Boost_REGEX_LIBRARY}
    ${Boost_SYSTEM_LIBRARY}
    ${Boost_FILESYSTEM_LIBRARY}
    ${Boost_CHRONO_LIBRARY})

set(BOOST_SERVER_LIBS
    ${Boost_THREAD_LIBRARY}
    ${Boost_SYSTEM_LIBRARY}
    ${Boost_DATE_TIME_LIBRARY}
    ${Boost_PROGRAM_OPTIONS_LIBRARY}
    ${Boost_FILESYSTEM_LIBRARY}
    ${Boost_CHRONO_LIBRARY})

sugar_target_link_libraries(http_client
    ${BOOST_CLIENT_LIBS}
    ${CMAKE_THREAD_LIBS_INIT}
    cppnetlib-uri
    cppnetlib-client-connections)

sugar_target_link_libraries(simple_wget
    ${BOOST_CLIENT_LIBS}
    ${CMAKE_THREAD_LIBS_INIT}
    cppnetlib-uri
    cppnetlib-client-connections)

sugar_target_link_libraries(atom_reader
    ${BOOST_CLIENT_LIBS}
    ${CMAKE_THREAD_LIBS_INIT}
    cppnetlib-uri
    cppnetlib-client-connections)

sugar_target_link_libraries(rss_reader
    ${BOOST_CLIENT_LIBS}
    ${CMAKE_THREAD_LIBS_INIT}
    cppnetlib-uri
    cppnetlib-client-connections)

sugar_target_link_libraries(twitter_search
    ${BOOST_CLIENT_LIBS}
    ${CMAKE_THREAD_LIBS_INIT}
    cppnetlib-uri
    cppnetlib-client-connections)

sugar_target_link_libraries(hello_world_server
    ${BOOST_SERVER_LIBS}
    ${CMAKE_THREAD_LIBS_INIT})

sugar_target_link_libraries(hello_world_client
    ${BOOST_CLIENT_LIBS}
    ${CMAKE_THREAD_LIBS_INIT}
    cppnetlib-uri
    cppnetlib-client-connections)

if (OPENSSL_FOUND)
  sugar_target_link_libraries(http_client ${OPENSSL_LIBRARIES})
  sugar_target_link_libraries(simple_wget ${OPENSSL_LIBRARIES})
  sugar_target_link_libraries(atom_reader ${OPENSSL_LIBRARIES})
  sugar_target_link_libraries(rss_reader ${OPENSSL_LIBRARIES})
  sugar_target_link_libraries(twitter_search ${OPENSSL_LIBRARIES})
  sugar_target_link_libraries(hello_world_server ${OPENSSL_LIBRARIES})
  sugar_target_link_libraries(hello_world_client ${OPENSSL_LIBRARIES})
endif (OPENSSL_FOUND)

if (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
  sugar_target_link_libraries(http_client rt)
  sugar_target_link_libraries(simple_wget rt)
  sugar_target_link_libraries(atom_reader rt)
  sugar_target_link_libraries(rss_reader rt)
  sugar_target_link_libraries(twitter_search rt)
  sugar_target_link_libraries(hello_world_server rt)
  sugar_target_link_libraries(hello_world_client rt)
endif()

if (UNIX)
  sugar_target_link_libraries(fileserver
    ${BOOST_SERVER_LIBS}
    ${Boost_FILESYSTEM_LIBRARY}
    ${CMAKE_THREAD_LIBS_INIT}
    cppnetlib-server-parsers)
  if (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    sugar_target_link_libraries(fileserver rt)
  endif()
endif (UNIX)
