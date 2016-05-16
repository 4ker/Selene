project( Selene )
cmake_minimum_required( VERSION 2.8 )

set( LUA_LIBRARIES lua )
add_subdirectory( lua )
set( LUA_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/lua ) # lua5.3
include_directories( ${LUA_INCLUDE_DIR} )

# c++ 11 support
include( CheckCXXCompilerFlag )
check_cxx_compiler_flag( "-std=c++11"   COMPILER_SUPPORTS_CXX11 )
check_cxx_compiler_flag( "-std=c++0x"   COMPILER_SUPPORTS_CXX0X )
if( COMPILER_SUPPORTS_CXX11 )
    if( CMAKE_COMPILER_IS_GNUCXX )
        set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11" )
    else()
        set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11" )
    endif()
elseif( COMPILER_SUPPORTS_CXX0X)
    set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x" )
else()
    # MSVC, On by default (if available)
endif()

# large address aware option setting
if( ${MINGW} )
	set( LARGEADDRESSAWARE "--large-address-aware" )
else( ${MINGW} )
	if ( WIN32 )
		set( LARGEADDRESSAWARE "/LARGEADDRESSAWARE" )
	else ( WIN32 )
		set( LARGEADDRESSAWARE " " )
	endif ( WIN32 )
endif( ${MINGW} )

if ( WIN32)
	SET( APP_OPT "WIN32" )
else ( WIN32 )
	if ( APPLE )
		#SET( APP_OPT "MACOSX_BUNDLE" )
		SET( APP_OPT "EXCLUDE_FROM_ALL" )
	else ( APPLE )
		SET( APP_OPT " " )
	endif ( APPLE )
endif ( WIN32 )

if( WIN32 )
    set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /bigobj" )
endif( WIN32 )

include_directories( include )
file( GLOB headers RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
    include/*.h include/selene/*.h )

add_executable( test_runner ${CMAKE_CURRENT_SOURCE_DIR}/test/Test.cpp )
target_link_libraries( test_runner ${LUA_LIBRARIES} )
