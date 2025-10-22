cmake_minimum_required(VERSION 3.11)

include(FetchContent)

FetchContent_Declare(
    GSL
    GIT_REPOSITORY https://github.com/Microsoft/GSL.git
    GIT_TAG v4.0.0)

FetchContent_Declare(
    googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG v1.14.0)

project(arcana.cpp)

FetchContent_MakeAvailable(GSL)
FetchContent_MakeAvailable(googletest)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Top-level folder
set(SOURCES
    "Source/Shared/arcana/algorithm.h"
    "Source/Shared/arcana/expected.h"
    "Source/Shared/arcana/finally_scope.h"
    "Source/Shared/arcana/iterators.h"
    "Source/Shared/arcana/macros.h"
    "Source/Shared/arcana/math.h"
    "Source/Shared/arcana/sentry.h"
    "Source/Shared/arcana/string.h"
    "Source/Shared/arcana/type_traits.h")

# containers
set(SOURCES ${SOURCES}
    "Source/Shared/arcana/containers/sorted_vector.h"
    "Source/Shared/arcana/containers/ticketed_collection.h"
    "Source/Shared/arcana/containers/unique_vector.h"
    "Source/Shared/arcana/containers/unordered_bimap.h"
    "Source/Shared/arcana/containers/weak_table.h")

# experimental
set(SOURCES ${SOURCES}
    "Source/Shared/arcana/experimental/array.h")

# functional
set(SOURCES ${SOURCES}
    "Source/Shared/arcana/functional/inplace_function.h")

# messaging
set(SOURCES ${SOURCES}
    "Source/Shared/arcana/messaging/mediator.h"
    "Source/Shared/arcana/messaging/router.h")

# scheduling
set(SOURCES ${SOURCES}
    "Source/Shared/arcana/scheduling/state_machine.h"
    "Source/Shared/arcana/scheduling/state_machine_state.h")

# threading
set(SOURCES ${SOURCES}
    "Source/Shared/arcana/threading/affinity.h"
    "Source/Shared/arcana/threading/blocking_concurrent_queue.h"
    "Source/Shared/arcana/threading/cancellation.h"
    "Source/Shared/arcana/threading/coroutine.h"
    "Source/Shared/arcana/threading/dispatcher.h"
    "Source/Shared/arcana/threading/pending_task_scope.h"
    "Source/Shared/arcana/threading/task.h")

# threading/internal
set(SOURCES ${SOURCES}
    "Source/Shared/arcana/threading/internal/callable_traits.h"
    "Source/Shared/arcana/threading/internal/internal_task.h")

if(WIN32)
    set(SOURCES ${SOURCES}
        "Source/Windows/arcana/hresult.h"
        "Source/Windows/arcana/hresult.cpp"
        "Source/Windows/arcana/threading/set_thread_name.h"
        "Source/Windows/arcana/threading/set_thread_name.cpp"
        "Source/Windows/arcana/threading/task_conversions.h"
        "Source/Windows/arcana/threading/task_schedulers.h"
        "Source/Windows/arcana/threading/task_schedulers.cpp")

    add_library(arcana ${SOURCES})

    target_include_directories(arcana PUBLIC "Source/Shared")
    target_include_directories(arcana PUBLIC "Source/Windows")

    set_target_properties(arcana PROPERTIES LINKER_LANGUAGE CXX)

    target_compile_definitions(arcana PRIVATE NOMINMAX)
    target_compile_definitions(arcana PRIVATE _SILENCE_CXX17_CODECVT_HEADER_DEPRECATION_WARNING)
else()
    add_library(arcana INTERFACE)

    target_include_directories(arcana INTERFACE "Source/Shared")

    if(ANDROID)
        target_include_directories(arcana INTERFACE "Source/Android")
    elseif(APPLE)
        target_include_directories(arcana INTERFACE "Source/Apple")
    elseif(UNIX)
        target_include_directories(arcana INTERFACE "Source/Unix")
    endif()
endif()

target_include_directories(arcana PUBLIC "${gsl_SOURCE_DIR}/include")

source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR} FILES ${SOURCES})

# Tests
if(WIN32)
    # Shared tests
    set(SHARED_TEST_SOURCES
        "Source/Shared.Test/AlgorithmUnitTest.cpp"
        "Source/Shared.Test/ExpectedUnitTest.cpp"
        "Source/Shared.Test/InplaceFunctionUnitTest.cpp"
        "Source/Shared.Test/IteratorUnitTest.cpp"
        "Source/Shared.Test/TypeTraitsTest.cpp"
        "Source/Shared.Test/Containers/ContainerUnitTest.cpp"
        "Source/Shared.Test/Experimental/ArrayUnitTest.cpp"
        "Source/Shared.Test/Messaging/MediatorUnitTest.cpp"
        "Source/Shared.Test/Scheduling/SchedulingUnitTest.cpp"
        "Source/Shared.Test/Threading/CoroutineTests.cpp"
        "Source/Shared.Test/Threading/DispatcherUnitTest.cpp"
        "Source/Shared.Test/Threading/TaskUnitTest.cpp")

    # Windows-specific tests
    set(WINDOWS_TEST_SOURCES
        "Source/Windows.Test/HresultTest.cpp"
        "Source/Windows.Test/Threading/TaskConversionsTests.cpp"
        "Source/Windows.Test/Threading/TaskSchedulersTests.cpp")

    # Create shared test executable
    add_executable(arcana_shared_tests ${SHARED_TEST_SOURCES})
    target_link_libraries(arcana_shared_tests PRIVATE arcana gtest_main gtest)
    target_include_directories(arcana_shared_tests PRIVATE "Source/Shared")

    # Create Windows test executable
    add_executable(arcana_windows_tests ${WINDOWS_TEST_SOURCES})
    target_link_libraries(arcana_windows_tests PRIVATE arcana gtest_main gtest)
    target_include_directories(arcana_windows_tests PRIVATE "Source/Shared" "Source/Windows")

    # Enable testing
    enable_testing()
    include(GoogleTest)
    
    gtest_discover_tests(arcana_shared_tests)
    gtest_discover_tests(arcana_windows_tests)
endif()
