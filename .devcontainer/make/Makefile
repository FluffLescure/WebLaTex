define error_exit
	@echo "${1:-"Unknown Error"}" && exit 1
endef

utils_PATH := $(HOME)/.config/latex-utils
hsnips_PATH := $(HOME)/.config/HyperSnips

COMMON_FILES := $(wildcard ./common/*.tex)
LOGOS_FILES := $(wildcard ./logos/*.*)
common_dest_files := $(patsubst ./common/%.tex, ${utils_PATH}/common/%.tex, $(COMMON_FILES))
logos_dest_files := $(patsubst ./logos/%, ${utils_PATH}/logos/%, $(LOGOS_FILES))


all : intro directories sources common logos snippets end

intro :
	@echo "The latex-utils directory path will be: ${utils_PATH}"; \
	echo "The HyperSnips directory path will be: ${hsnips_PATH}"

directories :
	@if test -e ${utils_PATH} && test -e ${hsnips_PATH} && test -e ${utils_PATH}/common && test -e ${utils_PATH}/logos; then \
		echo "Directories already exist"; \
	else \
		mkdir -p ${utils_PATH} ${hsnips_PATH} ${utils_PATH}/common ${utils_PATH}/logos; \
		if [ $$? -ne 0 ]; then \
			$(call error_exit, "\tError creating directories"); \
		else \
			echo "Directories successfully created"; \
		fi; \
	fi


sources : 
	@sed -i "s|~/projects/common/logos|${utils_PATH}/logos|g" common/cover.tex; \
	sed -i "s|~/projects/common|${utils_PATH}/common|g" latex.hsnips; \
	echo "sources for latex.hsnips and common/cover.tex successfully updated"



${utils_PATH}/common/%.tex: ./common/%.tex
	@echo "Unpacking $< to $@"
	@rsync -uq $< $@

common: $(common_dest_files) 
	@echo "Files in ./common successfully unpacked"

${utils_PATH}/logos/%: ./logos/%
	@echo "Unpacking $< to $@"
	@rsync -uq $< $@

logos :$(logos_dest_files) 
	@echo "Files in ./logos successfully unpacked"

snippets :
	@echo "Unpacking latex.hsnips to ${hsnips_PATH}/latex.hsnips"; \
	rsync -uq latex.hsnips ${hsnips_PATH}/latex.hsnips; \
	if [ $$? -ne 0 ]; then \
		$(call error_exit, "\tError unpacking latex.hsnips"); \
	else \
		echo "File latex.hsnips successfully unpacked"; \
	fi

end : 
	@echo "Setup completed successfully"; \
	echo "Please configure HyperSnips to use the latex.hsnips file in ${hsnips_PATH} by adding the following line to your settings.json file:\n"; \
	echo "\t\"hsnips.hsnipsPath\": \"${hsnips_PATH}\"\n"

clean :
	rm -r ${utils_PATH}
	rm -r ${hsnips_PATH}
