# webpack-issue-mangling

This is a repo case for the webpack issue reported here: https://github.com/webpack/webpack/issues/19153

The issue is that mangling of the export names get disabled for a module if there is a module that imports/exports the module's exports without referencing the exports explicitly.  This repro case shows both the mangled exports and unmangled export side-by-side for equivalent modules.

## How to run
Simply do:

* npm install
* npm run build

## Test case description
After building, look at the output chunk `./dist/main.js` and look at the references to `ENUM1` through `ENUM8`.  See how the first 4 are mangled properly but the last 4 are referenced by their full names.

The enums are defined in exactly the same way in `Enums.ts` and `Enums2.ts`.  You can see in `Consumer.ts` that all enum values from both modules are referenced in exactly the same way.  The only difference is `Consolidator.ts` where `Enums2` is not consumed directly, but instead is consumed via a function in `Provider.ts` and then exported without referencing the enums.  The presence of this `Consolidator.ts` module causes webpack to disable mangling for `Enum2.ts`, which is the issue.


