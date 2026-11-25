CC     ?= gcc
CFLAGS  = -std=c11 -Wall -Wextra -O2 -D_DEFAULT_SOURCE
SRC     = algorithms.c wasm_api.c

all: trackx_cli libtrackx.so

trackx_cli: $(SRC) main.c
	$(CC) $(CFLAGS) $(SRC) main.c -o $@

libtrackx.so: $(SRC)
	$(CC) $(CFLAGS) -fPIC -shared $(SRC) -o $@

native: trackx_cli
	./trackx_cli --demo

test: test_algorithms
	./test_algorithms

test_algorithms: $(SRC) test_algorithms.c
	$(CC) $(CFLAGS) $(SRC) test_algorithms.c -o $@

# Optional: needs Emscripten (emcc). Not required for the normal run.
wasm:
	emcc -std=c11 -O2 algorithms.c wasm_api.c -o ../frontend/trackx.js -s MODULARIZE=1 -s EXPORT_NAME=TrackXModule \
	  -s EXPORTED_FUNCTIONS=_trackx_run,_trackx_free -s EXPORTED_RUNTIME_METHODS=ccall,UTF8ToString

clean:
	rm -f trackx_cli libtrackx.so test_algorithms

.PHONY: all native test wasm clean
