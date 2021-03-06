set(TARGET_NAME IMPORT_EXPORT_TEST)

set(TEST_SCRIPT_FILES
    testUsdExportAsClip.py
    testUsdExportCamera.py
    testUsdExportColorSets.py
    testUsdExportConnected.py
    testUsdExportDisplayColor.py
    testUsdExportEulerFilter.py
    testUsdExportFileFormat.py
    testUsdExportFilterTypes.py
    testUsdExportFrameOffset.py
    testUsdExportInstances.py
    testUsdExportLocator.py
    testUsdExportMesh.py
    testUsdExportNurbsCurve.py
    testUsdExportOpenLayer.py
    testUsdExportOverImport.py
    testUsdExportParentScope.py
    # To investigate: following test asserts in MFnParticleSystem, but passes.
    # PPT, 17-Jun-20.
    testUsdExportParticles.py
    testUsdExportPref.py
    # Following test runs only in legacy render layer mode, otherwise skipped.
    # Legacy render layers are deprecated, and should not be used.
    # To avoid skipping and run in legacy render layer mode,
    # export MAYA_ENABLE_LEGACY_RENDER_LAYERS=1
    testUsdExportRenderLayerMode.py
    ## XXX: This test is disabled by default since it requires the RenderMan for Maya plugin.
    # testUsdExportRfMLight.py
    testUsdExportSelection.py
    testUsdExportSelectionHierarchy.py
    testUsdExportShadingInstanced.py
    testUsdExportShadingModeDisplayColor.py
    testUsdExportShadingModePxrRis.py
    testUsdExportSkeleton.py
    testUsdExportStripNamespaces.py
    testUsdExportVisibilityDefault.py
    testUsdImportCamera.py
    testUsdImportColorSets.py
    # To investigate: following test asserts in TDNshapeEditorManager.cpp, but
    # passes.  PPT, 17-Jun-20.
    testUsdImportFrameRange.py
    testUsdImportMayaReference.py
    testUsdImportMesh.py
    # XXX: This test is disabled by default since it requires the RenderMan for Maya plugin.
    # testUsdImportRfMLight.py
    testUsdImportSessionLayer.py
    testUsdImportShadingModeDisplayColor.py
    testUsdImportShadingModePxrRis.py
    testUsdExportImportRoundtripPreviewSurface.py
    testUsdImportSkeleton.py
    testUsdImportXforms.py
)

add_custom_target(${TARGET_NAME} ALL)

# copy all the resources and python scripts to build directory
mayaUsd_copyDirectory(${TARGET_NAME}
    SOURCE ${CMAKE_CURRENT_SOURCE_DIR}
    DESTINATION ${CMAKE_CURRENT_BINARY_DIR}
    EXCLUDE "*.txt"
)

foreach(script ${TEST_SCRIPT_FILES})
    mayaUsd_get_unittest_target(target ${script})
    mayaUsd_add_test(${target}
        PYTHON_MODULE ${target}
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    )

    # Assign a ctest label to these tests for easy filtering.
    set_tests_properties(${target} PROPERTIES LABELS translators)
endforeach()

# testUsdExportUVSets and testUsdImportUVSets are run twice, with float writing
# and reading (respectively) turned on and off.

mayaUsd_add_test(testUsdExportUVSets
    PYTHON_MODULE testUsdExportUVSets
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    ENV
        "PIXMAYA_WRITE_UV_AS_FLOAT2=0"
)
set_tests_properties(testUsdExportUVSets PROPERTIES LABELS translators)

mayaUsd_add_test(testUsdExportUVSetsFloat
    PYTHON_MODULE testUsdExportUVSets
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    ENV
        "PIXMAYA_WRITE_UV_AS_FLOAT2=1"
)
set_tests_properties(testUsdExportUVSetsFloat PROPERTIES LABELS translators)

mayaUsd_add_test(testUsdImportUVSets
    PYTHON_MODULE testUsdImportUVSets
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    ENV
        "PIXMAYA_READ_FLOAT2_AS_UV=0"
)
set_tests_properties(testUsdImportUVSets PROPERTIES LABELS translators)

mayaUsd_add_test(testUsdImportUVSetsFloat
    PYTHON_MODULE testUsdImportUVSets
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    ENV
        "PIXMAYA_READ_FLOAT2_AS_UV=1"
)
set_tests_properties(testUsdImportUVSetsFloat PROPERTIES LABELS translators)
