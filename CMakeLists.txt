cmake_minimum_required(VERSION 3.8)

if(1)
	get_filename_component(CURRENT_DIR_NAME ${CMAKE_CURRENT_SOURCE_DIR} NAME)
	set(ProjectName "${CURRENT_DIR_NAME}")

	include($ENV{cpp_lib_build_scripts_path}/cmake-module/setup.cmake)

	project(${ProjectName})
	enable_language(ASM)
	set(CMAKE_CXX_STANDARD 20)
	set(CMAKE_C_STANDARD 17)
endif()


# include(target_import_boost)
# include(target_import_nlohmann_json)

# 编译目标
add_library(${ProjectName})
target_import_src(${ProjectName})
# target_import_boost(${ProjectName} PUBLIC)
# target_import_nlohmann_json(${ProjectName} PUBLIC)

if(platform STREQUAL "msys")
	set(exe_name "lua")
	add_executable(${exe_name} ${CMAKE_CURRENT_SOURCE_DIR}/exe/lua.c)

	target_link_libraries(${ProjectName} PUBLIC -Wl,--start-group)
	target_link_libraries(${exe_name} ${ProjectName})
	target_link_libraries(${ProjectName} PUBLIC -Wl,--end-group)

	install(TARGETS ${exe_name}
			RUNTIME DESTINATION bin
			LIBRARY DESTINATION lib
			ARCHIVE DESTINATION lib)
endif()

if(platform STREQUAL "msys")
	set(exe_name "luac")
	add_executable(${exe_name} ${CMAKE_CURRENT_SOURCE_DIR}/exe/luac.c)

	target_link_libraries(${ProjectName} PUBLIC -Wl,--start-group)
	target_link_libraries(${exe_name} ${ProjectName})
	target_link_libraries(${ProjectName} PUBLIC -Wl,--end-group)

	target_install(${exe_name})
endif()
