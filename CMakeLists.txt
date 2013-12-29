cmake_minimum_required(VERSION 2.6)
project(leveldb)


################################
# Include Paths
################################
include_directories("include")
include_directories("../leveldb")

set(SRC
  db/builder.cc
  db/c.cc
  db/db_impl.cc
  db/db_iter.cc
  db/dbformat.cc
  db/filename.cc
  db/log_reader.cc
  db/log_writer.cc
  db/memtable.cc
  db/repair.cc
  db/table_cache.cc
  db/version_edit.cc
  db/version_set.cc
  db/write_batch.cc
  table/block.cc
  table/block_builder.cc
  table/filter_block.cc
  table/format.cc
  table/iterator.cc
  table/merger.cc
  table/table.cc
  table/table_builder.cc
  table/two_level_iterator.cc
  util/arena.cc
  util/bloom.cc
  util/cache.cc
  util/coding.cc
  util/comparator.cc
  util/crc32c.cc
  util/env.cc
  util/filter_policy.cc
  util/hash.cc
  util/histogram.cc
  util/logging.cc
  util/options.cc
  util/status.cc
)


################################
# Port Sources
################################
if (MINGW OR MSVC)
  set(PORT
    port/port_win.cc
    util/env_win.cc
  )
else()
  set(PORT
    port/port_posix.cc
    util/env_posix.cc
  )
endif()


################################
# Static library
################################
add_library(leveldb STATIC ${PORT} ${SRC})

if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
  set(_def OS_MACOSX LEVELDB_PLATFORM_POSIX)
  set_target_properties(leveldb PROPERTIES COMPILE_DEFINITIONS "${_def}")
elseif(MSVC)
  target_link_libraries(leveldb shlwapi)
  set(_def
    LEVELDB_PLATFORM_WINDOWS
    OS_WINDOWS
    ssize_t=long
  )
  set_target_properties(leveldb PROPERTIES COMPILE_DEFINITIONS "${_def}")
elseif(MINGW)
  set(_def
    _WIN32_WINNT=0x0501
    _WIN32_IE=0x0501
    WINVER=0x0501
    LEVELDB_PLATFORM_WINDOWS
    _REENTRANT
    OS_WINDOWS
    __USE_MINGW_ANSI_STDIO=1
  )
  set_target_properties(leveldb PROPERTIES COMPILE_DEFINITIONS "${_def}")
  set_target_properties(leveldb PROPERTIES COMPILE_FLAGS "-std=gnu++0x")
  target_link_libraries(leveldb shlwapi)
else()
  set(_def LEVELDB_PLATFORM_POSIX)
  set_target_properties(leveldb PROPERTIES COMPILE_DEFINITIONS "${_def}")
  set_target_properties(leveldb PROPERTIES COMPILE_FLAGS "-std=c++11")
endif()

################################
# Tests
################################
set(TEST_SRC
  util/testutil.cc
  util/testharness.cc
)

add_executable(arena_test ${TEST_SRC} util/arena_test.cc)

if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
  set(_def OS_MACOSX LEVELDB_PLATFORM_POSIX)
  set_target_properties(arena_test PROPERTIES COMPILE_DEFINITIONS "${_def}")
elseif(MINGW)
  set(_def
    _WIN32_WINNT=0x0501
    _WIN32_IE=0x0501
    WINVER=0x0501
    LEVELDB_PLATFORM_WINDOWS
    _REENTRANT
    OS_WINDOWS
    __USE_MINGW_ANSI_STDIO=1
  )
  set_target_properties(arena_test PROPERTIES COMPILE_DEFINITIONS "${_def}")
  set_target_properties(arena_test PROPERTIES COMPILE_FLAGS "-std=gnu++0x")
else()
  set(_def LEVELDB_PLATFORM_POSIX)
  set_target_properties(arena_test PROPERTIES COMPILE_DEFINITIONS "${_def}")
  set_target_properties(arena_test PROPERTIES COMPILE_FLAGS "-std=c++11")
  target_link_libraries(arena_test leveldb pthread)
endif()

add_executable(bloom_test ${TEST_SRC} util/bloom_test.cc)

if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
  set(_def OS_MACOSX LEVELDB_PLATFORM_POSIX)
  set_target_properties(bloom_test PROPERTIES COMPILE_DEFINITIONS "${_def}")
elseif(MINGW)
  set(_def
    _WIN32_WINNT=0x0501
    _WIN32_IE=0x0501
    WINVER=0x0501
    LEVELDB_PLATFORM_WINDOWS
    _REENTRANT
    OS_WINDOWS
    __USE_MINGW_ANSI_STDIO=1
  )
  set_target_properties(bloom_test PROPERTIES COMPILE_DEFINITIONS "${_def}")
  set_target_properties(bloom_test PROPERTIES COMPILE_FLAGS "-std=gnu++0x")
else()
  set(_def LEVELDB_PLATFORM_POSIX)
  set_target_properties(bloom_test PROPERTIES COMPILE_DEFINITIONS "${_def}")
  set_target_properties(bloom_test PROPERTIES COMPILE_FLAGS "-std=c++11")
  target_link_libraries(bloom_test leveldb pthread)
endif()

add_executable(cache_test ${TEST_SRC} util/cache_test.cc)

if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
  set(_def OS_MACOSX LEVELDB_PLATFORM_POSIX)
  set_target_properties(cache_test PROPERTIES COMPILE_DEFINITIONS "${_def}")
elseif(MINGW)
  set(_def
    _WIN32_WINNT=0x0501
    _WIN32_IE=0x0501
    WINVER=0x0501
    LEVELDB_PLATFORM_WINDOWS
    _REENTRANT
    OS_WINDOWS
    __USE_MINGW_ANSI_STDIO=1
  )
  set_target_properties(cache_test PROPERTIES COMPILE_DEFINITIONS "${_def}")
  set_target_properties(cache_test PROPERTIES COMPILE_FLAGS "-std=gnu++0x")
else()
  set(_def LEVELDB_PLATFORM_POSIX)
  set_target_properties(cache_test PROPERTIES COMPILE_DEFINITIONS "${_def}")
  set_target_properties(cache_test PROPERTIES COMPILE_FLAGS "-std=c++11")
  target_link_libraries(cache_test leveldb pthread)
endif()

add_executable(env_test ${TEST_SRC} util/env_test.cc)

if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
  set(_def OS_MACOSX LEVELDB_PLATFORM_POSIX)
  set_target_properties(env_test PROPERTIES COMPILE_DEFINITIONS "${_def}")
elseif(MINGW)
  set(_def
    _WIN32_WINNT=0x0501
    _WIN32_IE=0x0501
    WINVER=0x0501
    LEVELDB_PLATFORM_WINDOWS
    _REENTRANT
    OS_WINDOWS
    __USE_MINGW_ANSI_STDIO=1
  )
  set_target_properties(env_test PROPERTIES COMPILE_DEFINITIONS "${_def}")
  set_target_properties(env_test PROPERTIES COMPILE_FLAGS "-std=gnu++0x")
else()
  set(_def LEVELDB_PLATFORM_POSIX)
  set_target_properties(env_test PROPERTIES COMPILE_DEFINITIONS "${_def}")
  set_target_properties(env_test PROPERTIES COMPILE_FLAGS "-std=c++11")
  target_link_libraries(env_test leveldb pthread)
endif()
