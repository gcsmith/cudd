################################################################################
# cudd_mtr
################################################################################

set(mtr_src
    mtrBasic.c
    mtrGroup.c
)

add_library(cudd_mtr OBJECT ${mtr_src})

