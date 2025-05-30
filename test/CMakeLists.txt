if(NOT QGC_BUILD_TESTING)
    return()
endif()

find_package(Qt6 REQUIRED COMPONENTS Core Test)

include(CTest)
enable_testing()

add_custom_target(check
    COMMAND ctest --output-on-failure .
    USES_TERMINAL
)

function(add_qgc_test test_name)
    add_test(
        NAME ${test_name}
        COMMAND $<TARGET_FILE:${PROJECT_NAME}> --unittest:${test_name}
    )
    add_dependencies(check ${PROJECT_NAME})
endfunction()

add_subdirectory(AnalyzeView)
add_qgc_test(ExifParserTest)
# add_qgc_test(LogDownloadTest)
# add_qgc_test(MavlinkLogTest)
add_qgc_test(PX4LogParserTest)
add_qgc_test(ULogParserTest)

add_subdirectory(Audio)
add_qgc_test(AudioOutputTest)

# add_subdirectory(AutoPilotPlugins)
# add_qgc_test(RadioConfigTest)

add_subdirectory(Comms)

add_subdirectory(FactSystem)
add_qgc_test(FactSystemTestGeneric)
add_qgc_test(FactSystemTestPX4)
add_qgc_test(ParameterManagerTest)

add_subdirectory(Geo)
add_qgc_test(GeoTest)

add_subdirectory(MAVLink)
add_qgc_test(StatusTextHandlerTest)

add_subdirectory(MissionManager)
add_qgc_test(CameraCalcTest)
add_qgc_test(CameraSectionTest)
add_qgc_test(CorridorScanComplexItemTest)
# add_qgc_test(FWLandingPatternTest)
# add_qgc_test(LandingComplexItemTest)
# add_qgc_test(MissionCommandTreeEditorTest)
add_qgc_test(MissionCommandTreeTest)
add_qgc_test(MissionControllerManagerTest)
add_qgc_test(MissionControllerTest)
add_qgc_test(MissionItemTest)
add_qgc_test(MissionManagerTest)
add_qgc_test(MissionSettingsTest)
add_qgc_test(PlanMasterControllerTest)
add_qgc_test(QGCMapPolygonTest)
add_qgc_test(QGCMapPolylineTest)
# add_qgc_test(SectionTest)
add_qgc_test(SimpleMissionItemTest)
add_qgc_test(SpeedSectionTest)
add_qgc_test(StructureScanComplexItemTest)
add_qgc_test(SurveyComplexItemTest)
add_qgc_test(TransectStyleComplexItemTest)
# add_qgc_test(VisualMissionItemTest)

add_subdirectory(qgcunittest)
# add_qgc_test(FileDialogTest)
# add_qgc_test(MainWindowTest)
# add_qgc_test(MessageBoxTest)

add_subdirectory(QmlControls)

add_subdirectory(Terrain)
add_qgc_test(TerrainQueryTest)

add_subdirectory(UI)

add_subdirectory(Utilities)
# Compression
add_qgc_test(DecompressionTest)

add_subdirectory(Vehicle)
# Components
add_qgc_test(ComponentInformationCacheTest)
add_qgc_test(ComponentInformationTranslationTest)
add_qgc_test(FTPManagerTest)
# add_qgc_test(InitialConnectTest)
# add_qgc_test(RequestMessageTest)
# add_qgc_test(SendMavCommandWithHandlerTest)
# add_qgc_test(SendMavCommandWithSignalingTest)

# add_qgc_test(FlightGearUnitTest)
# add_qgc_test(LinkManagerTest)
# add_qgc_test(SendMavCommandTest)
# add_qgc_test(TCPLinkTest)

# set(UNIT_TESTING_RESOURCES)
# qt_add_resources(UNIT_TESTING_RESOURCES UnitTest.qrc)
qt_add_library(qgctest
    STATIC
        UnitTestList.cc
        UnitTestList.h
        # ${UNIT_TESTING_RESOURCES}
)

target_link_libraries(qgctest
    PRIVATE
        AnalyzeViewTest
        AudioTest
        CommsTest
        CompressionTest
        FactSystemTest
        GeoTest
        MAVLinkTest
        MissionManagerTest
        QmlControlsTest
        TerrainTest
        UITest
        VehicleTest
        VehicleComponentsTest
        QGC
        Utilities
        UtilitiesTest
    PUBLIC
        Qt6::Core
        qgcunittest
)

target_include_directories(qgctest INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})
