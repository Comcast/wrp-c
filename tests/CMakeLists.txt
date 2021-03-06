# SPDX-FileCopyrightText: 2010-2021 Comcast Cable Communications Management, LLC
# SPDX-License-Identifier: Apache-2.0

if(NOT DISABLE_VALGRIND)
set (MEMORY_CHECK valgrind --leak-check=full --show-reachable=yes --error-exitcode=1 -v)
endif ()

set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wno-error=missing-field-initializers -W -g -fprofile-arcs -ftest-coverage -O0 -finstrument-functions")
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -W  -g -fprofile-arcs -ftest-coverage -O0 -finstrument-functions")
set (CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -fprofile-arcs -ftest-coverage -O0 -finstrument-functions")

add_test(NAME Simple COMMAND ${MEMORY_CHECK} ./simple)
add_executable(simple simple.c ../src/b64.c
                               ../src/locator.c
                               ../src/msgpack.c
                               ../src/string.c
                               ../src/utils.c
                               ../src/wrp-c.c)
target_link_libraries (simple ${TROWER_LIBRARIES}
                              ${CIMPLOG_LIBRARIES}
                              ${MSGPACK_LIBRARIES}
                              ${CUNIT_LIBRARIES}
                              gcov)

add_test(NAME Test_utils COMMAND ${MEMORY_CHECK} ./test_utils)
add_executable(test_utils test_utils.c ../src/utils.c)
target_link_libraries(test_utils ${CUNIT_LIBRARIES} gcov)

add_test(NAME Test_locator COMMAND ${MEMORY_CHECK} ./test_locator)
add_executable(test_locator test_locator.c ../src/locator.c)
target_link_libraries(test_locator ${CUNIT_LIBRARIES} gcov)

add_test(NAME Test_msgpack COMMAND ${MEMORY_CHECK} ./test_msgpack)
add_executable(test_msgpack test_msgpack.c ../src/b64.c
                                           ../src/locator.c
                                           ../src/msgpack.c
                                           ../src/string.c
                                           ../src/utils.c
                                           ../src/wrp-c.c)
target_link_libraries (test_msgpack ${TROWER_LIBRARIES}
                                    ${CIMPLOG_LIBRARIES}
                                    ${MSGPACK_LIBRARIES}
                                    ${CUNIT_LIBRARIES}
                                    gcov)
