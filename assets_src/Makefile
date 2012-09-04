# Source paths
BOOTSTRAP_DIR = bootstrap
BOOTSTRAP_SRC = ./vendor/${BOOTSTRAP_DIR}

FONTAWESOME_DIR = font-awesome
FONTAWESOME_SRC = ./vendor/${FONTAWESOME_DIR}
LIBS_SRC = ./vendor/js
IMG_PATH = ./img
COFFEE_PATH = ./coffee
LESS_PATH = ./less
# Build paths
ASSETS_PATH = ../assets
DATE=$(shell date +%I:%M%p)
TMP_PATH=/tmp/plainify
CHECK=\033[32m✔\033[39m
HR=\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#


build:
	@make create_staging
	@make fontawesome
	@make bootstrap

	@echo "\n${HR}"
	@echo "Moving stuff..."
	@echo "${HR}\n"
	@make jslibs
	@make img
	@make less
	@make coffee
	@make remove_staging

	@echo "\n${HR}"
	@echo "Assets successfully built at ${DATE}."
	@echo "Find the files in ${ASSETS_PATH}."
	@echo "${HR}\n"


create_staging:
	@echo "\n${HR}"
	@echo "Create staging..."
	@echo "${HR}\n"

	@mkdir -p ${ASSETS_PATH}
	@mkdir -p ${TMP_PATH}/less

	@cp -pr ${BOOTSTRAP_SRC}/less/* ${TMP_PATH}/less/
	@echo "Copying Bootrap LESS to tmp dir...         ${CHECK} Done"

	@cp -pr ${FONTAWESOME_SRC}/less/* ${TMP_PATH}/less/
	@echo "Copying Font-Awesome LESS to tmp dir...    ${CHECK} Done"

remove_staging:
	@rm -rf ${TMP_PATH}/
	@echo "Deleting tmp dir...                        ${CHECK} Done"

fontawesome:
	@echo "\n${HR}"
	@echo "Integrating Font-Awesome into Bootstrap..."
	@echo "${HR}\n"

	@cp ${FONTAWESOME_SRC}/font/* ${ASSETS_PATH}/
	@echo "Copying fonts over...                      ${CHECK} Done"

	@sed -i "" "s/^@fontAwesomePath.*/@fontAwesomePath: '.';/g" ${TMP_PATH}/less/font-awesome.less 
	@echo "Setting font path...                       ${CHECK} Done"

	@sed -i "" 's|^@import "sprites.less";|@import "font-awesome.less";|g' ${TMP_PATH}/less/bootstrap.less
	@echo "Include Font-Awesome in Bootstrap...       ${CHECK} Done"

	@lessc /tmp/plainify/less/font-awesome-ie7.less > ${ASSETS_PATH}/font-awesome-ie7.css
	@lessc /tmp/plainify/less/font-awesome-ie7.less --yui-compress > ${ASSETS_PATH}/font-awesome-ie7.min.css
	@echo "Compiling ie7 support css...               ${CHECK} Done"

bootstrap:
	@echo "\n${HR}"
	@echo "Building Bootstrap..."
	@echo "${HR}\n"
	@cp ${BOOTSTRAP_SRC}/img/* ${ASSETS_PATH}/
	@echo "Copying bootstrap images...                ${CHECK} Done"
	@recess --compile ${TMP_PATH}/less/bootstrap.less > ${ASSETS_PATH}/bootstrap.css
	@recess --compress ${TMP_PATH}/less/bootstrap.less > ${ASSETS_PATH}/bootstrap.min.css
	@recess --compile ${TMP_PATH}/less/responsive.less > ${ASSETS_PATH}/bootstrap-responsive.css
	@recess --compress ${TMP_PATH}/less/responsive.less > ${ASSETS_PATH}/bootstrap-responsive.min.css
	@echo "Compiling bootstrap LESS with Recess...    ${CHECK} Done"
	@cat ${BOOTSTRAP_SRC}/js/bootstrap-transition.js ${BOOTSTRAP_SRC}/js/bootstrap-alert.js ${BOOTSTRAP_SRC}/js/bootstrap-button.js ${BOOTSTRAP_SRC}/js/bootstrap-carousel.js ${BOOTSTRAP_SRC}/js/bootstrap-collapse.js ${BOOTSTRAP_SRC}/js/bootstrap-dropdown.js ${BOOTSTRAP_SRC}/js/bootstrap-modal.js ${BOOTSTRAP_SRC}/js/bootstrap-tooltip.js ${BOOTSTRAP_SRC}/js/bootstrap-popover.js ${BOOTSTRAP_SRC}/js/bootstrap-scrollspy.js ${BOOTSTRAP_SRC}/js/bootstrap-tab.js ${BOOTSTRAP_SRC}/js/bootstrap-typeahead.js ${BOOTSTRAP_SRC}/js/bootstrap-affix.js > ${ASSETS_PATH}/bootstrap.js
	@uglifyjs -nc ${ASSETS_PATH}/bootstrap.js > ${ASSETS_PATH}/bootstrap.min.js
	@echo "Compiling and minifying javascript...      ${CHECK} Done"

jslibs:
	@cat ${LIBS_SRC}/* | uglifyjs  -nc -o ${ASSETS_PATH}/libs.js
	@echo "Concatenate & compressing libs...          ${CHECK} Done"

coffee:
	@coffee --join ${ASSETS_PATH}/application.js --compile ${COFFEE_PATH}/*.coffee
	@uglifyjs -nc ${ASSETS_PATH}/application.js > ${ASSETS_PATH}/application.min.js
	@echo "Compiling coffee...                        ${CHECK} Done"

less:
	@recess --compile ${LESS_PATH}/application.less > ${ASSETS_PATH}/application.css
	@recess --compress ${LESS_PATH}/application.less > ${ASSETS_PATH}/application.min.css
	@echo "Build custom styles/less...                ${CHECK} Done"
img:
	@find ${IMG_PATH} -type f -exec cp {} ${ASSETS_PATH} \;
	@echo "Copying images to assets folder...         ${CHECK} Done"

#
# WATCH LESS FILES
#
watch:
	echo "Watching less files..."; \
	watchr -e "watch('less/.*\.less') { system 'make' }"

.PHONY: all img less jslibs coffee
