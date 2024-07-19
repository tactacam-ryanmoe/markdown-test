# Makefiles relationship

```mermaid
flowchart TB
    subgraph rcp-bsp
        Makefile
    
        subgraph make1[~/make/]
            make_env[env.mk]
            make_version[version.mk]
            make_custom[custom.mk]
        end

        Makefile -- inc --> make_env
        Makefile -- inc--> make_version
        Makefile -. optional inc .-> make_custom
    
        subgraph board[~/board/rv1106g/make/]
            board_env[env.mk]
            subgraph source[./source/]
                board_buildroot[buildroot.mk]
                board_cache[cache.mk]
                board_linux[linux.mk]
                board_uboot[uboot.mk]
            end
        end
    
        make_env -- inc --> board_env
    
        board_env -. optional inc .-> board_buildroot
        board_env -. optional inc .-> board_cache
        board_env -. optional inc .-> board_linux
        board_env -. optional inc .-> board_uboot
    end
```



# Makefiles target relationship

```mermaid
flowchart BT
    subgraph Makefile[~/Makefile]
        subgraph targets
            all[all]
            buildroottarget[buildroot]
            dependstarget[depends]
        end
        subgraph functions
            buildrootfunction[buildroot]
        end
    end

    

    subgraph makeenv[~/make/env.mk]
        subgraph makeenvfunctions[functions]
            makeenvdefines[rv1106g-depends]
        end
    end

    subgraph buildrootdir[~/source/buildroot/Makefile]
        subgraph buildrootdirMakefileTargets[targets]
            buildrootdirMakefileTarget[rv1106g_debug_defconfig]
        end
    end

    all -.-> buildroottarget
    buildroottarget -.-> dependstarget
    dependstarget --> makeenvdefines
    buildroottarget --> buildrootfunction
    buildrootfunction --> buildrootdirMakefileTarget

    buildrootdir ~~~ makeenv
    
```



# Repo Relationship

```mermaid
flowchart TB

    subgraph rcp-bsp
        rcp-cache
        buildroot
    end

    rcp-bsp --> SDK
    SDK[(arch specific SDK)]
    

    SDK --> rcp-library
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

    rcp-library --> TACTACAMSDK
    TACTACAMSDK[(Tactacam SDK)]

    TACTACAMSDK --> rcp-camera
    subgraph rcp-camera
        vendor-SDK
    end

    rcp-camera --> BOOTABLE
    BOOTABLE[(Bootable Image)]

    subgraph rcp-mcu-silabs
        gecko-sdk
        rcp-cache-silabs
    end

    MCUIMG[(Bootable MCU Img???)]
    rcp-mcu-silabs --> MCUIMG
    
```
