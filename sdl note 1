import 'dart:io';

import 'package:sdl_2/sdl2.dart';
import 'dart:ffi' as ffi;
import 'package:ffi/ffi.dart';

void main(List<String> arguments) async {
  final library = ffi.DynamicLibrary.open('bin/libSDL2-2.0.so.0.12.0');
  final sdl2 = SDL2(library);
  ffi.Pointer<SDL_Window> window;
  ffi.Pointer<SDL_Surface> screenSurface;
  ffi.Pointer<SDL_Surface> helloWorld;

  final initResult = sdl2.SDL_Init(SDL_INIT_VIDEO);
  if (initResult < 0) {
    print('SDL could not initialize! SDL_Error: ${Utf8.fromUtf8(sdl2.SDL_GetError().cast())}');
    exit(1);
  }

  window = sdl2.SDL_CreateWindow(
    Utf8.toUtf8('DartUp').cast(),
    SDL_WINDOWPOS_UNDEFINED,
    SDL_WINDOWPOS_UNDEFINED,
    800,
    600,
    SDL_WindowFlags.SDL_WINDOW_SHOWN,
  );
  if (window == ffi.nullptr) {
    print('Window could not be created! SDL_Error: ${Utf8.fromUtf8(sdl2.SDL_GetError().cast())}');
    exit(1);
  }

  screenSurface = sdl2.SDL_GetWindowSurface(window);

  helloWorld = loadBMP(sdl2, 'assets/Dartup.bmp');
  if (helloWorld == ffi.nullptr) {
    print('Unable to load image! SDL Error: ${Utf8.fromUtf8(sdl2.SDL_GetError().cast())}');
    exit(1);
  }

  sdl2.SDL_UpperBlit(helloWorld, ffi.nullptr, screenSurface, ffi.nullptr);
  sdl2.SDL_UpdateWindowSurface(window);

  sdl2.SDL_Delay(2000);

  await ProcessSignal.sigint.watch().first;

  sdl2.SDL_FreeSurface(helloWorld);
  helloWorld = ffi.nullptr;

  //Destroy window
  sdl2.SDL_DestroyWindow(window);
  window = ffi.nullptr;

  //Quit SDL subsystems
  sdl2.SDL_Quit();
  exit(0);
}

ffi.Pointer<SDL_Surface> loadBMP(SDL2 sdl2, String path) {
  final rwOps = sdl2.SDL_RWFromFile(Utf8.toUtf8(path).cast(), Utf8.toUtf8('rb').cast());
  return sdl2.SDL_LoadBMP_RW(rwOps, 1);
}
