RDKIT=$(RDBASE)

SHLIB_LINK += -L${RDKIT}/bin -Wl,-rpath,'${RDKIT}/bin' -lGraphMol -lSmilesParse  -lFingerprints -lSubstructMatch -lDescriptors -lSubgraphs -lDataStructs -lRDGeometryLib -lRDGeneral 
PG_CPPFLAGS = -I$(BOOSTHOME) -I${RDKIT}/Code -DRDKITVER='"001910"' 

MODULE_big = rdkit
OBJS = rdkit_io.o mol_op.o bfp_op.o sfp_op.o rdkit_gist.o low_gist.o guc.o cache.o adapter.o 
PG_CONFIG = pg_config
PGXS := $(shell $(PG_CONFIG) --pgxs)

DATA_built = rdkit.sql
DATA = uninstall_rdkit.sql
REGRESS = rdkit props btree molgist bfpgist sfpgist slfpgist fps 

include $(PGXS)

CPLUSPLUSFLAGS = $(filter-out -Wmissing-prototypes -Wdeclaration-after-statement, $(CFLAGS))
CPLUSPLUSFLAGS += -Wno-deprecated -Wno-unused-function
CPLUSPLUSFLAGS += $(PG_CPPFLAGS)

%.o : %.cpp
	$(CC) $(CPLUSPLUSFLAGS) $(CPPFLAGS)  -c -o $@ $< 