# SPDX-FileCopyrightText: 2006-2023, Knut Reinert & Freie Universität Berlin
# SPDX-FileCopyrightText: 2016-2023, Knut Reinert & MPI für molekulare Genetik
# SPDX-License-Identifier: CC0-1.0
cmake_minimum_required (VERSION 3.14)

set(CPM_CHECK_VERSION_PATH ${CMAKE_CURRENT_SOURCE_DIR} CACHE INTERNAL "CPM_CHECK_VERSION_PATH")
function(cpm_check_version)
    set(ignore_packages ${ARGN})
    message("ignoring packages:  ${ignore_packages}")

    option(CPM_CHECK_VERSION_ENABLED "Enables CPM_CHECK_VERSION as target for test." OFF)
    if (CPM_CHECK_VERSION_ENABLED)
        add_custom_target(CPM_CHECK_NEWER_PACKAGES)
        foreach(package ${CPM_PACKAGES})
            message("Is ${package} in ${ignore_packages}?")
            if ("${package}" IN_LIST ignore_packages)
                message("yes")
                continue()
            endif()
            message("no")
            add_custom_target(CPM_CHECK_NEWER_PACKAGES_${package}
                COMMAND ${CPM_CHECK_VERSION_PATH}/isNewestVersion.sh
                              ${package}
                              ${CPM_PACKAGE_${package}_SOURCE_DIR}
                              ${CPM_PACKAGE_${package}_VERSION}
            )
            add_dependencies(CPM_CHECK_NEWER_PACKAGES CPM_CHECK_NEWER_PACKAGES_${package})
        endforeach()
        add_test(NAME cpm_check_newer_packages_test
            COMMAND
            "${CMAKE_COMMAND}" --build "${CMAKE_BINARY_DIR}"
            --config "$<CONFIG>"
            --target CPM_CHECK_NEWER_PACKAGES
        )
    endif()
endfunction()
