cmake_minimum_required(VERSION 2.8)
include(noweb.cmake)
include(UseLatex)
set(libds_C
    main.c)
set(libds_H
    main.h)
set(libds_TEX
    main.tex)

tangle("${libds_H}")
tangle("${libds_C}")
weave("${libds_TEX}")
add_custom_target(tangle DEPENDS ${libds_H} ${libds_C})
add_custom_target(weave DEPENDS ${libds_TEX})

add_library(libds SHARED ${libds_C})
add_dependencies(libds tangle)
target_link_libraries(libds m)

add_latex_document(libds.tex
                   FORCE_PDF
                   TARGET_NAME _libds)
add_dependencies(_libds_pdf weave)
