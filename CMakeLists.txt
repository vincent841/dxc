# 요구 CMake 최소 버전
cmake_minimum_required( VERSION 3.0 )
 
# 프로젝트 이름 및 버전
project( "dxc" )
 
# Build configuration
set( CMAKE_BUILD_TYPE Debug )
set( CMAKE_VERBOSE_MAKEFILE true )

# Build custom configuration
set( DX_BUILD_ARM ON)


#################################################3
# Build Target & source file list
set( SRC_LIBRARY
        "dxc"
        )

set( TEST_EXEC
        "dxc_test"
        )

set( DEMO_EXEC
        "dxc_demo"
        )        

set( SRC_FILES
        src/dx_camera.c
        src/dx_camera.c           
        src/dx_net_client.c                 
        src/dx_util_buffer.c
        src/dx_console.c          
        src/dx_net_dgram.c                  
        src/dx_util_clock.c
        src/dx_debug_malloc.c     
        src/dx_net_discovery.c              
        src/dx_util_file.c
        src/dx_event_control.c    
        src/dx_net_packet.c                 
        src/dx_util_list.c
        src/dx_event_mplexer.c    
        src/dx_net_packet_camera_preview.c  
        src/dx_util_lock.c
        src/dx_event_pipe.c       
        src/dx_net_packet_file.c            
        src/dx_util_log.c
        src/dx_file_avi.c         
        src/dx_net_packet_io.c              
        src/dx_util_schedule.c
        src/dx_file_avi_parser.c  
        src/dx_net_packet_movie_playback.c  
        src/dx_v4l2.c
        src/dx_file_movie.c       
        src/dx_net_server.c        
        )

set( TEST_FILES
        test/console_test.c        
        test/net_dgram_loopback_test.c     
        test/util_buffer_test.c
        test/debug_malloc_test.c   
        test/net_discovery_test.c          
        test/util_clock_test.c
        test/event_control_test.c  
        test/net_packet_file_test.c        
        test/util_list_test.c
        test/event_mplexer_test.c  
        test/net_pipe_test.c               
        test/util_lock_test.c
        test/file_avi_test.c       
        test/net_server_connection_test.c  
        test/util_schedule_test.c
        test/main.c                
        test/net_server_test.c
        )

set( DEMO_FILES
        demo/demo_console_menu_camera.c              
        demo/omnid_drive_handler_movie.c
        demo/demo_console_menu_client.c              
        demo/omnid_drive_handler_set_setting.c
        demo/demo_console_menu_movie.c               
        demo/omnid_drive_handler_set_state.c
        demo/demo_console_menu_server.c              
        demo/demo_console_menu_start.c               
        demo/omnid_host.c
        demo/demo_console_menu_video_file.c          
        demo/omnid_host_handler.c
        demo/omnid_host_handler_command.c
        demo/main.c                                  
        demo/omnid_host_handler_event.c
        demo/omnid_drive.c                           
        demo/omnid_host_handler_file.c
        demo/omnid_drive_handler.c                   
        demo/omnid_host_handler_get_setting.c
        demo/omnid_drive_handler_command.c              
        demo/omnid_host_handler_get_state.c
        demo/omnid_drive_handler_command_playback.c  
        demo/omnid_host_handler_hb.c
        demo/omnid_drive_handler_event.c             
        demo/omnid_host_handler_movie.c
        demo/omnid_drive_handler_file.c              
        demo/omnid_host_handler_set_setting.c
        demo/omnid_drive_handler_get_setting.c       
        demo/omnid_host_handler_set_state.c
        demo/omnid_drive_handler_get_state.c         
        demo/omnid_host_handler_stream.c
        demo/omnid_drive_handler_hb.c
        )        
 
# toolchain selections
if(DX_BUILD_ARM)
        set( CMAKE_C_COMPILER "arm-linux-gnueabihf-gcc" )
        set( CMAKE_CXX_COMPILER "arm-linux-gnueabihf-g++" )
        set(CMAKE_LINKER "arm-linux-gnueabihf-ld")
        set(CMAKE_NM "arm-linux-gnueabihf-nm")
        set(CMAKE_OBJCOPY "arm-linux-gnueabihf-objcopy")
        set(CMAKE_OBJDUMP "arm-linux-gnueabihf-objdump")
        set(CMAKE_RANLIB "arm-linux-gnueabihf-gcc-ranlib")
else()        
        set( CMAKE_C_COMPILER "gcc" )
        set( CMAKE_CXX_COMPILER "g++" )
        set(CMAKE_NM "nm")
        set(CMAKE_OBJCOPY "objcopy")
        set(CMAKE_OBJDUMP "objdump")
        set(CMAKE_RANLIB "ranlib")        
endif()

 
# include directories
include_directories( include )
 
# common compile option
add_compile_options( -g -Wall )
set( CMAKE_EXE_LINKER_FLAGS "-static -Wl,--gc-sections" )

# compile option for libraries
link_libraries( pthread )
 
# add a library
add_library( ${SRC_LIBRARY} STATIC ${SRC_FILES} )

# library directories
link_directories( /usr/lib )
 
# debug options
set( CMAKE_C_FLAGS_DEBUG "-DDEBUG -DC_FLAGS" )
set( CMAKE_EXE_LINKER_FLAGS_DEBUG "-DDEBUG -DLINKER_FLAGS" )
 
# release options
set( CMAKE_C_FLAGS_RELEASE "-DRELEASE -DC_FLAGS" )
set( CMAKE_EXE_LINKER_FLAGS_RELEASE "-DRELEASE -DLINKER_FLAGS" )
 
# output directories
set( CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BUILD_TYPE} )
set( CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BUILD_TYPE}/lib )
set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BUILD_TYPE}/lib )


# add executables
link_libraries( pthread dxc )

add_executable( ${TEST_EXEC} ${TEST_FILES} )
add_dependencies( ${TEST_EXEC} ${SRC_LIBRARY} )

add_executable( ${DEMO_EXEC} ${DEMO_FILES} )
add_dependencies( ${DEMO_EXEC} ${SRC_LIBRARY} )
