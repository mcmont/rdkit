find_package(SWIG REQUIRED)
include(${SWIG_USE_FILE})

set(T_LIBS ${Boost_LIBRARIES})
find_package(Boost 1.39.0 COMPONENTS serialization)
if (Boost_SERIALIZATION_LIBRARY)
   # don't add the boost serialization definitions, swig can't compile the
   # headers.  Hopefully it will stil link and run...
  set(Boost_LIBRARIES ${T_LIBS} ${Boost_LIBRARIES})
else()
endif()

INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR})

SET(CMAKE_SWIG_FLAGS "")

if(WIN32)
	set(RDKit_Wrapper_Libs
        FilterCatalog
        Catalogs
        ${Boost_SERIALIZATION_LIBRARY} 
        FMCS MolDraw2D
        FileParsers SmilesParse
	Depictor SubstructMatch ChemReactions Fingerprints ChemTransforms
        Descriptors MolTransforms  PartialCharges 
	Subgraphs GraphMol DataStructs
	DistGeomHelpers DistGeometry ForceFieldHelpers ForceField
	EigenSolvers
	Optimizer
	MolAlign
	Alignment
        SimDivPickers
	RDGeometryLib RDGeneral )
        if(RDK_BUILD_INCHI_SUPPORT)
          set(RDKit_Wrapper_Libs RDInchiLib Inchi ${RDKit_Wrapper_Libs})
        endif(RDK_BUILD_INCHI_SUPPORT)
        if(RDK_BUILD_AVALON_SUPPORT)
          set(RDKit_Wrapper_Libs AvalonLib  avalon_clib ${RDKit_Wrapper_Libs})
        endif(RDK_BUILD_AVALON_SUPPORT)
else()
	set(RDKit_Wrapper_Libs
        Catalogs_static
        FilterCatalog_static
        ${Boost_SERIALIZATION_LIBRARY}
        FMCS_static MolDraw2D_static
        FileParsers_static SmilesParse_static
	Depictor_static SubstructMatch_static ChemReactions_static Fingerprints_static ChemTransforms_static
	Subgraphs_static GraphMol_static DataStructs_static Descriptors_static PartialCharges_static MolTransforms_static
	DistGeomHelpers_static DistGeometry_static ForceFieldHelpers_static ForceField_static
	EigenSolvers_static
	Optimizer_static
	MolAlign_static
	Alignment_static
        SimDivPickers_static
	RDGeometryLib_static RDGeneral_static )
        if(RDK_BUILD_INCHI_SUPPORT)
          set(RDKit_Wrapper_Libs RDInchiLib_static Inchi_static ${RDKit_Wrapper_Libs})
        endif(RDK_BUILD_INCHI_SUPPORT)
        if(RDK_BUILD_AVALON_SUPPORT)
          set(RDKit_Wrapper_Libs AvalonLib_static  avalon_clib_static ${RDKit_Wrapper_Libs})
        endif(RDK_BUILD_AVALON_SUPPORT)
endif()

if(RDK_BUILD_SWIG_JAVA_WRAPPER)
add_subdirectory(gmwrapper)
endif()

if(RDK_BUILD_SWIG_CSHARP_WRAPPER)
add_subdirectory(csharp_wrapper)
endif()
