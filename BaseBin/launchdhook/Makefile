TARGET = launchdhook.dylib
CC = clang

CFLAGS = -framework Foundation -framework CoreServices -framework Security -I../.include -Isrc -I../_external/modules/litehook/src -isysroot $(shell xcrun --sdk iphoneos --show-sdk-path) -arch arm64
LDFLAGS = -dynamiclib -rpath @loader_path/fallback -arch arm64 -L../.build -L../_external/lib -ljailbreak -lbsm -lchoma -lMobileGestalt

.PHONY: sign clean
.PRECIOUS: $(TARGET)

sign: $(TARGET)
	@ldid -Cadhoc -S $^

# Ensure dependent libraries are built before linking
$(TARGET): | ../.build ../_external/lib
$(TARGET): $(wildcard src/*.m src/*.c src/jbserver/*.c ../systemhook/src/common/*.[cSm] ../systemhook/src/envbuf.c ../_external/modules/litehook/src/*.c)
	$(CC) $(CFLAGS) -o $@ $^ $(LDFLAGS)

../.build:
	@mkdir -p $@

../_external/lib:
	@mkdir -p $@

clean:
	@rm -f $(TARGET)
