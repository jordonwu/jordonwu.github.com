---
layout: post
title: "Yocto Usage Summary"
date: 2025-08-29 21:00:00 +0800
comments: true
categories: yocto linux
---

# <center> Yocto Usage Summary

## Commands
### listtask - "bitbake -c listtasks <target>"
```
$ bitbake -c listtasks core-image-minimal
NOTE: No setscene tasks
NOTE: Executing Tasks
do_build                              Default task for a recipe - depends on all other normal tasks required to 'build' a recipe
do_build_without_rm_work
do_checkuri                           Validates the SRC_URI value
do_clean                              Removes all output files for a target
do_cleanall                           Removes all output files, shared state cache, and downloaded source files for a target
do_cleansstate                        Removes all output files and shared state cache for a target
do_collect_spdx_deps
do_compile                            Compiles the source in the compilation directory
do_configure                          Configures the source by enabling and disabling any build-time and configuration options for the software being built
do_create_image_sbom_spdx
do_create_image_sbom_spdx_setscene     (setscene version)
do_create_image_spdx
do_create_image_spdx_setscene          (setscene version)
do_create_package_spdx
do_create_package_spdx_setscene        (setscene version)
do_create_rootfs_spdx
do_create_rootfs_spdx_setscene         (setscene version)
do_create_spdx
do_create_spdx_setscene                (setscene version)
do_deploy_source_date_epoch
do_deploy_source_date_epoch_setscene   (setscene version)
do_devshell                           Starts a shell with the environment set up for development/debugging
do_fetch                              Fetches the source code
do_flush_pseudodb
do_image
do_image_complete
do_image_complete_setscene             (setscene version)
do_image_qa
do_image_qa_setscene                   (setscene version)
do_image_tar
do_image_wic
do_install                            Copies files from the compilation directory to a holding area
do_listtasks                          Lists all defined tasks for a target
do_patch                              Locates patch files and applies them to the source code
do_populate_lic_deploy
do_populate_lic_setscene              Writes license information for the recipe that is collected later when the image is constructed (setscene version)
do_populate_mfgtool
do_populate_sdk                       Creates the file and directory structure for an installable SDK
do_populate_sdk_ext
do_populate_sdk_setscene              Creates the file and directory structure for an installable SDK (setscene version)
do_populate_sysroot_setscene          Copies a subset of files installed by do_install into the sysroot in order to make them available to other recipes (setscene version)
do_prepare_recipe_sysroot
do_pydevshell                         Starts an interactive Python shell for development/debugging
do_recipe_qa
do_recipe_qa_setscene                  (setscene version)
do_rm_work                            Removes work files after the build system has finished with them
do_rm_work_all                        Top-level task for removing work files after the build system has finished with them
do_rootfs                             Creates the root filesystem (file and directory structure) for an image
do_rootfs_wicenv
do_sdk_depends
do_unpack                             Unpacks the source code into a working directory
do_write_wks_template
NOTE: Tasks Summary: Attempted 1 tasks of which 0 didn't need to be rerun and all succeeded.
```

```
$ bitbake -c listtasks gnutls
......
NOTE: No setscene tasks
NOTE: Executing Tasks
do_build                              Default task for a recipe - depends on all other normal tasks required to 'build' a recipe
do_build_without_rm_work
do_checkuri                           Validates the SRC_URI value
do_clean                              Removes all output files for a target
do_cleanall                           Removes all output files, shared state cache, and downloaded source files for a target
do_cleansstate                        Removes all output files and shared state cache for a target
do_collect_spdx_deps
do_compile                            Compiles the source in the compilation directory
do_compile_ptest_base                 Compiles the runtime test suite included in the software being built
do_configure                          Configures the source by enabling and disabling any build-time and configuration options for the software being built
do_configure_ptest_base               Configures the runtime test suite included in the software being built
do_create_package_spdx
do_create_package_spdx_setscene        (setscene version)
do_create_spdx
do_create_spdx_setscene                (setscene version)
do_deploy_source_date_epoch
do_deploy_source_date_epoch_setscene   (setscene version)
do_devshell                           Starts a shell with the environment set up for development/debugging
do_fetch                              Fetches the source code
do_install                            Copies files from the compilation directory to a holding area
do_install_ptest_base                 Copies the runtime test suite files from the compilation directory to a holding area
do_listtasks                          Lists all defined tasks for a target
do_package                            Analyzes the content of the holding area and splits it into subsets based on available packages and files
do_package_qa                         Runs QA checks on packaged files
do_package_qa_setscene                Runs QA checks on packaged files (setscene version)
do_package_setscene                   Analyzes the content of the holding area and splits it into subsets based on available packages and files (setscene version)
do_package_write_deb                  Creates the actual DEB packages and places them in the Package Feed area
do_package_write_deb_setscene         Creates the actual DEB packages and places them in the Package Feed area (setscene version)
do_packagedata                        Creates package metadata used by the build system to generate the final packages
do_packagedata_setscene               Creates package metadata used by the build system to generate the final packages (setscene version)
do_patch                              Locates patch files and applies them to the source code
do_populate_lic                       Writes license information for the recipe that is collected later when the image is constructed
do_populate_lic_setscene              Writes license information for the recipe that is collected later when the image is constructed (setscene version)
do_populate_sysroot                   Copies a subset of files installed by do_install into the sysroot in order to make them available to other recipes
do_populate_sysroot_setscene          Copies a subset of files installed by do_install into the sysroot in order to make them available to other recipes (setscene version)
do_prepare_recipe_sysroot
do_pydevshell                         Starts an interactive Python shell for development/debugging
do_recipe_qa
do_recipe_qa_setscene                  (setscene version)
do_rm_work                            Removes work files after the build system has finished with them
do_rm_work_all                        Top-level task for removing work files after the build system has finished with them
do_unpack                             Unpacks the source code into a working directory
NOTE: Tasks Summary: Attempted 1 tasks of which 0 didn't need to be rerun and all succeeded.
```


## 99. Reference Link

1> 
