#-------------------------------------------------------------------------------
# Collect source
#-------------------------------------------------------------------------------

file(GLOB_RECURSE CPP_FILES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} *.cc)
file(GLOB_RECURSE HPP_FILES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} *.h)

if(NOT USE_UNIT_TEST)
	foreach(f ${CPP_FILES})
		string(FIND "${f}" "_unittest" _indx)
		if (NOT ${_indx} STREQUAL "-1")
			list(REMOVE_ITEM CPP_FILES "${f}")
		endif()
	endforeach()
endif()

#AH_ERROR(CPP_FILES)

#-------------------------------------------------------------------------------
# Build
#-------------------------------------------------------------------------------

add_library(base SHARED 
	${CPP_FILES} 
	${HPP_FILES}
	)
   
#-------------------------------------------------------------------------------
# Install
#-------------------------------------------------------------------------------

AH_INSTALL_BINARY_TARGET(base "Runtime")

