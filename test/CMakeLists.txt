include(${PROJECT_SOURCE_DIR}/cmake/create_test.cmake)

include(CTest)

include(ExternalProject)
find_package(Git REQUIRED)

ExternalProject_Add(
  doctest
  PREFIX ${CMAKE_BINARY_DIR}/doctest
  GIT_REPOSITORY https://github.com/doctest/doctest.git
  TIMEOUT 10
  UPDATE_COMMAND ${GIT_EXECUTABLE} pull
  CONFIGURE_COMMAND ""
  BUILD_COMMAND ""
  INSTALL_COMMAND ""
  LOG_DOWNLOAD ON)

# Expose required variable (DOCTEST_INCLUDE_DIR) to parent scope
ExternalProject_Get_Property(doctest source_dir)
set(DOCTEST_INCLUDE_DIR
    ${source_dir}/doctest
    CACHE INTERNAL "Path to include folder for doctest")


create_test("tokenizer" FILES "Tokenizer.test.cpp" LIBS ast_lib INCLUDE ${DOCTEST_INCLUDE_DIR})
create_test("parser" FILES "Parser.test.cpp" LIBS ast_lib INCLUDE ${DOCTEST_INCLUDE_DIR})
