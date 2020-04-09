################################################################################
# cudd_dddmp
################################################################################

set(dddmp_src
    dddmpBinary.c
    dddmpConvert.c
    dddmpDbg.c
    dddmpLoad.c
    dddmpLoadCnf.c
    dddmpNodeAdd.c
    dddmpNodeBdd.c
    dddmpNodeCnf.c
    dddmpStoreAdd.c
    dddmpStoreBdd.c
    dddmpStoreCnf.c
    dddmpStoreMisc.c
    dddmpUtil.c
)

add_library(cudd_dddmp OBJECT ${dddmp_src})

