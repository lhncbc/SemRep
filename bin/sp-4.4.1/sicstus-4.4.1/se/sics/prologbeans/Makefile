#
# Copyright (C) 2003 Swedish Institute of Computer Science.
#
# NOTE: This Makefile requires GNU make.
#

SRCDIR=.

.PHONY: default
default: all

# Include common Makefile parts.
include $(SRCDIR)/../../../Common

ifneq ($(FORCE_PARMAKE),yes) # DBG
.NOTPARALLEL: # [PM] 4.0.2+ make -j probably does not work with this file
endif

# Classes to build
JAVA_FILES := $(wildcard *.java)

EXAMPLES := 

.PHONY: all
ifeq ($(ENABLE_JAVA),yes)	# [PM] 3.11.2 We do not care about PROLOGBEANS_MODULE only Java
all:  prologbeans.jar
else				#!ENABLE_JAVA
all:
	@echo "Nothing to be done here; PrologBeans is not supported."
endif				#!ENABLE_JAVA


override JAVACFLAGS += $(javacflags_extra)
# [PM] 3.10.2 Recompile everything if any $(JAVA_FILES) changes
#             It is much to hard to figure out which .class files are generated by the $(JAVA_FILES)
#
#             NOTE: cannot use $(shell find -name *.class) or
#                   $(wildcard *.class) since the class-files does not
#                   exist when this rule *starts* running.
#                   Instead we use the somewhat ugly tmp-dir-hack
#
prologbeans.jar: PrologBeans.manifest $(JAVA_FILES) javax.servlet.jar
	rm -rf tmp	
	mkdir -p tmp
	$(JAVAC) -d tmp $(JAVACFLAGS) $(JAVACLINTFLAGS) -deprecation -classpath '$(SRCDIR)/../../..$(PATHSEP)javax.servlet.jar' $(filter %.java, $^)
	$(JAR) cmf $< $@ -C tmp se/sics/prologbeans
	: # rm -rf tmp

.PHONY: install
# [PM] 3.10.2 there is no need to install .class files.
install:
	$(MKDIR) $(SP_LIBDIR)/se/sics/prologbeans
	for file in $(JAVA_FILES) Makefile; do \
	    if expr $$file : '.*~.*' > /dev/null || expr $$file : '\.#.*' > /dev/null; then echo "Skipping $$file..." ; continue; fi; \
	    echo "Installing $$file..." ;\
	    $(INSTALL_DATA) $$file $(SP_LIBDIR)/se/sics/prologbeans ;\
	done

.PHONY: examples
# [PM] the example code now resides in library/prologbeans/examples/. 
examples:
	@echo "The examples have been misplaced.. move here later/PM"
# examples:
# 	(cd examples; \
# 		$(MAKE) -f $(SRCDIR)/../Makefile SRCDIR=.. $(EXAMPLES:=.class))

.PHONY: clean distclean mostlyclean maintainerclean
clean distclean mostlyclean maintainerclean:
	rm -f $(patsubst %,'%',$(filter-out javax.servlet.jar, $(wildcard *.class examples/*.class *~ *.jar))) $(RMDUMMY)
	rm -rf tmp
