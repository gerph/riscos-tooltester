COMPONENT = riscos-tooltester

.PHONY: all links cross_export cross_install tool_dir clean

all: cross_install

cross_export:
	@echo No exports

cross_install: \
		tool_dir \
		$(ROTOOL_DIR)/$(COMPONENT)
	@echo Installed to $(ROTOOL_DIR)

$(ROTOOL_DIR)/riscos-tooltester: ../tooltester.pl
	cp $? $@

tool_dir:
	mkdir -p $(ROTOOL_DIR)

# Note: Currently the tests for the tool are in the primary repository in my gerph
# 		user in Git, not in here

clean:;

# Links
