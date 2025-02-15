INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR})

INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/../../src/Graph) # we need this line because this file is not on the header file List of MueLu
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/../../src/Smoothers)

# first ensure that these cmake boolean variables are defined
ASSERT_DEFINED(
  ${PACKAGE_NAME}_ENABLE_Epetra
  ${PACKAGE_NAME}_ENABLE_EpetraExt
  ${PACKAGE_NAME}_ENABLE_Amesos
  )


IF (${PACKAGE_NAME}_ENABLE_Epetra AND
    ${PACKAGE_NAME}_ENABLE_EpetraExt AND
    ${PACKAGE_NAME}_ENABLE_Ifpack AND
    ${PACKAGE_NAME}_ENABLE_Amesos)

  IF(${PACKAGE_NAME}_ENABLE_AztecOO)
    # build only if AztecOO available, too
    TRIBITS_ADD_EXECUTABLE_AND_TEST(
      Navier2D_Epetra
      SOURCES Navier2D_Epetra.cpp
      COMM mpi
      )
    TRIBITS_COPY_FILES_TO_BINARY_DIR(Navier2D_cpdata
      SOURCE_FILES A_re1000_5932.txt b_re1000_5932.txt
      )

    TRIBITS_ADD_EXECUTABLE_AND_TEST(
      Navier2DBlocked_Epetra
      SOURCES Navier2DBlocked_Epetra.cpp
      COMM serial mpi
      )

    ################################# tests for xml framework
    TRIBITS_ADD_EXECUTABLE(
      Navier2DBlocked_xml
      SOURCES Navier2DBlocked_xml.cpp
      COMM mpi
      )

    TRIBITS_ADD_TEST(
      Navier2DBlocked_xml
      NAME "Navier2DBlocked_xml_format"
      ARGS "--xml=myXML.xml"
      NUM_MPI_PROCS 4
      COMM mpi
      )

    TRIBITS_ADD_TEST(
      Navier2DBlocked_xml
      NAME "Navier2DBlocked_xml_format2"
      ARGS "--xml=myXMLg.xml"
      NUM_MPI_PROCS 4
      COMM mpi
      )

    TRIBITS_ADD_TEST(
      Navier2DBlocked_xml
      NAME "Navier2DBlocked_xml_blockdirect"
      ARGS "--xml=myXML2.xml"
      NUM_MPI_PROCS 4
      COMM mpi
      )

    TRIBITS_ADD_TEST(
      Navier2DBlocked_xml
      NAME "Navier2DBlocked_xml_bgs1"
      ARGS "--xml=myBGS1.xml"
      NUM_MPI_PROCS 4
      COMM mpi
      )

    TRIBITS_ADD_TEST(
      Navier2DBlocked_xml
      NAME "Navier2DBlocked_xml_bs1"
      ARGS "--xml=myBS1.xml"
      NUM_MPI_PROCS 4
      COMM mpi
      )

    TRIBITS_ADD_TEST(
      Navier2DBlocked_xml
      NAME "Navier2DBlocked_xml_bs2"
      ARGS "--xml=myBS2.xml"
      NUM_MPI_PROCS 4
      COMM mpi
      )

    TRIBITS_ADD_TEST(
      Navier2DBlocked_xml
      NAME "Navier2DBlocked_xml_bs3"
      ARGS "--xml=myBS3.xml"
      NUM_MPI_PROCS 4
      COMM mpi
      )

    TRIBITS_ADD_TEST(
      Navier2DBlocked_xml
      NAME "Navier2DBlocked_xml_sim1"
      ARGS "--xml=mySIM1.xml"
      NUM_MPI_PROCS 4
      COMM mpi
      )

    TRIBITS_ADD_TEST(
      Navier2DBlocked_xml
      NAME "Navier2DBlocked_xml_sim2"
      ARGS "--xml=mySIM2.xml"
      NUM_MPI_PROCS 4
      COMM mpi
      )

    TRIBITS_ADD_TEST(
      Navier2DBlocked_xml
      NAME "Navier2DBlocked_xml_uzawa1"
      ARGS "--xml=myUzawa1.xml"
      NUM_MPI_PROCS 4
      COMM mpi
      )

    TRIBITS_ADD_TEST(
      Navier2DBlocked_xml
      NAME "Navier2DBlocked_xml_uzawa2"
      ARGS "--xml=myUzawa2.xml"
      NUM_MPI_PROCS 4
      COMM mpi
      )

    TRIBITS_ADD_TEST(
      Navier2DBlocked_xml
      NAME "Navier2DBlocked_xml_indef1"
      ARGS "--xml=myIndefDiag1.xml"
      NUM_MPI_PROCS 4
      COMM mpi
      )
    ##################################
    TRIBITS_ADD_EXECUTABLE_AND_TEST(
      Navier2DBlocked_BraessSarazinSmoother
      SOURCES Navier2DBlocked_BraessSarazinSmoother.cpp
      COMM serial mpi
      )

    TRIBITS_ADD_EXECUTABLE_AND_TEST(
      Navier2DBlocked_SimpleSmoother
      SOURCES Navier2DBlocked_SimpleSmoother.cpp
      COMM serial mpi
      )

    TRIBITS_ADD_EXECUTABLE_AND_TEST(
      Navier2DBlocked_BraessSarazin
      SOURCES Navier2DBlocked_BraessSarazin.cpp
      COMM serial mpi
      )

    TRIBITS_ADD_EXECUTABLE(
      Navier2DBlocked_rebalancing2
      SOURCES Navier2DBlocked_test2.cpp
      COMM mpi
      )

    # TODO: fix CloneRepartitionInterface
    TRIBITS_ADD_TEST(
      Navier2DBlocked_rebalancing2
      NAME "Navier2DBlockedReuseAggs"
      ARGS "--rebalanceBlocks=0"
      NUM_MPI_PROCS 4
      COMM mpi
      )
    #TRIBITS_ADD_TEST(
    #  Navier2DBlocked_rebalancing2
    #  NAME "Navier2DBlockedReuseAggsReb"
    #  ARGS "--rebalanceBlocks=1"
    #  NUM_MPI_PROCS 4
    #  COMM mpi
    #  )

    TRIBITS_ADD_EXECUTABLE_AND_TEST(
      Navier2DBlocked_Simple
      SOURCES Navier2DBlocked_Simple.cpp
      COMM serial mpi
      )
    TRIBITS_COPY_FILES_TO_BINARY_DIR(Navier2D_cp
      SOURCE_FILES myXML.xml myXMLg.xml myXML2.xml myBS1.xml myBS2.xml myBS3.xml myBGS1.xml mySIM1.xml mySIM2.xml myUzawa1.xml myUzawa2.xml myIndefDiag1.xml
      )
  ENDIF()

ENDIF()
