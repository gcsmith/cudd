################################################################################
# cudd_util
################################################################################

set(util_src
    cpu_stats.c
    cpu_time.c
    cstringstream.c
    datalimit.c
    pathsearch.c
    pipefork.c
    prtime.c
    safe_mem.c
    strsav.c
    texpand.c
    ucbqsort.c
)

add_library(cudd_util OBJECT ${util_src})

