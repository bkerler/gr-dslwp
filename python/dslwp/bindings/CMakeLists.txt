# Copyright 2020 Free Software Foundation, Inc.
#
# This file is part of GNU Radio
#
# SPDX-License-Identifier: GPL-3.0-or-later
#

########################################################################
# Check if there is C++ code at all
########################################################################
if(NOT dslwp_sources)
    MESSAGE(STATUS "No C++ sources... skipping python bindings")
    return()
endif(NOT dslwp_sources)

########################################################################
# Check for pygccxml
########################################################################
GR_PYTHON_CHECK_MODULE_RAW(
    "pygccxml"
    "import pygccxml"
    PYGCCXML_FOUND
    )

include(GrPybind)

########################################################################
# Python Bindings
########################################################################

list(APPEND dslwp_python_files
        attach_sync_marker_python.cc
        ccsds_pseudo_randomizer_python.cc
        ccsds_turbo_decode_python.cc
        ccsds_turbo_encode_python.cc
        differential_phase_detection_1bit_cf_python.cc
        fec_encode_b_python.cc
        fft_estimator_cc_python.cc
        frame_spliter_c_python.cc
        frame_spliter_f_python.cc
        kiss_decode_pdu_python.cc
        kiss_encode_pdu_python.cc
        lrtc_mod_bc_python.cc
        oqpsk_coherent_demod_cc_python.cc
        program_tracking_cc_python.cc
        set_gain_by_tag_cc_python.cc
        steam_to_vector_moving_window_cc_python.cc
        tm_header_parser_python.cc
        tm_parser_python.cc
        vector_padding_cc_python.cc
        python_bindings.cc)

GR_PYBIND_MAKE_OOT(dslwp
   ../../..
   gr::dslwp
   "${dslwp_python_files}")

# copy in bindings .so file for use in QA test module
add_custom_target(
  copy_bindings_for_tests ALL
  COMMAND
    ${CMAKE_COMMAND} -E copy "${CMAKE_CURRENT_BINARY_DIR}/*.so"
    ${CMAKE_BINARY_DIR}/test_modules/gnuradio/dslwp/
  DEPENDS dslwp_python)

install(TARGETS dslwp_python DESTINATION ${GR_PYTHON_DIR}/gnuradio/dslwp COMPONENT pythonapi)
