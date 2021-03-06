#
# Copyright (C) Mellanox Technologies Ltd. 2001-2014.  ALL RIGHTS RESERVED.
# Copyright (C) UT-Battelle, LLC. 2015. ALL RIGHTS RESERVED.
# Copyright (C) The University of Tennessee and the University of Tennessee Research Foundation. 2016. ALL RIGHTS RESERVED.
#
# See file LICENSE for terms.
#


UCX_HANDLE_ERRORS ?= freeze
export UCX_HANDLE_ERRORS

GTEST_FILTER        ?= *
GTEST_EXTRA_ARGS    =
LAUNCHER            =
VALGRIND_EXTRA_ARGS =

GTEST_ARGS = \
	--gtest_filter=$(GTEST_FILTER) \
	$(GTEST_EXTRA_ARGS)

VALGRIND_ARGS = \
	--tool=memcheck \
	--leak-check=full \
	--track-origins=yes \
	--fair-sched=try \
	$(VALGRIND_EXTRA_ARGS)

noinst_PROGRAMS = gtest

gtestdir  = $(includedir)
gtest_LDADD = \
	$(top_builddir)/src/ucs/libucs.la \
	$(top_builddir)/src/uct/libuct.la \
	$(top_builddir)/src/ucm/libucm.la \
	$(top_builddir)/src/ucp/libucp.la \
	$(OPENMP_CFLAGS) $(top_builddir)/src/tools/perf/libucxperf.la \
	$(GTEST_LIBS)

gtest_CPPFLAGS = \
	-I$(top_srcdir)/src \
	-I$(top_srcdir)/test \
	-I$(top_builddir)/src \
	-I$(top_srcdir)/test/gtest \
	$(GTEST_CPPFLAGS)

gtest_LDFLAGS  = $(GTEST_LDFLAGS) -no-install
gtest_CXXFLAGS = $(GTEST_CXXFLAGS) -g -O3 -fno-tree-vectorize

gtest_SOURCES = \
	common/gtest-all.cc \
	common/main.cc \
	common/test_helpers.cc \
	common/test_perf.cc \
	common/test.cc \
	\
	ucm/malloc_hook.cc \
	\
	uct/test_amo.cc \
	uct/test_amo_add.cc \
	uct/test_amo_cswap.cc \
	uct/test_amo_fadd.cc \
	uct/test_amo_swap.cc \
	uct/test_fence.cc \
	uct/test_flush.cc \
	uct/test_many2one_am.cc \
	uct/test_mm.cc \
	uct/test_mem.cc \
	uct/test_p2p_am.cc \
	uct/test_p2p_err.cc \
	uct/test_p2p_mix.cc \
	uct/test_p2p_rma.cc \
	uct/test_pd.cc \
	uct/test_pending.cc \
	uct/test_uct_ep.cc \
	uct/test_uct_perf.cc \
	uct/uct_p2p_test.cc \
	uct/uct_test.cc \
	uct/test_stats.cc \
	uct/test_wakeup.cc \
	uct/test_error_handling.cc \
	\
	ucp/test_ucp_atomic.cc \
	ucp/test_ucp_memheap.cc \
	ucp/test_ucp_mmap.cc \
	ucp/test_ucp_perf.cc \
	ucp/test_ucp_rma.cc \
	ucp/test_ucp_tag_cancel.cc \
	ucp/test_ucp_tag_match.cc \
	ucp/test_ucp_tag_probe.cc \
	ucp/test_ucp_tag_xfer.cc \
	ucp/test_ucp_tag.cc \
	ucp/test_ucp_context.cc \
	ucp/test_ucp_wireup.cc \
	ucp/test_ucp_wakeup.cc \
	ucp/test_ucp_fence.cc \
	ucp/ucp_test.cc \
	\
	ucs/test_algorithm.cc \
	ucs/test_arbiter.cc \
	ucs/test_async.cc \
	ucs/test_callbackq.cc \
	ucs/test_class.cc \
	ucs/test_component.cc \
	ucs/test_config.cc \
	ucs/test_datatype.cc \
	ucs/test_debug.cc \
	ucs/test_memtrack.cc \
	ucs/test_instr.cc \
	ucs/test_math.cc \
	ucs/test_mpmc.cc \
	ucs/test_mpool.cc \
	ucs/test_pgtable.cc \
	ucs/test_profile.cc \
	ucs/test_rcache.cc \
	ucs/test_stats.cc \
	ucs/test_sys.cc \
	ucs/test_time.cc \
	ucs/test_twheel.cc \
	ucs/test_frag_list.cc \
	ucs/test_hash_perf.cc

