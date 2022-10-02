# **TypeScript TDD Project Template**

This template aims to provide a quick setup to start coding on a TypeScript TDD style

![alt text](./public/img/template.png)

---
> ## Steps

### Create folder **ts-tdd-project-template**
```
$ mkdir ts-tdd-project-template && cd $_
```

### Initialize git repository
```
$ git init
```

### Create **.gitignore** file
```
.vscode
node_modules
dist
coverage
data
!src/data
!tests/data
globalConfig.json
```

### Initialize & setup package.json file
```
$ npm init -y
```

```
$ npm pkg delete main
$ npm pkg set author=".luizhp"
$ npm pkg set license="MIT"
```

```
$ git c "chore: initial commit"
```

### Create project folders
```
$ mkdir {src,tests,public}
```

### git-commit-msg-linter
( [npm](https://www.npmjs.com/package/git-commit-msg-linter), [github](https://github.com/legend80s/commit-msg-linter) )

```
$ npm i -D git-commit-msg-linter
```

```
$ git c "chore: add git-commit-msg-linter package"
```

### TypeScript

( [npm](https://www.npmjs.com/package/typescript), [github](https://github.com/Microsoft/TypeScript) )

```
$ npm i -D typescript @types/node
```

Create **tsconfig.json** file
```
{
  "compilerOptions": {
    "outDir": "dist",
    "module": "commonjs",
    "target": "es2019",
    "esModuleInterop": true,
    "sourceMap": true,
    "rootDirs": ["src", "tests"],
    "baseUrl": "src",
    "paths": {
      "@/tests/*": ["../tests/*"],
      "@/*": ["*"]
    }
  },
  "include": ["src", "tests"],
  "exclude": []
}
```

```
$ git c "chore: add typescript package"
```

### eslint-config-standard-with-typescript
( [npm](https://www.npmjs.com/package/eslint-config-standard-with-typescript), [github](https://github.com/standard/eslint-config-standard-with-typescript) )

```
# check updates
$ npm install --save-dev \
  typescript@\* \
  eslint@^8.0.1 \
  eslint-plugin-promise@^6.0.0 \
  eslint-plugin-import@^2.25.2 \
  eslint-plugin-n@^15.0.0 \
  @typescript-eslint/eslint-plugin@^5.0.0 \
  eslint-config-standard-with-typescript@latest
```

* At Visual Studio Code, install and enable [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) extension

* Create **.eslintrc.json** file
```
{
  "extends": "standard-with-typescript",
  "parserOptions": {
    "project": "./tsconfig.json"
  },
  "rules": {
    "@typescript-eslint/strict-boolean-expressions": "off",
    "@typescript-eslint/no-namespace": "off",
    "@typescript-eslint/no-misused-promises": "off",
    "@typescript-eslint/restrict-template-expressions": "off",
    "@typescript-eslint/no-floating-promises": "off"
  }
}
```

* Create **.eslintignore** file
```
node_modules
dist
coverage
./data
requirements
.vscode
```

```
$ git c "chore: add eslint-config-standard-with-typescript package"
```

### lint-staged

( [npm](https://www.npmjs.com/package/lint-staged), [github](https://github.com/okonet/lint-staged) )

```
$ npm i -D lint-staged
```

* Create **.lintstagedrc.json** file
```
{
  "*.ts": [
    "eslint . --fix"
  ]
}
```

```
$ git c "chore: add lint-staged package"
```

### husky

( [npm](https://www.npmjs.com/package/husky), [github](https://github.com/typicode/husky) )

```
$ npm install -D husky
```

* Setup husky hooks
```
$ npx husky install
$ npx husky add .husky/pre-commit "npx lint-staged"
$ npx husky add .husky/commit-msg ".git/hooks/commit-msg \$1"
```

```
$ git c "chore: add husky package"
```

### jest

( [jest](https://jestjs.io/), [npm](https://www.npmjs.com/package/jest), [github](https://github.com/facebook/jest) )

```
$ npm i -D jest @types/jest ts-jest @shelf/jest-mongodb
```

* Create **jest.config.js** file
```
module.exports = {
  roots: ['<rootDir>/tests'],
  collectCoverageFrom: [
    '<rootDir>/src/**/*.ts',
    '!<rootDir>/src/main/**'
  ],
  coverageDirectory: 'coverage',
  coverageProvider: 'babel',
  testEnvironment: 'node',
  preset: '@shelf/jest-mongodb',
  transform: {
    '.+\\.ts$': 'ts-jest'
  },
  moduleNameMapper: {
    '@/tests/(.*)': '<rootDir>/tests/$1',
    '@/(.*)': '<rootDir>/src/$1'
  }
}
```

* Create **jest-mongodb-config.js** file
```
module.exports = {
  mongodbMemoryServer: {
    version: 'latest'
  },
  mongodbMemoryServerOptions: {
    instance: {
      dbName: 'jest'
    },
    binary: {
      version: '4.0.3',
      skipMD5: true
    },
    autoStart: false
  }
}
```

* Create **jest-unit-config.js** file
```
const config = require('./jest.config')
config.testMatch = ['**/*.spec.ts']
module.exports = config
```

* Create **jest-integration-config.js** file
```
const config = require('./jest.config')
config.testMatch = ['**/*.test.ts']
module.exports = config
```

* Set test scripts
```
$ npm pkg set scripts.test="jest --passWithNoTests --runInBand --no-cache --detectOpenHandles --forceExit"
$ npm pkg set scripts.test:unit="npm test -- --watch -c jest-unit-config.js"
$ npm pkg set scripts.test:integration="npm test -- --watch -c jest-integration-config.js"
$ npm pkg set scripts.test:staged="npm test -- --findRelatedTests"
$ npm pkg set scripts.test:ci="npm test -- --coverage"
$ npm pkg set scripts.test:coveralls="npm run test:ci && coveralls < coverage/lcov.info"
```

* Edit **.lintstagedrc.json** file
```
{
  "*.ts": [
    "eslint . --fix",
    "npm run test:staged"
  ]
}
```

```
$ git c "chore: add jest package"
```

### MongoDb

( [npm](https://www.npmjs.com/package/mongodb), [github](https://github.com/mongodb/node-mongodb-native) )

```
$ npm i --save mongodb
$ npm i -D @types/mongodb
```

```
$ git c "chore: add mongodb package"
```

### Scripts
```
$ npm i --save nodemon
$ npm i -D rimraf copyfiles npm-check coveralls
```

```
$ npm pkg set scripts.start="node dist/main/index.js"
$ npm pkg set scripts.debug="nodemon -L --watch ./dist --inspect=0.0.0.0:9222 --nolazy ./dist/main/index.js"
$ npm pkg set scripts.build="rimraf dist && tsc -p tsconfig-build.json"
$ npm pkg set scripts.build:watch="rimraf dist && tsc -p tsconfig-build.json -w"
$ npm pkg set scripts.postbuild="copyfiles -u 1 public/**/* dist/static"
$ npm pkg set scripts.up="npm run build && docker-compose up -d --force-recreate --build"
$ npm pkg set scripts.down="docker-compose down"
$ npm pkg set scripts.check="npm-check -s -u"
```

* Create **tsconfig-build.json** file
```
{
  "extends": "./tsconfig.json",
  "exclude": ["tests"]
}
```

```
$ git c "chore: add scripts"
```

### module-alias

( [npm](https://www.npmjs.com/package/module-alias), [github](https://github.com/ilearnio/module-alias) )

```
$ npm i --save module-alias
```

```
$ npm pkg set _moduleAliases['@']="dist"
```

* Create **/src/main/index.ts** file
```
import 'module-alias/register'
console.log('main >> start')
```

```
$ git c "chore: add module-alias package"
```

### Build & Run
```
$ npm run build
$ node ./dist/index.js
```
