# Makefiles relationship
> [Syntax](https://mermaid.js.org/syntax/flowchart.html)

```mermaid
flowchart TB
    %% ----- Objects -----
    subgraph rcp-bsp
        Makefile[Makefile]
    
        subgraph make[~/make/]
            make_env[env.mk]
            make_version[version.mk]
            make_custom[custom.mk]
        end

        Makefile -- inc --> make_env
        Makefile -- inc--> make_version
        Makefile -. optional inc .-> make_custom
    
        subgraph board[~/board/rv1106g/make/]
            board_env-mk[env.mk]
            subgraph board_source[./source/]
                board_source_buildroot[buildroot.mk]
                board_source_cache[cache.mk]
                board_source_linux[linux.mk]
                board_source_uboot[uboot.mk]
            end
        end

        %% ----- Lines -----
    
        make_env -- inc --> board_env-mk
    
        board_env-mk -. optional inc .-> board_source_buildroot
        board_env-mk -. optional inc .-> board_source_cache
        board_env-mk -. optional inc .-> board_source_linux
        board_env-mk -. optional inc .-> board_source_uboot
    end
```



# Makefiles target relationship

```mermaid
flowchart BT

    %% ----- Objects -----
    subgraph make[~/Makefile]
        subgraph make_targets[targets]
            make_targets_all[all]
            make_targets_buildroot[buildroot]
            make_targets_depends[depends]
        end
        subgraph make_functions[functions]
            make_functions_buildroot[buildroot]
            make_functions_buildroot-deps[buildroot-depends]
        end
    end

    subgraph scripts[~/scripts]
        subgraph scripts_bash[bash functions]
            scripts_bash_get-source[get-source]
        end
    end

    subgraph board-rv1106g-make[~/board/rv1106g/make]
        subgraph board-rv1106g-make_functions[functions]
            board-rv1106g-make_functions_rv1106-deps[rv1106g-depends]
        end
    end

    subgraph buildroot-dir[~/source/buildroot/Makefile]
        subgraph buildroot-dir_targets[targets]
            buildroot-dir_targets_rv1106config[%_defconfig]
            buildroot-dir_targets_all[all]
        end
    end

    %% ----- Lines -----

    make_targets_all --> make_targets_buildroot
    make_targets_buildroot -- 1st path --> make_targets_depends
    make_targets_depends -- 1st path --> make_functions_buildroot-deps
    make_functions_buildroot-deps -- $ bash --> scripts_bash_get-source
    make_targets_depends -- 2nd path --> board-rv1106g-make_functions_rv1106-deps
    make_targets_buildroot -- 2nd path --> make_functions_buildroot
    make_functions_buildroot -. optional \n $ make .-> buildroot-dir_targets_rv1106config
    make_functions_buildroot -- $ make --> buildroot-dir_targets_all

    %%dependsFunc --> makeenvdep
    %%buildroottarget --> buildrootfunction
    %%buildrootfunction -.-> buildrootdirMakefileTarget

    %%buildrootdir ~~~ makeenv
    
```



# Repo Relationship

```mermaid
flowchart TB

    %% ----- Objects -----
    subgraph rcp-bsp
        rcp-cache
        buildroot
    end
    
    SDK[(arch specific SDK)]
    
    subgraph rcp-library
        subgraph rcp-core
            coreso[.so]
            coreexe[exec]
        end
        subgraph rcp-display
            displayso[.so]
            displayexe[exec]
        end
        subgraph rcp-media
            mediaso[.so]
            mediaexe[exec]
        end
        subgraph rcp-sense
            senseso[.so]
            senseexe[exec]
        end
    end
    
    TACTACAMSDK[(Library SDK)]
    
    subgraph rcp-camera
        vendor-SDK
    end

    BOOTABLE[(Bootable Image)]

    subgraph rcp-mcu-silabs
        gecko-sdk
        rcp-cache-silabs
    end

    MCUIMG[(Bootable MCU Img)]

    %% ----- Lines -----
    rcp-bsp --> SDK
    SDK --> rcp-library
    rcp-library --> TACTACAMSDK
    TACTACAMSDK --> rcp-camera
    rcp-camera --> BOOTABLE
    rcp-mcu-silabs --> MCUIMG
    
```
