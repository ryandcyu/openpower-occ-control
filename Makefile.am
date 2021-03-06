if !INSTALL_ERROR_YAML
# Build these headers, don't install them
noinst_HEADERS = \
	occ_pass_through.hpp \
	occ_status.hpp \
	powercap.hpp \
	occ_errors.hpp \
	occ_events.hpp \
	occ_finder.hpp \
	occ_presence.hpp \
	utils.hpp

noinst_LTLIBRARIES = libocc_control.la

sbin_PROGRAMS = openpower-occ-control

openpower_occ_control_SOURCES = \
	app.cpp

libocc_control_la_SOURCES = \
	occ_pass_through.cpp \
	occ_manager.cpp \
	occ_status.cpp \
	occ_device.cpp \
	occ_errors.cpp \
	occ_presence.cpp \
	powercap.cpp \
	org/open_power/OCC/Device/error.cpp \
	occ_finder.cpp \
	i2c_occ.cpp \
	utils.cpp

openpower_occ_control_LDADD = libocc_control.la -lstdc++fs

BUILT_SOURCES =  org/open_power/OCC/Device/error.hpp \
                 org/open_power/OCC/Device/error.cpp \
                 occ_sensor.hpp

CLEANFILES = ${BUILT_SOURCES}

REQ_MAKO_FILE ?= ${top_srcdir}/occ_sensor.mako.hpp
REQ_PY_SCRIPT ?= ${top_srcdir}/sensor_gen.py

EXTRA_DIST = $(REQ_MAKO_FILE) $(REQ_PY_SCRIPT)

occ_sensor.hpp: ${REQ_PY_SCRIPT} ${REQ_MAKO_FILE}
	$(AM_V_GEN) ${PYTHON} ${REQ_PY_SCRIPT} -i ${YAML_PATH} > $@


generic_ld_flags = \
	$(SDBUSPLUS_LIBS) \
	$(PHOSPHOR_LOGGING_LIBS) \
	$(OPENPOWER_DBUS_INTERFACES_LIBS) \
	$(PHOSPHOR_DBUS_INTERFACES_LIBS)

generic_cxx_flags = \
	$(SDBUSPLUS_CFLAGS) \
	$(PHOSPHOR_LOGGING_CFLAGS) \
	$(OPENPOWER_DBUS_INTERFACES_CFLAGS) \
	$(PHOSPHOR_DBUS_INTERFACES_CFLAGS)

libocc_control_la_CXXFLAGS = $(generic_cxx_flags)

libocc_control_la_LDFLAGS = $(generic_ld_flags)

openpower_occ_control_CXXFLAGS = $(generic_cxx_flags)

openpower_occ_control_LDFLAGS = $(generic_ld_flags)

org/open_power/OCC/Device/error.hpp: ${top_srcdir}/org/open_power/OCC/Device.errors.yaml
	@mkdir -p `dirname $@`
	$(SDBUSPLUSPLUS) -r $(top_srcdir) error exception-header org.open_power.OCC.Device > $@

org/open_power/OCC/Device/error.cpp: ${top_srcdir}/org/open_power/OCC/Device.errors.yaml
	@mkdir -p `dirname $@`
	$(SDBUSPLUSPLUS) -r $(top_srcdir) error exception-cpp org.open_power.OCC.Device > $@

SUBDIRS = . test
endif

# Export error YAML to shared location
yamldir = ${datadir}/phosphor-dbus-yaml/yaml
nobase_yaml_DATA = \
	org/open_power/OCC/Device.errors.yaml \
	org/open_power/OCC/Device.metadata.yaml
