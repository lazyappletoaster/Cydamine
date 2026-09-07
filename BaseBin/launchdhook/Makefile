TARGET = launchdhook.dylib
CC = clang

CFLAGS = -framework Foundation -framework CoreServices -framework Security -I../.include -Isrc -I../_external/modules/litehook/src -isysroot $(shell xcrun --sdk iphoneos --show-sdk-path) -arch arm64
LDFLAGS = -dynamiclib -rpath @loader_path/fallback -arch arm64 -L../.build -L../_external/lib -ljailbreak -lbsm -lchoma -lMobileGestalt -lcydia_substrate

SOURCES = $(wildcard src/*.m src/*.c src/jbserver/*.c ../systemhook/src/common/*.[cSm] ../systemhook/src/envbuf.c ../_external/modules/litehook/src/*.c)

.PHONY: sign clean
.PRECIOUS: $(TARGET)

sign: $(TARGET)
	@ldid -Cadhoc -S $^

$(TARGET): $(SOURCES) | ../.build ../_external/lib
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ $(SOURCES)

../.build:
	@mkdir -p $@

../_external/lib:
	@mkdir -p $@

clean:
	@rm -f $(TARGET)
