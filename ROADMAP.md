# Roadmap

Purpose: PS4 native-port research.

Status: not started. This repository is a private mirror of its upstream; nothing here is published.

## Phases

1. Phase 1: placeholder
2. Phase 2: placeholder
3. Phase 3: placeholder

## Assets

Game assets are never in this repository. They live in a private folder outside the repo (and outside every clone). `.gitignore` guards the usual game file types so none can be added by accident.

## Known issues / to do

- Port the MXCSR guest-thread fix from the uncharted_pc work (our commit 0cf1277 there): guest threads must start with the PS4 MXCSR 0x9fc0 and x87 control word 0x037f. (That commit touches the runtime's guest-thread startup code, setting the FPU state threads start with; the guest's fibers already used the PS4 state. It has an environment override to restore the host default.)
