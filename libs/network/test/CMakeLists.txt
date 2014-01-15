#            Copyright (c) Dean Michael Berris 2010.
# Distributed under the Boost Software License, Version 1.0.
#    (See accompanying file LICENSE_1_0.txt or copy at
#          http://www.boost.org/LICENSE_1_0.txt)

include(sugar_add_executable)
include(sugar_add_gtest)
include(sugar_target_link_libraries)

add_subdirectory(uri)
add_subdirectory(http)

if (Boost_FOUND)
    set(
        TESTS
        message_test
        message_transform_test
        utils_thread_pool
        )
    foreach (test ${TESTS})
        if (${CMAKE_CXX_COMPILER_ID} MATCHES GNU)
          set_source_files_properties(${test}.cpp
            PROPERTIES COMPILE_FLAGS "-Wall")
        endif()
        sugar_add_executable(cpp-netlib-${test} ${test}.cpp)
	add_dependencies(cpp-netlib-${test} cppnetlib-uri)
        sugar_target_link_libraries(cpp-netlib-${test} ${Boost_LIBRARIES} ${CMAKE_THREAD_LIBS_INIT} cppnetlib-uri)
        if (OPENSSL_FOUND)
            sugar_target_link_libraries(cpp-netlib-${test} ${OPENSSL_LIBRARIES})
        endif()
	if (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
	  sugar_target_link_libraries(cpp-netlib-${test} rt)
	endif()
        sugar_add_gtest(cpp-netlib-${test} cpp-netlib-${test})
    endforeach (test)

endif()

