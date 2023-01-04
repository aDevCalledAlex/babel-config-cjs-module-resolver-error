# Babel module-resolver plugin cjs file extension error with Expo

Issue:
Babel module-resolver does not resolve configured path alias when file extension is cjs, giving error message "Unable to resolve [alias/component] from [importing file]".

Solution:
Rename babel.config.cjs to babel.config.js

Expect:
babel.config file extension to have no effect on whether module-resolver works, given the config file is valid with respect to its file extension.

Reproduction steps:
Steps to reproduce this example project:

1. Create an expo app with `npx create-expo-app`
2. Rename babel.config.js to babel.config.cjs
3. Add the following property to babel.config return value, and replace the comments:

```js
plugins: [
  [
    "module-resolver",
    {
      alias: {
        /* alias name as valid JS object key */: /* relative path to directory from root as string */,
      },
    },
  ],
],
```

4. Create file in the path specified in the alias key of the module-resolver
5. Import module with path alias in App.js
6. Run `npm run start -- -c`
   - Note: I always run expo on tunnel mode so my command is `npm run start -- --tunnel -c`, but I expect no difference with this
7. Expect Metro bundle failure with error message "Unable to resolve [alias/component] from [App.js]"

Steps to fix error:

1. Rename babel.config.cjs to babel.config.js
2. Run `npm run start -- -c`
3. Expect Metro to bundle successfully

I have not tested to see if this issue happens when

1. not using expo
2. not using expo tunnel mode
3. using other babel plugins
4. using other valid babel.config file extensions
