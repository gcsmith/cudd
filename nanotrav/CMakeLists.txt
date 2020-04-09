################################################################################
# nanotrav
################################################################################

set(nanotrav_src
    bnet.c
    chkMterm.c
    main.c
    ntr.c
    ntrBddTest.c
    ntrHeap.c
    ntrMflow.c
    ntrShort.c
    ntrZddTest.c
)

add_executable(nanotrav ${nanotrav_src})
target_link_libraries(nanotrav cudd m)

