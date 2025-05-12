Example JupyterLite deployment with `pyodide` and `xeus-cpp`, `xeus-python` and `xeus-r` kernels.
There are multiple environment files, one for each `xeus` kernel.

Deployment available on github pages at https://ianthomas23.github.io/jlite-kernels-multi.

To build and deploy locally:

```bash
micromamba create -f build-environment.yml
micromamba activate jlite-kernels-multi
jupyter lite build --contents contents --output-dir dist --XeusAddon.environment_file=env-cpp.yml --XeusAddon.environment_file=env-python.yml --XeusAddon.environment_file=env-r.yml
npx static-handler dist
```

Running without cross-origin headers (using `npx static-handler dist/`) will use the
service worker for `stdin` requests. This can be confirmed from the Network tab in the
browser's dev tools which will show an `xhr` request to `/api/stdin/kernel` for each
`stdin` request.

Alternatively use SharedArrayBuffer for `stdin` requests by serving with
```bash
npx static-handler --cors --coop --coep --corp dist/
```
and there will be no `xhr` requests in the Network tab.
