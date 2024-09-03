**Workflow**

1. Build the microservice Rust source code (Cargo project) into Wasm bytecode. Run the following command in the example project directory.

```
$ cargo build --target wasm32-wasi --release
```

2. Optional: Use the WasmEdge Ahead-of-Time (AOT) compiler to compile the Wasm app to native machine code. You can skip this step and everything would still work. Compiling to AOT will help improve performance, however. Run the following command in the example project directory:

```
$ wasmedge compile target/wasm32-wasi/release/sales_tax_rate_lookup.wasm  sales_tax_rate_lookup.wasm
```

3. Run the Wasm application in WadmEdge. The Wasm file could be the one compiled by Cargo or the one after AOT compilation. It starts a server that listens at port 8001.

```
$ wasmedge target/wasm32-wasi/release/sales_tax_rate_lookup.wasm
```

4. Query the microservice with a zip code. Run the following curl command to query for a zip code.

```
$ curl http://localhost:8001/find_rate -X POST -d "78701"
```

5. The CSV file in the source code directory contains the mapping of US zip codes to sales tax rates. The CSV file was read and parsed at the compile time, and was incorporated into the Wasm bytecode program. The included CSV file only contains a small number of zip codes. If the zip code is not in the source CSV file, the microservice will return a default tax rate of 0.08.

```
$ curl http://localhost:8001/find_rate -X POST -d "12345"
```

