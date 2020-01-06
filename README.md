# For reproduce docker-compose error

## Follow steps below can reproduce

```bash
git clone https://github.com/taoabc/reproduce-verdaccio-error.git
cd reproduce-verdaccio-error
chmod -R 777 storage # or chown 10001:65535
docker-compose up
```

will show errors like this
```
Creating verdaccio-test ... done
Attaching to verdaccio-test
verdaccio-test |  warn --- config file  - /verdaccio/conf/config.yaml
verdaccio-test |  warn --- Verdaccio started
verdaccio-test | (node:8) UnhandledPromiseRejectionWarning: TypeError: Converting circular structure to JSON
verdaccio-test |     --> starting at object with constructor 'Timeout'
verdaccio-test |     |     property '_idlePrev' -> object with constructor 'TimersList'
verdaccio-test |     --- property '_idleNext' closes the circle
verdaccio-test |     at JSON.stringify (<anonymous>)
verdaccio-test |     at new LocalDatabase (/opt/verdaccio/node_modules/@verdaccio/local-storage/lib/local-database.js:65:20)
verdaccio-test |     at LocalStorage._loadStorage (/opt/verdaccio/build/lib/local-storage.js:842:14)
verdaccio-test |     at new LocalStorage (/opt/verdaccio/build/lib/local-storage.js:47:31)
verdaccio-test |     at Storage.init (/opt/verdaccio/build/lib/storage.js:64:25)
verdaccio-test |     at _default (/opt/verdaccio/build/api/index.js:126:17)
verdaccio-test |     at startVerdaccio (/opt/verdaccio/build/lib/bootstrap.js:60:22)
verdaccio-test |     at init (/opt/verdaccio/build/lib/cli.js:78:35)
verdaccio-test |     at Object.<anonymous> (/opt/verdaccio/build/lib/cli.js:111:3)
verdaccio-test |     at Module._compile (internal/modules/cjs/loader.js:959:30)
verdaccio-test | (node:8) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled with .catch(). (rejection id: 2)
verdaccio-test | (node:8) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.
verdaccio-test |  fatal--- uncaught exception, please report this
verdaccio-test | Error: ENOENT: no such file or directory, open '/verdaccio/storage/logs/log.jsonl'
verdaccio-test exited with code 255
```

## **BUT**

Remove `config/config.yaml` line 30, everything will be OK.
```yaml
- { type: rotating-file, format: json, path: /verdaccio/storage/logs/log.jsonl, level: http, options: {period: 1d} }
```