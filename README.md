# DOTMENT Control Center

DOTMENT's console above every brand workspace. One page, no build.

- `index.html` — the whole thing: gate, brands, manage.
- `assets/fonts/` — licensed Gilroy (300–800 + italics) and IBM Plex Mono.
- `assets/logos/` — brand logos. Add a file and point the brand's `logo` at it in the seed.
- `assets/favicon.*` — the DOTMENT mark.

Run it over http, never `file://`, or the sign-in cookie will not persist:

```
python3 -m http.server 8000
```

State lives in the browser: one `localStorage` key (`dotment-control-center`) for the data, and sign-in kept in both a cookie (`dcc_session`) and a storage flag, so it survives a refresh even when opened as a file. The seed loads on first visit only. To reset, clear site data for the origin.

The gate password and the brand list sit at the top of the script in `index.html` (`PASSWORD` and `seed()`).
