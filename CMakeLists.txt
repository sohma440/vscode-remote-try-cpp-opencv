project(TestProjectWithOpenCV)

find_package(OpenCV REQUIRED)

add_executable(test main.cpp)

target_include_directories(test PRIVATE ${OpenCV_INCLUDE_DIRS})

target_link_libraries(test ${OpenCV_LIBS})
