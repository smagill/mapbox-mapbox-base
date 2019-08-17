function(mapbox_base_add_library name include_path)
    if(TARGET mapbox-base-${name})
        return()
    endif()

    execute_process(
        COMMAND git submodule update --init ${name}
        WORKING_DIRECTORY ${CMAKE_CURRENT_LIST_DIR}
    )

    add_library(mapbox-base-${name} INTERFACE)
    add_library(Mapbox::Base::${name} ALIAS mapbox-base-${name})

    target_include_directories(mapbox-base-${name} SYSTEM INTERFACE
        ${include_path}
    )

    target_link_libraries(mapbox-base INTERFACE mapbox-base-${name})
endfunction()

add_library(mapbox-base INTERFACE)
add_library(Mapbox::Base ALIAS mapbox-base)

mapbox_base_add_library(geometry.hpp ${CMAKE_CURRENT_LIST_DIR}/geometry.hpp/include)
mapbox_base_add_library(optional ${CMAKE_CURRENT_LIST_DIR}/optional)
mapbox_base_add_library(pixelmatch-cpp ${CMAKE_CURRENT_LIST_DIR}/pixelmatch-cpp/include)
mapbox_base_add_library(variant ${CMAKE_CURRENT_LIST_DIR}/variant/include)