if HAVE_IB
gtest_SOURCES += \
    uct/test_ib.cc
gtest_CPPFLAGS += \
    $(IBVERBS_CPPFLAGS)
if HAVE_TL_UD
gtest_SOURCES += \
	uct/ud_base.cc \
	uct/test_ud.cc \
	uct/test_ud_slow_timer.cc \
	uct/test_ud_pending.cc \
	uct/test_ud_ds.cc
endif
if HAVE_TL_RC
gtest_SOURCES += \
	uct/test_rc.cc
endif	
if HAVE_TL_DC
gtest_SOURCES += \
	uct/test_dc.cc
endif
endif

noinst_HEADERS = \
	common/gtest.h \
	common/test.h \
	common/test_helpers.h \
	common/test_perf.h \
	common/tap.h \
	\
	uct/test_amo.h \
	uct/uct_p2p_test.h \
	uct/uct_test.h \
	uct/ud_base.h \
	\
	ucp/test_ucp_atomic.h \
	ucp/test_ucp_memheap.h \
	ucp/test_ucp_tag.h \
	ucp/ucp_test.h

.PHONY: test test gdb valgrind fix_rpath ucx


all-local: gtest

ucx:
	$(MAKE) -C $(top_builddir)


help:
	@echo
	@echo "Targets:"
	@echo "  list          : List unit tests."
	@echo "  test          : Run unit tests."
	@echo "  test_gdb      : Run unit tests with GDB."
	@echo "  test_valgrind : Run unit tests with Valgrind."
	@echo
	@echo "Environment variables:"
	@echo "  GTEST_FILTER        : Unit tests filter (\"$(GTEST_FILTER)\")"
	@echo "  GTEST_EXTRA_ARGS    : Additional arguments for gtest (\"$(GTEST_EXTRA_ARGS)\")"
	@echo "  LAUNCHER            : Custom launcher for gtest executable (\"$(LAUNCHER)\")"
	@echo "  VALGRIND_EXTRA_ARGS : Additional arguments for Valgrind (\"$(VALGRIND_EXTRA_ARGS)\")"
	@echo

#
# List unit tests
#
list: gtest
	$(abs_builddir)/gtest --gtest_list_tests $(GTEST_ARGS)

#
# Run unit tests
#
test: ucx gtest
	@rm -f core.*
	$(LAUNCHER) $(abs_builddir)/gtest $(GTEST_ARGS)
	
#
# Run unit tests with GDB
#
test_gdb: ucx gtest
	echo -e 'r\ninit-if-undefined $$_exitcode=-1\nif $$_exitcode>=0\n\tq\nend' > .gdbcommands
	$(LAUNCHER) env UCX_HANDLE_ERRORS=none \
		UCX_GDB_PATH="" \
		gdb -x .gdbcommands --args $(GDB_ARGS) \
			$(abs_builddir)/gtest $(GTEST_ARGS)

#
# Run unit tests with valgrind
#
test_valgrind: ucx gtest
	$(LAUNCHER) env LD_LIBRARY_PATH="$(VALGRIND_LIBPATH):${LD_LIBRARY_PATH}" \
	valgrind $(VALGRIND_ARGS) $(abs_builddir)/gtest $(GTEST_ARGS)
