# 🖇 Parsing gyro data

Parsing of all gyro formats for Gyroflow is done in the [telemetry-parser ](https://github.com/AdrianEddy/telemetry-parser)library. It was created specifically for Gyroflow and aims to implement all possible gyro data parsers **natively.**

This means that the code is pure Rust, without **any** external dependencies, any SDKs or anything else.

This allows it to be extremely portable with the ability to be built as a [python module](https://pypi.org/project/telemetry-parser/), [WASM module](https://github.com/AdrianEddy/telemetry-parser/tree/master/bin/wasm-module), for all mobile platforms and basically everywhere else Rust is supported.

It also doesn't need filesystem access, so the source data bytes can be provided from anywhere.

## CLI tool

telemetry-parser provides a CLI tool called [gyro2bb](https://github.com/AdrianEddy/telemetry-parser/releases), which parses the gyro data and outputs a Betaflight blackbox compatible `.csv` file.

It can also dump (print) all parsed metadata to the terminal.

## Gyroflow's gyro formats

Gyroflow team has designed two formats in an attempt to standardize writing gyro data by the cameras.

1. [GCSV format](gcsv-format.md) is a simple CSV-based .gcsv text file
2. [Gyroflow protobuf](gyroflow-protobuf.md) is a binary protobuf description covering all possible camera features

Both formats are parsed by the `telemetry-parser` library.

