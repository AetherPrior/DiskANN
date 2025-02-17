# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT license.

add_library(diskann_dll SHARED dllmain.cpp ../partition_and_pq.cpp ../pq_flash_index.cpp ../logger.cpp ../utils.cpp 
	  ../windows_aligned_file_reader.cpp ../distance.cpp ../memory_mapper.cpp ../index.cpp ../math_utils.cpp ../aux_utils.cpp ../ann_exception.cpp)
if (MSVC)
	add_definitions(-D_USRDLL -D_WINDLL -DDISKANN_DLL)
	add_compile_options(/MD)
	target_include_directories(diskann_dll PRIVATE "${BOOST_ROOT}")
	target_link_options(diskann_dll PRIVATE /DLL /MACHINE:X64 /DEBUG:FULL "/INCLUDE:_tcmalloc")
	target_link_options(diskann_dll PRIVATE $<$<CONFIG:Debug>:/IMPLIB:${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}/diskann_dll.lib>
										$<$<CONFIG:Release>:/IMPLIB:${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}/diskann_dll.lib>
					   )
	target_link_libraries(diskann_dll debug  ${PROJECT_SOURCE_DIR}/gperftools/x64/Release-Patch/libtcmalloc_minimal.lib)
	target_link_libraries(diskann_dll optimized ${PROJECT_SOURCE_DIR}/gperftools/x64/Release-Patch/libtcmalloc_minimal.lib)
					

	add_custom_command(TARGET 
							diskann_dll 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${INTEL_ROOT}redist/intel64/compiler/libiomp5md.dll "$<$<CONFIG:debug>:${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}>$<$<CONFIG:release>:${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}>" )
	add_custom_command(TARGET 
							diskann_dll 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${INTEL_ROOT}redist/intel64/mkl/mkl_avx.dll "$<$<CONFIG:debug>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}\">$<$<CONFIG:release>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}\">" )
	add_custom_command(TARGET 
							diskann_dll 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${INTEL_ROOT}redist/intel64/mkl/mkl_avx2.dll "$<$<CONFIG:debug>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}\">$<$<CONFIG:release>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}\">" )
	add_custom_command(TARGET 
							diskann_dll 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${INTEL_ROOT}redist/intel64/mkl/mkl_avx512.dll "$<$<CONFIG:debug>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}\">$<$<CONFIG:release>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}\">" )
	add_custom_command(TARGET 
							diskann_dll 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${INTEL_ROOT}redist/intel64/mkl/mkl_core.dll "$<$<CONFIG:debug>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}\">$<$<CONFIG:release>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}\">" )
	add_custom_command(TARGET 
							diskann_dll 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${INTEL_ROOT}redist/intel64/mkl/mkl_def.dll "$<$<CONFIG:debug>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}\">$<$<CONFIG:release>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}\">" )
	add_custom_command(TARGET 
							diskann_dll 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${INTEL_ROOT}redist/intel64/mkl/mkl_intel_thread.dll "$<$<CONFIG:debug>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}\">$<$<CONFIG:release>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}\">" )
	add_custom_command(TARGET 
							diskann_dll 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${INTEL_ROOT}redist/intel64/mkl/mkl_rt.dll "$<$<CONFIG:debug>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}\">$<$<CONFIG:release>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}\">" )
	add_custom_command(TARGET 
							diskann_dll 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${PROJECT_SOURCE_DIR}/gperftools/x64/Release-Patch/libtcmalloc_minimal.dll "$<$<CONFIG:debug>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}\">$<$<CONFIG:release>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}\">" )

endif()



