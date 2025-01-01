include(GetFTXUI)
add_library(calc_console_static STATIC tui.cpp)
target_include_directories(calc_console_static PUBLIC include)
target_precompile_headers(calc_console_static PUBLIC <string>)
target_link_libraries(calc_console_static PUBLIC calc_shared
                    ftxui::screen ftxui::dom ftxui::component)

target_compile_options(calc_console_static PRIVATE -Wno-uninitialized)

get_target_property(my_foo_original ftxui::screen ALIASED_TARGET)
message(STATUS "Alias ftxui::screen refers to the target ${my_foo_original}")
target_compile_options(screen PRIVATE -Wno-error -Wno-deprecated-declarations -Wno-uninitialized)

include(BuildInfo)
BuildInfo(calc_console_static)
include(Coverage)
CleanCoverage(calc_console_static)
include(Format)
Format(calc_console_static .)
include(CppCheck)
AddCppCheck(calc_console_static)
include(Doxygen)
Doxygen(calc_console src/calc_console)

add_executable(calc_console bootstrap.cpp)
target_link_libraries(calc_console calc_console_static)
