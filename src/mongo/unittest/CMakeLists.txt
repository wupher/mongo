add_library(unittest STATIC
  unittest
  )
add_dependencies(unittest generate_error_codes generate_action_types)
target_link_libraries(unittest LINK_PUBLIC
  foundation
  )

add_library(unittest_main STATIC
  unittest_main
  )
add_dependencies(unittest_main generate_error_codes generate_action_types)
target_link_libraries(unittest_main LINK_PUBLIC
  unittest
  base
  )

add_library(unittest_crutch STATIC
  crutch
  )
add_dependencies(unittest_crutch generate_error_codes generate_action_types)

if (BUILD_TESTING)
  foreach (test
      unittest_test
      fixture_test
      )
    add_executable(${test} ${test})
    add_dependencies(${test} generate_error_codes generate_action_types)
    link_recursive_deps(${test}
      COMBINED_LIBNAME basic_unittest_deps
      unittest_main
      unittest_crutch
      ${TOKUMX_SSL_LIBRARIES}
      )
  endforeach ()
endif ()
