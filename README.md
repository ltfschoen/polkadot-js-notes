* polkadot-js/client
  * polkadot-js/client/scripts
    * WASM WABT (WebAssembly Binary Toolkit) https://github.com/WebAssembly/wabt.git
      * About
        * Use for manipulating WebAssembly files
      * Usage
        * wat2wasm: translate WebAssembly text format .wat to WebAssembly binary format
          * i.e. `wat2wasm <file> -o BASEDIR/.wat/.wasm` using binary tmp/wabt/bin
        * wasm2wat: inverse of above
          * i.e. `wasm2wat <file> -o BASEDIR/.wasm/.wat` using binary tmp/wabt/bin
        * wasm-objdump: print info about wasm binary. Similiar to objdump.
        * wasm-interp: decode and run a WebAssembly binary file using a stack-based interpreter
        * wat-desugar: parse .wat text form as supported by the spec interpreter (s-expressions, flat syntax, or mixed) and print "canonical" flat format
        * wasm2c: convert a WebAssembly binary file to a C source and header
      * Examples
        * WAT Format:
          ```
          (module
            (func $addTwo (param i32 i32) (result i32)
              get_local 0
              get_local 1
              i32.add)
            (export "addTwo" (func $addTwo)))
          ```
          * WAT Text Specification - https://webassembly.github.io/spec/core/text/index.html
            * **TODO** - read it all
        * WASM Binary Format Equivalent
          ```
          0000000: 0061 736d                                 ; WASM_BINARY_MAGIC
          0000004: 0100 0000                                 ; WASM_BINARY_VERSION
          ; section "Type" (1)
          0000008: 01                                        ; section code

          ...
          ```
          * WASM Binary Specification - https://webassembly.github.io/spec/core/binary/index.html
            * **TODO** - read it all
        * JavaScript Format Equivalent
          ```
          const wasmInstance = new WebAssembly.Instance(wasmModule, {});
          const { addTwo } = wasmInstance.exports;
          for (let i = 0; i < 10; i++) {
            console.log(addTwo(i, i));
          }
          ```
      * Demos
        * WAT2WASM Demo - https://cdn.rawgit.com/WebAssembly/wabt/e0719fe0/demo/wat2wasm/index.html
        * WASM2WAT Demo (upload WASM Binary to convert it to WAT Format - https://cdn.rawgit.com/WebAssembly/wabt/e0719fe0/demo/wasm2wat/
        * Reference: https://github.com/WebAssembly/wabt

    * WASM2JS Compiler - compiles WebAssembly module to JS (ASM.js)
      * About
        * Use for compiling to WASM
        * Built using WASM Binaryen https://github.com/WebAssembly/binaryen.git (subset of WASM that compiles down to WASM and includs a code optimiser (minification, coloring, etc)
          * **TODO** - read Binaryen IR section and below in Readme file - https://github.com/WebAssembly/binaryen#binaryen-ir
        * Code - https://github.com/WebAssembly/binaryen/blob/master/src/wasm2js.h
          * **TODO** - read the code!
      * Usage
        * i.e. `wasm2js --input <file> --output <file>`

  * polkadot-js/client/packages/client-wasm
    * WASM Compiler & Call Interface
      * About
        * Wrapper around WebAssembly apps.
        * Extended utility provider around base WebAssembly interfaces, reducing boilerplate.
        * Non-specific to the Polkadot usage.
        * Creates an instance input and provides consistent class with methods exposed. 
      * Usage
        * Update WASM Proxies Runtimes
          * Update dependencies (i.e. WASM Binaryen, WASM WABT) only if not already available in the project root `tmp`
            ```sh
            scripts/polkadot-wasm-build-binaryen.sh scripts/polkadot-wasm-build-wabt.sh
            ```
        * Compile WAT to WASM to JS, including converting Polkadot runtimes to JS-compatible version
          ```sh
          scripts/polkadot-wasm-js-compat.sh
          ```
          * It compiles WAT to WASM output (i.e. `wat2js` is passed .wat files for Polkadot Runtime and Substrate and uses `wat2wasm` to compile it to WASM using binary tmp/wabt/bin, which then uses `wasm2js` function of polkadot-js/client to compile the WASM and creating JS Uint8Array output)
    * **TODO**
      * Review the whole packages/client-wasm directory

  * polkadot-js/client/packages/client
    * Polkadot network client node (POC JavaScript version)
      * About
        * Transpiles TypeScript to JS for all packages with packages/client/scripts/polkadot.js
      * Run Client
        ```
        npm install;
        npm start <options>
        ```
