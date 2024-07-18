# Makefiles relationship

```mermaid
flowchart TB
    subgraph rcp-bsp
        Makefile
    
        subgraph make1[$root/make/]
            make_env[env]
            make_version[version]
            make_custom[custom]
        end
    
        subgraph board[$root/board/rv1106g/make/]
            board_env[env]
            subgraph source[./source/]
                board_buildroot[buildroot]
                board_cache[cache]
                board_linux[linux]
                board_uboot[uboot]
            end
        end
    
        Makefile -- inc --> make_env
        Makefile -- inc--> make_version
        Makefile -. optional inc .-> make_custom
    
        make_env -- inc --> board_env
    
        board_env -- inc --> board_buildroot
        board_env -- inc --> board_cache
        board_env -- inc --> board_linux
        board_env -- inc --> board_uboot
    end
```

# Repo Relationship

```mermaid
flowchart TB
    subgraph rcp-bsp
        rcp-cache
        buildroot
    end

    SDK[(SDK+Rootfs)]
    rcp-bsp --> SDK

    subgraph rcp-library
        rcp-core
        rcp-display
        rcp-media
        rcp-sense
    end

    SDK --> rcp-library

    CPUIMG[(Bootable Image?)]
    rcp-library --> CPUIMG

    subgraph rcp-mcu-silabs
        gecko-sdk
        rcp-cache-silabs
    end

    MCUIMG[(Bootable MCU Img???)]
    rcp-mcu-silabs --> MCUIMG
    
```
