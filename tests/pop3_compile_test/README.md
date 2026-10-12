# Population III compile test

Build and run the local C++ Pop III variant using the shared physics helpers
and tables from this repository. Requires a full checkout, Make, and a C++17
compiler such as `g++`.

## Download the inputs

The following datasets are excluded from Git because of their size.
**Contact the repository maintainer to request download links**, then place
all three files in `tests/pop3_compile_test/` with these exact names:

| File | Approximate size |
| --- | ---: |
| `data_1e-11.dat` | 13 MiB |
| `spectrum1e-11.txt` | 135 MiB |
| `spectrum_cleaned00000000001.txt` | 27 MiB |

Compilation works without these files; `make run` requires them and checks
that they exist before starting.

## Build and run

From the repository root:

```bash
cd tests/pop3_compile_test
make clean && make
make run
```

`make` builds the local `BPOP.x`. Use `make run` for execution: it creates a
fresh output directory, protecting the input files from the simulator's bulk
file moves. Avoid running `./BPOP.x` directly in this input directory.

## Configuration

Edit `input_params_p3.h`, then run `make` to recompile. Defaults are 1,000
sources at metallicity `1e-11`, Fuller spins, and disabled chunking, upper
mass-gap sampling, and BH seeds. Other model settings retain their supplied
values.

Paths are generated automatically from the checkout location. After moving
the checkout or changing compiler flags, use `make clean && make`. To select
a compiler explicitly, use `make CXX=g++`.

## Outputs

Each run prints its directory: `../../tmp/pop3-run.XXXXXX/`.

- `Output.txt`: execution log.
- `SIM_*/`: merger catalogues and retention table.
- `BPOP.x` and both configuration headers: snapshots used for the run.

`make clean` removes local build artifacts while preserving datasets and runs.

## Known limitations

- The supplied mixed spectrum has an incomplete final row. The loader warns
  and discards it; other malformed or non-finite input stops execution.
- Dissolved clusters can produce infinite semimajor axes in
  `Larger_than_tH.txt`. Check results before scientific use.
- Python rate postprocessing remains unfinished: the local script has syntax,
  indentation, import, and path issues. `make run` executes only C++.
