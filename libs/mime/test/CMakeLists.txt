include(sugar_add_executable)
include(sugar_add_gtest)
include(sugar_target_link_libraries)
find_package ( Boost 1.41.0 COMPONENTS unit_test_framework )
set ( Boost_USE_STATIC_LIBS ON )
set ( Boost_USE_MULTITHREADED ON )

if ( Boost_FOUND )
    sugar_add_executable ( mime-roundtrip mime-roundtrip.cpp )
    sugar_target_link_libraries ( mime-roundtrip ${Boost_LIBRARIES} )
    sugar_add_gtest ( mime-roundtrip mime-roundtrip )
endif ()

