cmake_minimum_required(VERSION 3.10)
project(leveldb)


################################
# Include Paths
################################
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
  set_target_properties(leveldb PROPERTIES COMPILE_FLAGS "-std=c++11")
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
elseif(ANDROID)
  set(_def
    LEVELDB_PLATFORM_POSIX
    _REENTRANT
    OS_ANDROID
  )
  set_target_properties(leveldb PROPERTIES COMPILE_DEFINITIONS "${_def}")
  set_target_properties(leveldb PROPERTIES COMPILE_FLAGS "-std=c++11")
else()
  set(_def LEVELDB_PLATFORM_POSIX)
  set_target_properties(leveldb PROPERTIES COMPILE_DEFINITIONS "${_def}")
  set_target_properties(leveldb PROPERTIES COMPILE_FLAGS "-std=c++11")
endif()

target_include_directories(leveldb PUBLIC "include")
target_include_directories(leveldb PUBLIC "../leveldb")