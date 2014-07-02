set(TRACE_ALL 0 CACHE BOOLEAN "Enable tracing implemented functions")
if(${TRACE_ALL})
    add_definitions(-DTRACE_ALL)
endif()

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})

add_library(freshwrapper-obj OBJECT
    async_network.c
    config.c
    header_parser.c
    keycodeconvert.c
    np_entry.c
    np_functions.c
    reverse_constant.c
    tables.c
    trace.c
    n2p_proxy_class.c
    p2n_proxy_class.c
    pp_interface.c
    pp_resource.c
    ppb_core.c
    ppb_audio_config.c
    ppb_audio_input_dev.c
    ppb_audio.c
    ppb_browser_font_trusted.c
    ppb_buffer_dev.c
    ppb_char_set_dev.c
    ppb_crypto_dev.c
    ppb_cursor_control_dev.c
    ppb_file_chooser_dev.c
    ppb_file_chooser_trusted.c
    ppb_file_ref.c
    ppb_flash_clipboard.c
    ppb_flash_file.c
    ppb_flash_file_modulelocal.c
    ppb_flash_font_file.c
    ppb_flash_fullscreen.c
    ppb_flash.c
    ppb_flash_menu.c
    ppb_flash_message_loop.c
    ppb_font_dev.c
    ppb_graphics2d.c
    ppb_graphics3d.c
    ppb_image_data.c
    ppb_ime_input_event_dev.c
    ppb_input_event.c
    ppb_instance.c
    ppb_instance_private.c
    ppb_memory_dev.c
    ppb_message_loop.c
    ppb_nacl_private.c
    ppb_net_address_private.c
    ppb_network_monitor.c
    ppb_opengles2.c
    ppb_printing_dev.c
    ppb_tcp_socket_private.c
    ppb_text_input_dev.c
    ppb_udp_socket_private.c
    ppb_url_loader.c
    ppb_url_loader_trusted.c
    ppb_url_request_info.c
    ppb_url_response_info.c
    ppb_url_util_dev.c
    ppb_var.c
    ppb_video_capture_dev.c
    ppb_view.c
)

add_library(freshwrapper-pepperflash SHARED
    $<TARGET_OBJECTS:freshwrapper-obj>
    $<TARGET_OBJECTS:parson-obj>
    config_pepperflash.c
)

add_library(freshwrapper-nacl SHARED
    $<TARGET_OBJECTS:freshwrapper-obj>
    $<TARGET_OBJECTS:parson-obj>
    config_nacl.c
)

target_link_libraries(freshwrapper-pepperflash
    dl
    ${REQ_LIBRARIES}
)

target_link_libraries(freshwrapper-nacl
    dl
    ${REQ_LIBRARIES}
)

# ensure libfreshwrapper is linked with all libraries it uses
add_executable(dep_check dep_check.c)
link_directories(${CMAKE_CURRENT_BINARY_DIR})
target_link_libraries(dep_check freshwrapper-pepperflash)